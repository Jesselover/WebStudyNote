#### jsconfig.json

https://blog.csdn.net/weixin_44067347/article/details/125632655

#### require import的区别

[面试：require 和 import 的区别？ - 掘金 (juejin.cn)](https://juejin.cn/post/7014011266796617736)

| |require|import|
|-|-------|------|
|标准|CommonJs|ES6|
|输出|值的拷贝|值的引用|
|执行|运行时执行，同步加载，是一个赋值过程|编译时执行，异步加载，是解构过程|
|性能|稍低（运行时才能引入模块并且赋值给某个对象）| 稍快（只需根据import中的接口，在编译时引入指定模块）|

#### rpc请求，restful风格

[restful与rpc风格 - myworldworld - 博客园 (cnblogs.com)](https://www.cnblogs.com/hello-/articles/9958943.html)
[技术杂谈 【RPC请求和Restful风格】_油纸雨伞的博客-CSDN博客](https://blog.csdn.net/xuschang/article/details/129630109)
restful ：无状态，可缓存，统一接口，分层系统
[Nodejs Sequelize暴力快速入门到应用 - 掘金 (juejin.cn)](https://juejin.cn/post/7123155989200633870)

#### Eureka

[微服务Eureka使用详解 - 一响贪欢 - 博客园 (cnblogs.com)](https://www.cnblogs.com/yxth/p/10845640.html)
[Spring Cloud 入门 之 Eureka 篇（一） - 掘金 (juejin.cn)](https://juejin.cn/post/6844903633419517960)

#### webpack、rollup打包

[rollup与webpack - 掘金 (juejin.cn)](https://juejin.cn/post/6984052513867563044)

rollup "小而美" 
专门针对于类库打包，打包时会自动 tree shaking，因此提及会比webpack小很多

#### v-if/v-show

> 尽量使用v-if ，不加载dom节点。防止dom节点被恶意篡改

|  |v-if|v-show|
|--|----|-------|
|控制手段|dom元素的控制与删除|css的display属性|
|编译过程|局部编译/卸载|简单的基于css的切换|
|编译条件|渲染条件为真才渲染|始终会被渲染并保存在dom中|
|切换|触发vue组件生命周期|不触发|
|template|与template配合v-else-if,v-else使用|不配合|
|适用场景|运行条件很少改变|频繁切换|


-  `v-if`由false变为true的时候，触发组件的`beforeCreate`、`create`、`beforeMount`、`mounted`钩子，由true变为false时触发组件的`beforeDestory`、`destoryed`方法。
- 编译过程不同：`v-show`只是简单地基于css切换。而`v-if`的切换有一个局部编译/卸载的过程，切换过程中会销毁和重建内部的事件监听和子组件
- 编译条件不同：`v-if`是真正的条件渲染，它会确保在切换过程中条件块内的事件监听器和子组件被销毁和重建，只有渲染条件为真时才渲染
- display：none;浏览器不渲染，不占据空间，切换发生重排，会继承，对计数器有影响，影响过渡动画

[v-if/v-show的区别 - 掘金 (juejin.cn)](https://juejin.cn/post/7139883954835816462)
[vue中v-show和v-if深入理解 - 掘金 (juejin.cn)](https://juejin.cn/post/7074834887269842975)


#### display:none visibility:hidden opacity:0 
|       |display:none|visibility:hidden|opacity:0|
|-------|------------|-----------------|---------|
|DOM结构|浏览器不渲染|渲染|渲染|
|占据空间|不占据|占据|占据|
|性能|重排|重绘|提升为合成层，不重绘|
|继承|会|会，子元素visibility: visible取消隐藏|会，不能取消隐藏|
|计数器|有影响|无影响||
|动画|transtion不会产生过度动画|transtion|

[display:none与visibility: hidden的区别 - 掘金 (juejin.cn)](https://juejin.cn/post/6975819518153064462)
[display:none visibility:hidden opacity:0 区别 - 掘金 (juejin.cn)](https://juejin.cn/post/6844904200401502215)