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