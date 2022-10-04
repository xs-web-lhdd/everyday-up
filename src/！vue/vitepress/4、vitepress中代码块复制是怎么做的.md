### vitepress 中代码块复制是怎么做的？
源码的位置在 src/client/app/composables/copyCode.ts 里面

**源码:**
```ts
import { inBrowser } from '../utils.js'

export function useCopyCode() {
  if (inBrowser) {
    window.addEventListener('click', (e) => {
      const el = e.target as HTMLElement
      // 如果点击的按钮是语言块中的 copy 按钮
      if (el.matches('div[class*="language-"] > button.copy')) {
        // 拿到父元素
        const parent = el.parentElement
        // 拿到下一个兄弟元素的下一个兄弟元素
        const sibling = el.nextElementSibling
          ?.nextElementSibling as HTMLPreElement | null
        if (!parent || !sibling) {
          return
        }

        // 判断是不是 shell 脚本
        const isShell = /language-(shellscript|shell|bash|sh|zsh)/.test(
          parent.classList.toString()
        )

        // 这个 text 就是语言块中的内容
        let { innerText: text = '' } = sibling

        if (isShell) {
          text = text.replace(/^ *(\$|>) /gm, '')
        }

        copyToClipboard(text).then(() => {
          // 拷贝完之后改变 copy 的图标
          el.classList.add('copied')
          // 两秒后恢复之前的 copy 图标
          setTimeout(() => {
            el.classList.remove('copied')
            el.blur()
          }, 2000)
        })
      }
    })
  }
}

async function copyToClipboard(text: string) {
  try {
    // 把 text 复制到粘贴板上
    return navigator.clipboard.writeText(text)
  } catch {
    // 如果不兼容 navigator.clipboard 采用下面的方式进行复制

    const element = document.createElement('textarea')
    // 拿到之前焦点的元素: document.activeElement 是用来获取当前页面的焦点元素
    const previouslyFocusedElement = document.activeElement

    element.value = text

    // Prevent keyboard from showing on mobile
    // 防止键盘显示在手机上 ( 表单设置 readonly 属性后就不能对表单进行编辑,自然就不会有键盘出现)
    element.setAttribute('readonly', '')

    element.style.contain = 'strict'
    element.style.position = 'absolute'
    element.style.left = '-9999px'
    // 防止iOS上的缩放
    element.style.fontSize = '12pt' // Prevent zooming on iOS

    // 返回一个 Selection 对象，表示用户选择的文本范围或光标的当前位置。
    const selection = document.getSelection()
    const originalRange = selection
      ? selection.rangeCount > 0 && selection.getRangeAt(0)
      : null

    document.body.appendChild(element)
    // 选中这个 textarea 也就是相当于获取焦点
    element.select()

    // Explicit selection workaround for iOS
    // iOS的显式选择方法
    // 用来确定复制内容的开始位置和结束位置
    element.selectionStart = 0
    element.selectionEnd = text.length

    // 该方法允许运行命令来操纵可编辑内容区域的元素。 这里是执行 copy 命令
    document.execCommand('copy')
    document.body.removeChild(element)

    if (originalRange) {
      // 当选择是错误的时候，originalRange不可能是真的
      selection!.removeAllRanges() // originalRange can't be truthy when selection is falsy
      selection!.addRange(originalRange)
    }

    // Get the focus back on the previously focused element, if any
    // 让焦点回到之前聚焦的元素上(如果有的话)
    if (previouslyFocusedElement) {
      ; (previouslyFocusedElement as HTMLElement).focus()
    }
  }
}
```

整体流程：
如果是浏览器环境下就给 window 绑定一个点击事件（为了后面事件委托），然后当页面内发生点击时会事件冒泡到 window 上面，然后会判断，被实际点击的元素是不是语言代码块中的 copy 按钮，如果是会拿到 copy按钮的父节点，和下下个兄弟节点（为什么是下下个呢？我的理解是这是 vitepress 的语言代码块的固定格式），然后通过 `let { innerText: text = '' } = sibling` 取出代码块的内容（这里我写了个demo可以看一下）,接着判断是不是 shell 代码，如果是 那么进行一个正则的替换，去掉一些字符串，接着通过 `copyToClipboard` 去执行`复制`操作，然后在复制成功后将 copy 按按钮的class里面增加 copied ，然后再两秒后去掉 copied

`copyToClipboard 函数流程`：
先尝试通过浏览器原生 API `navigator.clipboard.writeText(text)` 进行复制，如果报错就用封装的代码进行复制。

封装复制功能代码解析：
先创建一个 `textarea` 用来存放准备复制的内容，然后拿到当前页面获得焦点的元素（因为后续复制需要获得焦点，所以先存一下，等获得焦点结束后再恢复之前的焦点），然后给 `textarea` 设置只读属性，这样就不会在移动端设备上弹出虚拟键盘（正常的表单点击时会弹出虚拟键盘，设置只读后就不会弹出了），然后将 `textarea` 通过绝对定位消失在用户的视野里面，创建 选择对象 `document.getSelection`,接着确定选择的起始和结束位置，然后通过一个快废弃的 API 去复制`document.execCommand('copy') 该方法允许运行命令来操纵可编辑内容区域的元素。 这里是执行 copy 命令`，然后删除元素，将之前的焦点元素恢复焦点即可。