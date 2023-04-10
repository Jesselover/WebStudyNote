安装vuex: npm i vuex@3

   2022年2月7日，vue3成为默认版本，如果直接npm i vue 安装的直接就是vue3了，并且vue3成为默认版本的同时，vuex也更新到了4版本，那么也就是说如果我们直接npm i vuex安装的是vuex4，而vuex的4版本只能在vue3中使用，vue2中要用vuex的3版本，vue3中要用vuex的4版本，所以使用vue2要安装vuex3,  :     npm i vuex@3
https://blog.csdn.net/xx820702/article/details/126123533

## Vuex

### 1.概念

​专门在Vue中实现集中式状态（数据）管理的一个Vue插件(`vue.use(Vuex)`)，对vue应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，且适用于任意组件间通信。
**1. Github 地址: [https://github.com/vuejs/vuex](https://github.com/vuejs/vuex)**
![](Pasted%20image%2020220904211419.png)

### 2.何时使用？

​多个组件需要共享数据时
	
	
### 3.搭建vuex环境
- `npm i vuex` (在vue2中使用时`npm i vuex@3` 指定vuex的3版本)
- `Vue.use(vues)`
- 让所有vc都能看到`store`,在vm中加入配置项`store`

1. 创建文件：```src/store/index.js```
	- 该文件用于创建Vuex中最为核心的store
	- 创建store之前,必须使用Vue.use(Vuex),因此要引入vue
	- 配置store里的对象
	- 暴露

   ```js
   // 该文件用于创建Vuex中最为核心的store
   //引入Vue核心库
   import Vue from 'vue'
   //引入Vuex
   import Vuex from 'vuex'
   //应用Vuex插件
   Vue.use(Vuex)
   
   //准备actions对象——响应组件中用户的动作
   const actions = {}
   //准备mutations对象——修改state中的数据
   const mutations = {}
   //准备state对象——保存具体的数据
   const state = {}
   
   //创建并暴露store
   export default new Vuex.Store({
   	actions,// actions: actions的简写形势
   	mutations,
   	state
   })
   ```

2. 在```main.js```中创建vm时传入```store```配置项
	- 外部引入
	- vm里面添加

   ```js
   ......
   //引入store
   import store from './store '
   // 默认找store里面的index文件
   ......
   
   //创建vm
   new Vue({
   	el:'#app',
   	render: h => h(App),
   	store
   })
   ```

###    4.基本使用

1. 初始化数据、配置```actions```、配置```mutations```，操作文件```store.js```

   ```js
   //引入Vue核心库
   import Vue from 'vue'
   //引入Vuex
   import Vuex from 'vuex'
   //引用Vuex
   Vue.use(Vuex)
   
   const actions = {
       //响应组件中加的动作
   	jia(context,value){
   		// console.log('actions中的jia被调用了',miniStore,value)
   		// context 上下文对象，里面有可能用到的方法
   		// value 传入的值
   		context.commit('JIA',value)
   	},
   }
   
   const mutations = {
       //执行加
   	JIA(state,value){
   		// console.log('mutations中的JIA被调用了',state,value)
   		state.sum += value
   	}
   }
   
   //初始化数据
   const state = {
      sum:0
   }
   
   //创建并暴露store
   export default new Vuex.Store({
   	actions,
   	mutations,
   	state,
   })
   ```
```js
//使用
...
// 传递数据
this.$store.dispath('jia',this.n)
// 使用数据
{{$store.state.sum}}
...
```

2. 组件中读取vuex中的数据：```$store.state.sum```

3. 组件中修改vuex中的数据：```$store.dispatch('action中的方法名',数据)``` 或 ```$store.commit('mutations中的方法名',数据)```

   >  备注：若没有网络请求或其他业务逻辑，组件中也可以越过actions，即不写```dispatch```，直接编写```commit```

4. actions中的context
![](Pasted%20image%2020220905204816.png)
### 5.getters的使用

1. 概念：当state中的数据需要经过加工后再使用时，可以使用getters加工。（state与getter 类似于data与computed）

2. 在```store.js```中追加```getters```配置

   ```js
   ......   
   const getters = {
   	bigSum(state){
   		return state.sum * 10
   	}
   }
   
   export default new Vuex.Store({
   	......
   	getters
   })
   ```

3. 组件中读取数据：```$store.getters.bigSum```

### 6.四个map方法的使用
```
//在需用的组件中引入
import {mapState，mapGetters，mapMutations，mapActions} from 'vuex'
```

1. <strong>mapState方法：</strong>用于帮助我们映射```state```中的数据为计算属性
	- mapState是一个对象，通过 `...mapState` 来解构赋值
	- mapState写在计算属性中。在devtools中被放在vuex binding中
	- 两种方法：对象方法({此组件中的使用名，"store中的名字"})，数字方法
	- %%对象写法：name：函数（会注入一个参数state，是大仓库的数据）%%

   ```js
   computed: {
       //借助mapState生成计算属性：sum、school、subject（对象写法）从state中读取数据
   //用sum代替$store.state.sum
        ...mapState({sum:'sum',school:'school',subject:'subject'}),
       //借助mapState生成计算属性：sum、school、subject（数组写法）
       ...mapState(['sum','school','subject']),
   },
   ```

2. <strong>mapGetters方法：</strong>用于帮助我们映射```getters```中的数据为计算属性

   ```js
   computed: {
       //借助mapGetters生成计算属性：bigSum（对象写法）
       ...mapGetters({bigSum:'bigSum'}),
       // 计算属性的名字与getter中的方法名一样
   
       //借助mapGetters生成计算属性：bigSum（数组写法）
       ...mapGetters(['bigSum'])
   },
   ```

3. <strong>mapActions方法：</strong>用于帮助我们生成与```actions```对话的方法，即：包含```$store.dispatch(xxx)```的函数

   ```js
   methods:{
       //靠mapActions生成：incrementOdd、incrementWait（对象形式）
       ...mapActions({incrementOdd:'jiaOdd',incrementWait:'jiaWait'})
       //靠mapActions生成：incrementOdd、incrementWait（数组形式）
       //$store.dispatch(xxx)在组件中的method名字与actions中的函数名一样
       ...mapActions(['jiaOdd','jiaWait'])
   }
   ```

4. <strong>mapMutations方法：</strong>用于帮助我们生成与```mutations```对话的方法，即：包含```$store.commit(xxx)```的函数
	**调用的时候，要加上要使用的参数，否则默认为当前函数传入的value，一般为event**

   ```js
   methods:{
       //靠mapActions生成：increment、decrement（对象形式）
//**调用的时候，要加上要使用的参数，否则默认increment()传入的参数，这里是value**
...mapMutations({increment:'JIA',decrement:'JIAN'}),
       //靠mapMutations生成：JIA、JIAN（对象形式）
       ...mapMutations(['JIA','JIAN']),
   }
```
```html
<div>{{increment(this.n)}}</div>
```

> 备注：mapActions与mapMutations使用时，若需要传递参数需要：在模板中绑定事件时传递好参数，否则参数是事件对象。

### 7.模块化+命名空间

1. 目的：让代码更好维护，让多种数据分类更加明确。

2. 修改```store.js```
	1. 开启命名空间
	2. `...mapXXX('分类名'，[...]/{...})`

   ```javascript
   const countAbout = {
     namespaced:true,//开启命名空间，开启命名空间，开启后，分类名才能被mapState方法认出
     state:{x:1},
     mutations: { ... },
     actions: { ... },
     getters: {
       bigSum(state){
          return state.sum * 10
       }
     }
   }
   
   const personAbout = {
     namespaced:true,//开启命名空间
     state:{ ... },
     mutations: { ... },
     actions: { ... }
   }
   // 如下暴露后取数据的方式：countAbout.x
   // 若想直接写数据名字
   //1.开启命名空间,2.如下写数据
   //...mapState('countAbout',['sum','school','subject']),
   //...mapMutations('countAbout',{increment:'JIA',decrement:'JIAN'}),
   //课上的
   //export default new Vuex.Store({
   //modules:{
       //personAbout,
       //countAbou,
    //}
    //})
   //笔记上的
   const store = new Vuex.Store({
     modules: {
       countAbout,
       personAbout
     }
   })
   

   ```

3. 开启命名空间后，组件中读取state数据：

   ```js
   //方式一：自己直接读取
   this.$store.state.personAbout.list
   //方式二：借助mapState读取：
   ...mapState('countAbout',['sum','school','subject']),
   ```

4. 开启命名空间后，组件中读取getters数据：

   ```js
   //方式一：自己直接读取
   this.$store.getters['personAbout/firstPersonName']
   //方式二：借助mapGetters读取：
   ...mapGetters('countAbout',['bigSum'])
   ```

5. 开启命名空间后，组件中调用dispatch

   ```js
   //方式一：自己直接dispatch
   this.$store.dispatch('personAbout/addPersonWang',person)
   //方式二：借助mapActions：
   ...mapActions('countAbout',{incrementOdd:'jiaOdd',incrementWait:'jiaWait'})
   ```

6. 开启命名空间后，组件中调用commit

   ```js
   //方式一：自己直接commit
   this.$store.commit('personAbout/ADD_PERSON',person)
   //方式二：借助mapMutations：
   ...mapMutations('countAbout',{increment:'JIA',decrement:'JIAN'}),
   ```




