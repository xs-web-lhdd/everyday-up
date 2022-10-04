### 5、vitepress 如何做预解析：（只有在生产环境下才进行预解析，在源码中 client/app/index.ts/ VitePressApp 组件的 setup 函数里面）
> vitepress 进行 预解析 的操作,参照了 https://github.com/GoogleChromeLabs/quicklink 这个库,建议先看这个库，看完就很明白了。

###### 作用：初始化后，将利用空闲时间（requestIdleCallback）自动预解析位于视口中（IntersectionObserver）的链接的 URL

执行预解析的是通过 创建一个 link 标签，进行预解析，然后插入到 head 标签中，
```js
const viaDOM = (url: string) => {
  const link = createLink()
  // 表明当前文档和外部资源的关系
  link.rel = `prefetch`
  link.href = url
  document.head.appendChild(link)
}
```
如果 link 不支持 `prefetch` 那么就 创建一个 get 请求，并且发送，由于浏览器的机制，这样会自动解析和缓存，
```js
const viaXHR = (url: string) => {
  const req = new XMLHttpRequest()
  req.open('GET', url, (req.withCredentials = true))
  req.send()
}
```

##### 做预解析的流程：
1、通过判断是不是浏览器和支不支持 `window.IntersectionObserver`，不是或不支持就返回
2、检查用户的连接速度是否慢（使用 ）或是否启用了数据保护程序`navigator.connection.effectiveTypenavigator.connection.saveData`，如果是就返回
3、在 `onMounted` 钩子函数里面执行 `observeLinks`（这个函数就是对全部 a 标签属于 同站 并且是 html 进行 预解析,这里利用 requestIdleCallback 会自动优化,是个不错的选择,然后用 IntersectionObserver 进行观察,一旦交叉,直接调用 doFetch 进行预解析,会把解析的 url 的路径扔到 已经解析过的 Set 集合里面(hasFetched 里面防止页面间跳转再进行 重复 解析)）
4、创建路由实例，然后通过 watch 监听路由变化，一旦路有变化，直接重新执行 `observeLinks` 进行预解析
5、在 `onUnmounted` 时销毁之前创建的 `IntersectionObserver` 的实例，防止内存泄露


`observeLinks` 代码细节及其作用：
```js
  // 为什么选择 requestIdleCallback 因为它会根据事件循环的忙碌情况进行有效的执行操作,不容易影响其他操作
  const rIC = window.requestIdleCallback || setTimeout
  let observer: IntersectionObserver | null = null

  // 这个函数就是对全部 a 标签属于同站并且是 html 进行 预解析,这里利用 requestIdleCallback 会自动优化,是个不错的选择,然后用 IntersectionObserver 进行观察,一旦交叉,直接调用 doFetch 进行预解析,会把解析的 url 的路径扔到 已经解析过的 Set 集合里面(hasFetched 里面防止页面间跳转再进行 重复 解析)
  const observeLinks = () => {
    // 阻止IntersectionObserver对象观察任何目标，相当于取消之前的观察
    if (observer) {
      observer.disconnect()
    }

    // IntersectionObserver() 创建一个新IntersectionObserver对象，该对象将在检测到目标元素的可见性已超过一个或多个阈值时执行指定的回调函数。
    observer = new IntersectionObserver((entries) => {
      entries.forEach((entry) => {
        // 判断是不是相交
        if (entry.isIntersecting) {
          // 相交就去掉对 对应 link 的观察(TODO: 感觉就是这里进行了与解析操作)
          const link = entry.target as HTMLAnchorElement
          observer!.unobserve(link)
          const { pathname } = link
          if (!hasFetched.has(pathname)) {
            hasFetched.add(pathname)
            const pageChunkPath = pathToFile(pathname)
            doFetch(pageChunkPath)
          }
        }
      })
    })

    rIC(() => {
      // 查询 id=app 下的每一个 a 标签
      document.querySelectorAll<HTMLAnchorElement>('#app a').forEach((link) => {
        // 拿出 链接的位置（新窗口，子窗口，自身窗口）、链接的主机的地址（本地启动时 127.0.0.1）、链接地址的路径（href 的值端口后的值）
        const { target, hostname, pathname } = link

        // href 是 www.baidu.com 时，extMatch 是 ['.com', index: 10, input: '/www.baidu.com', groups: undefined]
        const extMatch = pathname.match(/\.\w+$/)
        // 只对 html 进行预解析,因为可能会跳转到该页面
        if (extMatch && extMatch[0] !== '.html') {
          return
        }

        if (
          // only prefetch same tab navigation, since a new tab will load
          // the lean js chunk instead.
          target !== `_blank` &&
          // only prefetch inbound links
          // 只预解析入站(同站)链接
          // 判断目标链接的主机和当前站点链接的主机是否一致，一致就是同一个站点
          hostname === location.hostname
        ) {
          // 如果目标链接的路径部分不等于当前站点链接的路径
          if (pathname !== location.pathname) {
            // 告诉IntersectionObserver要观察的目标元素。
            observer!.observe(link)
          } else {
            // No need to prefetch chunk for the current page, but also mark
            // it as already fetched. This is because the initial page uses its
            // lean chunk, and if we don't mark it, navigation to another page
            // with a link back to the first page will fetch its full chunk
            // which isn't needed.

            // 即使当前页面的链接的目标 url 和当前页面的 url 一摸一样,那么就做一个标记,防止进入新页面后,新页面有 a 标签并且 herf 值是当前页面的 url 那么就不需要再进行预解析了
            hasFetched.add(pathname)
          }
        }
      })
    })
  }
```


##### vitepress 中预解析的完整代码：
```js
/**
 * @description 该文件是 vitepress 进行 预解析 的操作,参照了 https://github.com/GoogleChromeLabs/quicklink 这个库,建议一看,看完再看下面代码就简单很多了
 */

// Customized pre-fetch for page chunks based on
// https://github.com/GoogleChromeLabs/quicklink

import { useRoute } from '../router.js'
import { onMounted, onUnmounted, watch } from 'vue'
import { inBrowser, pathToFile } from '../utils.js'

// 设置一个全局的 Set 对象,保存已经预解析过的 link 或者 url
const hasFetched = new Set<string>()
const createLink = () => document.createElement('link')

// 创建一个 link 标签，进行预解析，然后插入到 head 标签中
const viaDOM = (url: string) => {
  const link = createLink()
  // 表明当前文档和外部资源的关系
  link.rel = `prefetch`
  link.href = url
  document.head.appendChild(link)
}

// 创建一个 get 请求，并且发送，这样会自动解析和缓存
const viaXHR = (url: string) => {
  const req = new XMLHttpRequest()
  req.open('GET', url, (req.withCredentials = true))
  req.send()
}

let link
// TODO: !真正进行预解析的操作: 如果浏览器支持 prefetch ，那么就用 link 中的预解析，否则就发送一个 get 请求，这样浏览器会自动进行 url 解析和缓存
const doFetch: (url: string) => void =
  inBrowser &&
    (link = createLink()) &&
    link.relList &&
    link.relList.supports &&
    link.relList.supports('prefetch')
    ? viaDOM
    : viaXHR

export function usePrefetch() {
  if (!inBrowser) {
    return
  }

  if (!window.IntersectionObserver) {
    return
  }

  let conn
  // Navigator.connection 是只读的，提供一个 NetworkInformation 对象来获取设备的网络连接信息。例如用户设备的当前带宽或连接是否被计量，这可以用于基于用户的连接来选择高清晰度内容或低清晰度内容。
  if (
    (conn = (navigator as any).connection) &&
    // 它应该被用来减少发送到客户端的数据（saveData的作用） 和 判断是不是 2G 网
    // 检查用户的连接速度是否慢（使用 ）或是否启用了数据保护程序
    (conn.saveData || /2g/.test(conn.effectiveType))
  ) {
    // Don't prefetch if using 2G or if Save-Data is enabled.
    return
  }

  // 为什么选择 requestIdleCallback 因为它会根据事件循环的忙碌情况进行有效的执行操作,不容易影响其他操作
  const rIC = window.requestIdleCallback || setTimeout
  let observer: IntersectionObserver | null = null

  // 这个函数就是对全部 a 标签属于同站并且是 html 进行 预解析,这里利用 requestIdleCallback 会自动优化,是个不错的选择,然后用 IntersectionObserver 进行观察,一旦交叉,直接调用 doFetch 进行预解析,会把解析的 url 的路径扔到 已经解析过的 Set 集合里面(hasFetched 里面防止页面间跳转再进行 重复 解析)
  const observeLinks = () => {
    // 阻止IntersectionObserver对象观察任何目标，相当于取消之前的观察
    if (observer) {
      observer.disconnect()
    }

    // IntersectionObserver() 创建一个新IntersectionObserver对象，该对象将在检测到目标元素的可见性已超过一个或多个阈值时执行指定的回调函数。
    observer = new IntersectionObserver((entries) => {
      entries.forEach((entry) => {
        // 判断是不是相交
        if (entry.isIntersecting) {
          // 相交就去掉对 对应 link 的观察(TODO: 感觉就是这里进行了与解析操作)
          const link = entry.target as HTMLAnchorElement
          observer!.unobserve(link)
          const { pathname } = link
          if (!hasFetched.has(pathname)) {
            hasFetched.add(pathname)
            const pageChunkPath = pathToFile(pathname)
            doFetch(pageChunkPath)
          }
        }
      })
    })

    rIC(() => {
      // 查询 id=app 下的每一个 a 标签
      document.querySelectorAll<HTMLAnchorElement>('#app a').forEach((link) => {
        // 拿出 链接的位置（新窗口，子窗口，自身窗口）、链接的主机的地址（本地启动时 127.0.0.1）、链接地址的路径（href 的值端口后的值）
        const { target, hostname, pathname } = link

        // href 是 www.baidu.com 时，extMatch 是 ['.com', index: 10, input: '/www.baidu.com', groups: undefined]
        const extMatch = pathname.match(/\.\w+$/)
        // 只对 html 进行预解析,因为可能会跳转到该页面
        if (extMatch && extMatch[0] !== '.html') {
          return
        }

        if (
          // only prefetch same tab navigation, since a new tab will load
          // the lean js chunk instead.
          target !== `_blank` &&
          // only prefetch inbound links
          // 只预解析入站(同站)链接
          // 判断目标链接的主机和当前站点链接的主机是否一致，一致就是同一个站点
          hostname === location.hostname
        ) {
          // 如果目标链接的路径部分不等于当前站点链接的路径
          if (pathname !== location.pathname) {
            // 告诉IntersectionObserver要观察的目标元素。
            observer!.observe(link)
          } else {
            // No need to prefetch chunk for the current page, but also mark
            // it as already fetched. This is because the initial page uses its
            // lean chunk, and if we don't mark it, navigation to another page
            // with a link back to the first page will fetch its full chunk
            // which isn't needed.

            // 即使当前页面的链接的目标 url 和当前页面的 url 一摸一样,那么就做一个标记,防止进入新页面后,新页面有 a 标签并且 herf 值是当前页面的 url 那么就不需要再进行预解析了
            hasFetched.add(pathname)
          }
        }
      })
    })
  }

  onMounted(observeLinks)

  const route = useRoute()
  // 监听路由，改变就重新预解析
  watch(() => route.path, observeLinks)

  onUnmounted(() => {
    observer && observer.disconnect()
  })
}
```