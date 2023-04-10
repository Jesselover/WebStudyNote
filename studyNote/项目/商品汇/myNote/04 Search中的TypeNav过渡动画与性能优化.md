
%%三级联动业务:
3.1前面基础课程当中v-for很少使用index，以后在写项目的时候，index索引值切记加上
3.2防抖与节流【面试经常出现】
3.3vuex可以模块式开发
vuex经常用的套路是state、mutations、actions、getters、modules%%
## 1.Search中的三级联动与过渡动画
#### 1.过度
1. 需求分析：三级联动在home模块正常显示，但是在search一会显示、一会隐藏 ---解决方案：v-show控制+判断当前路由
2. 代码
事件委托 @mouseleave="leaveShow"（写在leaveIndex里面） @mouseenter="enterShow"
```html
<div @mouseleave="leaveIndex" 
@mouseenter="enterShow">
	<h2 class="all">全部商品分类</h2>
	<div class="sort">···
	</div>
</div>
```

判断是否在Home，做出相应的相应
```js
  mounted() {
    // ? 当目前不在home中时，组件挂载完毕，show=false
    if (this.$route.path != "/Home") {
      this.show = false
    }
  },
   methods: {
    leaveIndex() {
      // 鼠标移出
      this.currentIndex = -1;
      // leaveShow
      if (this.$route.path != "/Home") {
        this.show = false
      }
    },
    // 鼠标移入时，商品分类列表进行展示
    enterShow() {
      if (this.$route.path != "/Home") {
        this.show = true;
      }
    }
  },

```

#### 2.动画
[[vue_cli#12Vue封装的过度与动画]]
需求分析：进入的时候有动画
- 在Vue当中，你可以给 （某一个节点）|（某一个组件）添加过渡动画效果
但是需要注意，节点|组件务必出现v-if|v-show指令才可以使用
1. transition包裹过渡元素，并添加name属性
```html
        <transition name="sort">
          <div class="sort" v-show="show">
          ······
          </div>
        </transition>
```

2. `v-enter,v-leave-to; v-enter-to,v-leave`配合`transtion`;
	%%进入的起点就是离开的终点,开始的终点就是离开的起点%%
	%%`transtion`写在`v-xxx-active`或者标签里%%
```css
    .sort-enter {
      height: 0px;
    }
    .sort-enter-to {
      height: 461px;
    }
    .sort-enter-active {
      transition: all .5s linear;
    }
```
#### 备注;
%%1）
- 在home模块当中，使用了一个功能三级联动功能---->[typeNav]
- 在search模块当中，也使用三级联动的功能------->[typeNav]
- 注意的事项
	注意1：以后在开发项目的时候，如果发现某一个组件在项目当中多个地方出现频繁的使用
	咱们经常把这类的组件注册为全局组件。
	注册全局组件的好处是什么那：只需要注册一次，可以在程序任意地方使用
	
	注意2:咱们经常把项目中共用的全局组件放置于components里面，以后需要注意，
	项目当中全局组件（共用的组件）一般放置于components文件夹中
	
	注意3：全局组件只需要注册一次，就可以在项目当中任意的地方使用，注册全局组件一般是在入口文件注册。
	
2)组件name属性的作用?
2.1开发者工具中可以看见组件的名字
2.2注册全局组件的时候，可以通过组件实例获取相应组件的名字

3)TypeNav组件业务分析?
3.1三级联动在home模块正常显示
3.2三级联动在search一会显示、一会隐藏 ---解决方案：通过一个响应式属性控制三级联动显示与隐藏
3.3开发的时候的出现问题：在home模块下不应该出现显示与隐藏的效果
3.4现在这个问题【三级联动：本身在search模块应该有显示与隐藏的业务】 ，但是在home模块下不应该出现显示与隐藏的业务
说白了：你需要让三级联动组件知道谁在用它。
3.5:通过$route让组件区分在那个模块下
以后在功的时候，如果出现某一个组件要区分当前在哪一个模块中【home、search】，通过$route路由信息区分
3.6路由跳转的时候，相应的组件会把重新销毁与创建----【kepp-alive】

4)过渡效果
最早接触的时候:CSS3
Vue当中也有过渡动画效果---transition内置组件完成
4.1:注意1,在Vue当中，你可以给 （某一个节点）|（某一个组件）添加过渡动画效果
但是需要注意，节点|组件务必出现v-if|v-show指令才可以使用。

%%


## 2. TypeNav三级联动性能优化
### 1.频繁的服务器请求
#### why

1. TypeNav中会向服务器发送请求，所以当切换到含有TypeNav组件的Page时，组件会向服务器发请求%%项目：home切换到search或者search切换到home，你会发现一件事情，组件在频繁的向服务器发请求，获取三级联动的数据进行展示。项目中如果频繁的向服务器发请求，很消耗性能的，因此咱们需要进行优化。%%

2. 路由跳转的时候，组件会进行销毁的%%因为路由跳转的时候，组件会进行销毁的【home组件的created：在向vuex派发action，因此频繁的获取三级联动的数据】%%

===> **频繁的服务器请求**

#### 解决方法
解决方案：把TypeNav中的数据请求，放在App中.
%%
1. app的mounted只会执行一次；
2. app最先执行，执行后后面的组件想用数据都能拿到
 ==>只需要发一次请求，获取到三级联动的数据即可，不需要多次。
%%
```js
export default {
  name: "App",
  mounted(){
  // TypeNav中的数据请求;因为 TypeNav很多pages都用到了，为了只发送一次请求，放在这里
    this.$store.dispatch("categoryList");
  }
};
```

问题：可以放在main.js中吗？
不能，main.js不是一个组件，没有this.$store.dispatch；而且没有this




## 3.合并params，query参数
- TypeNav中的categoryList 传递query参数
- “搜索”按钮传递 params参数
query，params只能传一种，二者是抑或关系
知识：false：NaN+五个基本类型（"",0,undefined,null,false）
TypeNav
```js
goSearch(event) {
      // ? 判断：如果路由跳转时，携带params参数，也需要一并传过去
      if (this.$route.params) {
     // 恒真。Boolean({})=true.
        location.params = this.$route.params;
        location.query = query;
        this.$router.push(location);
      }
    },
```

header
```js
 goSearch() {
      if (this.$route.query) {     
      // 恒真。Boolean({})=true.
        let location = {
          name: "Search",
          params: { keyword: this.keyword || undefined },
          // 传递的是空串或者没传递，就是undefined
        }
        location.query = this.$route.query
        this.$router.push(location)
      }
    },

```
# question
### 1.项目优化
v-if|v-show选择
按需加载          lodash、ant
防抖与节流
请求次数优化 [[#2. TypeNav三级联动性能优化]]

























































