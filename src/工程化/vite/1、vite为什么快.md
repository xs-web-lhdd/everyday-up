### 1、vite为什么快？
[个人觉得官网说的也比较好](https://cn.vitejs.dev/guide/why.html)
[为什么快](https://mp.weixin.qq.com/s/HGnJg24Zk2hGdvnvPBWt-g)
首先 vite 的快主要体现在两方面：
- 快速的冷启动
- 快速的热更新

##### 快速的冷启动？
采用 `unbundle` 的机制： 不需要做 bundle 操作，即不需要构建、分解 module graph，源文件之间的依赖关系完全通过浏览器对 ESM 规范的支持来解析。这就使得 dev server 在启动过程中只需做一些初始化的工作，剩下的完全由浏览器支持。

unbundle 机制的核心:
- 模块之间的依赖关系的解析由浏览器实现；
- 文件的转换由 dev server 的 middlewares 实现并做缓存；
- 不对源文件做合并捆绑操作；



vite 的慢：
- 首屏
- 懒加载

##### 首屏：
webpack：
  浏览器向 dev server 发起请求， dev server 接受到请求，然后将已经打包构建好的首屏内容发送给浏览器。整个过程非常普遍，没有什么可说的，不存在什么性能问题。

vite:
  和 `Webpack` 对比，Vite 把需要在 dev server 启动过程中完成的工作，转移到了 dev server `响应浏览器请求的过程`中，不可避免的导致首屏性能下降。

  不过首屏性能差只发生在 dev server 启动以后第一次加载页面时发生。之后再 reload 页面时，首屏性能会好很多。原因是 dev server 会将之前已经完成转换的内容缓存起来。

首屏慢的核心原因：
  - 不对源文件做合并捆绑操作，导致大量的 http 请求；
  - dev server 运行期间对源文件做 resolve、load、transform、parse 操作；
  - 预构建、二次预构建操作也会阻塞首屏请求，直到预构建完成为止；

##### 懒加载：
和首屏一样，由于 unbundle 机制，动态加载的文件，需要做 resolve、load、transform、parse 操作，并且还有大量的 http 请求，导致懒加载性能也受到影响。

此外，如果懒加载过程中，发生了二次预构建，页面会 reload，对开发体验也有一定程度的影响。


##### 为什么 vite 要进行预构建？
[vite的预构建](https://blog.csdn.net/CRMEB/article/details/123750395)
- vite 引入了预构建机制。在预构建工具的选择上，vite选择了 esbuild 。esbuild 使用 Go 编写，比以 JavaScript 编写的打包器构建速度快 10-100 倍，有了预构建，再利用浏览器的esm方式按需加载业务代码，动态实时进行构建，结合缓存机制，大大提升了服务器的启动速度。

- 来自node_modules中的模块依赖是需要预构建的。例如import ElementPlus from ‘element-plus’。因为在浏览器环境下，是不支持这种裸模块引用的（bare import）。另一方面，如果不进行构建，浏览器面对由成百上千的子模块组成的依赖，依靠原生esm的加载机制，每个的依赖的import都将产生一次http请求。面对大量的请求，浏览器是吃不消的。因此客观上需要对裸模块引入进行打包，并处理成浏览器环境下支持的相对路径或路径的导入方式。例如：import ElementPlus from ‘/path/to/.vite/element-plus/es/index.mjs’。

- vite冷启动之所以快，除了esbuild本身构建速度够快外，也与vite做了必要的缓存机制密不可分。vite在预构建时，除了生成预构建的js文件外，还会创建一个_metadata.json文件


预构建流程：

总结起来就是先查找需要预构建的依赖，然后将这些依赖作为entryPoints进行构建，构建完成后更新缓存。vite在启动时为提升速度，会检查缓存是否有效，有效的话就可以跳过预构建环节，缓存是否有效的判定是对比缓存中的hash值与当前的hash值是否相同。由于hash的生成算法是基于vite配置文件和项目依赖的，所以配置文件和依赖的的变化都会导致hash发生变化，从而重新进行预构建。