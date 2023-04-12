#### v-if/v-show
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