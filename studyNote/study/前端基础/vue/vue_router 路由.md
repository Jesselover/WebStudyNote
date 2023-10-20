**安装路由**：npm i vue-router@3
   2022年2月7日以后，vue-router的默认版本为4版本，vue-router4只能在vue3中使用，
   vue-router3才能在vue2中使用
   使用vue2要npm i vue-router@3

https://blog.csdn.net/xx820702/article/details/126123533
### 1. 概念 

#### **路由**
 1. 理解： 
	 - 一个路由（route）就是一组映射关系（key - value）
	 - 多个路由需要路由器（router）进行管理。
 
 2. 前端路由：key是路径，value可能是function或者component。
 
 3. 路由分类
	1. 后端路由：
	1) 理解：value 是 function, 用于处理客户端提交的请求。
	2) 工作过程：服务器接收到一个请求时, 根据**请求路径**找到匹配的**函数**来处理请求, 返回响应数据。
	
	2. 前端路由：
	1) 理解：value 是 component，用于展示页面内容。
	2) 工作过程：当浏览器的路径改变时, 对应的组件就会显示。

#### spa
 **对 SPA 应用的理解**
	1. 单页 Web 应用（single page web application，SPA）。
	2. 整个应用只有**一个完整的页面**。
	3. 点击页面中的导航链接**不会刷新**页面，只会做页面的**局部更新。**
	4. 数据需要通过 ajax 请求获取。

#### 一般组件、路由组件
|  |一般组件|路由组件|
|--|--------|--------|
|用法|组件标签|由路由规则匹配、路由渲染|
|文件夹|Components|Pages|


### 注意
1. “to”传递数据的时候要加“：”，活数据用模板字符串
```html
<router-link to="/about"> 
<router-link :to="/home/message/detail?id=666&title=你好">
```

2. 通过切换，“隐藏”了的路由组件，默认是**被销毁掉的**，需要的时候再去挂载。%%缓存路由组件`<keep-alive include="组件名"></keep-alive> `%% 

3. 每个组件都有自己的```$route```属性，里面存储着自己的路由信息。

4. 整个应用只有一个router，可以通过组件的```$router```属性获取到。

| $route | $router|
|--------|--------|
|自己的路由信息|整个应用的都一样|



### 2.基本使用
#### **搭建环境**

1. 安装vue-router，命令：```npm i vue-router@3``` 插件库
2. 在`pages`文件夹中写路由组件。路由组件通常存放在```pages```（与components同级）文件夹，一般组件通常存放在```components```文件夹。（路由组件：靠路由规则匹配出来，由路由器渲染的组件。一般不需要自己写标签，一般组件要）
3. 应用插件：（在`main.js`中）
	1. `import VueRouter from "vue-router"`
	2. ```Vue.use(VueRouter)```
	3. `import router from './router'`
	4. 在vm中新加配置项：**`router ；router`** 简写为`router`
4. 编写router配置项:
	- 创建文件：```src/router/index.js``` 该文件用于创建整个应用的路由
	- 在文件中：1.引入VueRouter；2.引入路由组件；3.创建router实例对象，去管理一组一组的路由规则；4.暴露router

   ```js
   //引入VueRouter
   import VueRouter from 'vue-router'
   //引入路由 组件
   import About from '../components/About'
   import Home from '../components/Home'
   
   //创建router实例对象，去管理一组一组的路由规则
   const router = new VueRouter({
   	routes:[
   		{
   			path:'/about',
   			component:About
   		},
   		{
   			path:'/home',
   			component:Home
   		}
   	]
   })
   
   //暴露router
   export default router
   ```

5. 实现切换（

   ```vue
   <router-link active-class="active" to="/about">About</router-link>
   ```
	1. `<router-link to="路径">`
	2. `router-link ` 最终会被`vue-link`转换成`<a></a>` 标签
	3. `active-class` 设置激活后的样式

6. 指定展示位置

   ```vue
   <router-view></router-view>
   ```


### 3.多级路由（多级路由）

1. 配置路由规则，使用children配置项：
	
	**二级路由不要写`/`**
	children

   ```js
   routes:[
   	{
   		path:'/about',
   		component:About,
   	},
   	{
   		path:'/home',
   		component:Home,
   		children:[ //通过children配置子级路由
   			{
   				path:'news', //此处一定不要写：/news
   				component:News
   			},
   			{
   				path:'message',//此处一定不要写：/message
   				component:Message
   			}
   		]
   	}
   ]
   ```

2. 跳转（要写完整路径）：

   ```vue
   <router-link to="/home/news">News</router-link>
   ```

### 4.路由的query参数

1. 传递参数

   ```vue
   <!-- 1.跳转并携带query参数，to的字符串写法 -->
   <router-link :to="/home/message/detail?id=666&title=你好">跳转</router-link>
   <!-- 携带当前组件的数据。：to=“” 解析里面的js表达式，`` 里面的js表达式是个模板字符串 -->
   <router-link :to="`/home/message/detail?id=${m.id}&title=${m.title}`">{{m.title}}</router-link>&nbsp;&nbsp;		
   <!-- 2.跳转并携带query参数，to的对象写法 -->
   <router-link 
   	:to="{
   		path:'/home/message/detail',
   		query:{
	   		id:666,
	        title:'你好'
   		}
   	}"
   >
   跳转
   </router-link>
   ```

2. 接收参数：

   ```js
   $route.query.id
   $route.query.title
   ```

### 5.命名路由

1. 作用：可以简化路由的跳转。

2. 如何使用

   1. 给路由命名： `name:"xxx"`

      ```js
      {
      	path:'/demo',
      	component:Demo,
      	children:[
      		{
      			path:'test',
      			component:Test,
      			children:[
      				{
                        name:'hello' //给路由命名
      					path:'welcome',
      					component:Hello,
      				}
      			]
      		}
      	]
      }
      ```

   2. 简化跳转：` :to="{name:'hello'}`

      ```vue
      <!--简化前，需要写完整的路径 -->
   
      
      <!--简化后，直接通过名字跳转 -->
      <router-link :to="{name:'hello'}">跳转</router-link>
      
      <!--简化写法配合传递参数 -->
      <router-link 
      	:to="{
      		name:'hello',
      		query:{
      		   id:666,
               title:'你好'
      		}
      	}"
      >跳转</router-link>
      ```

### 6.路由的params参数

1. 配置路由，声明接收params参数 `path:'detail/:id/:title', //使用占位符声明接收params参数`
   `path:"/search/:keyword?"` 加个问号，表示可传可不穿
   ```js
   {
   	path:'/home',
   	component:Home,
   	children:[
   		{
   			path:'news',
   			component:News
   		},
   		{
   			component:Message,
   			children:[
   				{
   					name:'xiangqing',
   					path:'detail/:id/:title', //使用占位符声明接收params参数
   					component:Detail
   				}
   			]
   		}
   	]
   }
   ```

2. 传递参数 
	1. ` :to="/home/message/detail/666/你好`
	2. to的对象写法.**只能用name**，不能用path

   ```vue
   <!-- 跳转并携带params参数，to的字符串写法 -->
   <router-link :to="/home/message/detail/666/你好">跳转</router-link>
   				
   <!-- 跳转并携带params参数，to的对象写法.只能用name，不能用path -->
   <router-link 
   	:to="{
   		name:'xiangqing',
   		params:{
   		   id:666,
           title:'你好'
   		}
   	}"
   >跳转</router-link>
   ```

   > 特别注意：路由携带params参数时，若使用to的对象写法，则不能使用path配置项，必须使用name配置！

3. 接收参数：

   ```js
   $route.params.id
   $route.params.title
   ```

### 7.路由的props配置

​	作用：让路由组件更方便的收到参数 （写在那个组件里，把信息传给那个组件）

```js
{
	name:'xiangqing',
	path:'detail/:id/:title',
	component:Detail,

	//第一种写法：props值为对象，该对象中所有的key-value的组合最终都会通过props传给Detail组件（额外传递参数）
	props:{a:900} 只能写死数据

	//第二种写法：props值为布尔值，布尔值为true，则把路由收到的所有params参数通过props传给Detail组件（只能传递params）
	props:true    只能传递params
	
	//第三种写法：props值为函数，该函数返回的对象中每一组key-value都会通过props传给Detail组件（可以传递query，params）
	props($route){
		return {
			id:$route.query.id,
			title:$route.query.title
		}
	}
		// 解构赋值
	// props({query，param}){
	//  return {
	//      id:query.id,
	//      eg:param.eg
	//  }
	// }
	
		// 连续解构赋值
	// props({query:{id,title}}){
	// }
}
```
```js
// 使用数据的组件要接收一下
props:['id','title'],
```

### 8.```<router-link>```的replace属性

1. 作用：控制路由跳转时操作浏览器历史记录的模式

2. 浏览器的历史记录有两种写入方式：分别为```push```和```replace```，```push```是追加历史记录，```replace```是替换当前记录。路由跳转时候默认为```push```

3. 如何开启```replace```模式：```<router-link replace .......>News</router-link>```

### 9.编程式路由导航

1. 作用：不借助```<router-link> ```实现路由跳转，让路由跳转更加灵活

2. 具体编码：
- $router是vueRouter类的一个实例，push是vueRouter的原型上的一个方法==>push的this是此实例的上下文对象

   ```js
   //$router的两个API
   this.$router.push({
   	name:'xiangqing',
   	params:{
   			id:xxx,
   			title:xxx
   		}
   })
   
   this.$router.replace({
   	name:'xiangqing',
   	params:{
   			id:xxx,
   			title:xxx
   		}
   })
   this.$router.forward() //前进
   this.$router.back() //后退
   this.$router.go() //可前进也可后退，里面传递一个数字
   // +3 往前连续走3步，-2往后退两步
   ```

### 10.缓存路由组件

1. 作用：让不展示的路由组件保持挂载，不被销毁。

2. 具体编码：`<keep-alive include="组件名"></keep-alive> 
%%不写include ，默认缓存所有组件%%
   ```vue
   <keep-alive include="News"> 
       <router-view></router-view>
   </keep-alive>
   ```

### 11.两个新的生命周期钩子

1. 作用：路由组件所**独有**的两个钩子，用于捕获路由组件的激活状态。
2. 具体名字：
   1. ```activated``` 路由组件被激活时触发。
   2. ```deactivated``` 路由组件失活时触发。

### 12.路由守卫

1. 作用：对路由进行权限控制

2. 分类：全局守卫、独享守卫、组件内守卫

#### 全局守卫:
1. `router.beforeEach(to,from,next){}` 全局前置守卫：初始化时执行、每次路由切换前执行
2. `router.afterEach((to,from){}` 全局后置守卫：初始化时执行、每次路由切换后执行

   ```js
   //全局前置守卫：初始化时执行、每次路由切换前执行
   router.beforeEach((to,from,next)=>{
   	console.log('beforeEach',to,from)
   	if(to.meta.isAuth){ //判断当前路由是否需要进行权限控制
   		if(localStorage.getItem('school') === 'atguigu'){ //权限控制的具体规则
   			next() //放行
   		}else{
   			alert('暂无权限查看')
   			// next({name:'guanyu'})
   		}
   	}else{
   		next() //放行
   	}
   })
   
   //全局后置守卫：初始化时执行、每次路由切换后执行
   router.afterEach((to,from)=>{
   	console.log('afterEach',to,from)
   	if(to.meta.title){ 
   		document.title = to.meta.title //修改网页的title
   	}else{
   		document.title = 'vue_test'
   	}
   })
   ```

- to：去的目标路由的信息
- from：从哪里来的
- next:next() 放行


#### 独享守卫: 

- `beforeEnter(to,from,next){}`
- 写在route的配置项里
- 只有独享前置路由守卫，没有独享后置路由守卫。
- 独享前置路由守卫 可以和 全局后置路由守卫 配合使用

   ```js
{
   name:'xinwen',
   path:'news',
   component:News,
   meta:{isAuth:true,title:'新闻'},
   beforeEnter(to,from,next){
   	console.log('beforeEnter',to,from)
   	if(to.meta.isAuth){ //判断当前路由是否需要进行权限控制
   		if(localStorage.getItem('school') === 'atguigu'){
   			next()
   		}else{
   			alert('暂无权限查看')
   			// next({name:'guanyu'})
   		}
   	}else{
   		next()
   	}
   }
}
   ```

#### 组件内守卫
[导航守卫 | Vue Router (vuejs.org)](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#%E7%BB%84%E4%BB%B6%E5%86%85%E7%9A%84%E5%AE%88%E5%8D%AB)

6. 组件内想写一些单独的功能
	- 不通过路由规则无法调用（eg：直接进去）
	- 进入守卫，离开守卫一定要写next（），不写next（）无法继续向下跳转
	- beforeRouteEnter的to 和beforeRouterLeave的from一样
	
   ```js
   //进入守卫：通过路由规则，进入该组件时被调用
   beforeRouteEnter (to, from, next) {
   },
   //离开守卫：通过路由规则，离开该组件时被调用
   beforeRouteLeave (to, from, next) {
   }
   ```
```
// 同时使用组件内路由与组件特有声明周期钩子
// 进入时
beforeRouteEnter
activated
// 出去时
beforeRouteLeave
deactivated
```

### 13.路由器的两种工作模式
`npm run build` 打包项目，放在dist文件中，只能在服务器中打开
1. 对于一个url来说，什么是hash值？—— #及其后面的内容就是hash值。
2. hash值不会包含在 HTTP 请求中，即：hash值不会带给服务器。
3. hash模式：
   1. 地址中永远带着#号，不美观 。
   2. **若以后将地址通过第三方手机app分享，若app校验严格，则地址会被标记为不合法。**
   3. 兼容性较好。
4. history模式：
   1. 地址干净，美观 。
   2. 兼容性和hash模式相比略差。
   3. 应用部署上线时需要后端人员支持，解决刷新页面服务端404的问题（node.js的中间件：connect-history 。引入后使用，在静态页面使用之前）。（一刷新可能会把history模式全部认成地址）

# 总结
#### 1.#router
```js
this.$router.forward() //前进
this.$router.back() //后退
this.$router.go() //可前进也可后退，里面传递一个数字
this.$route.query.title
this.$route.params.title
```

#### 2.标签
1. `   <router-link to="...">跳转</router-link>`
	- `to` (`:to` 绑定数据)
	- `replace`
2. `<router-view></router-view>`
3. `<keep-alive include="News"> <router-view></router-view> </keep-alive>`缓存路由组件
	- `include="组件名"`

#### 3.配置项
0. routes 数组，里面是对象（route）
```js
   const router = new VueRouter({
   	routes:[
   		{
   			path:'/about',
   			component:About
   		},
   	]
   })
```

1. path 字符串
2. component 组件名
3. children 数组，里面是对象（routes）
4. name 字符串
5. props($route){ return ... } j善于解构赋值
6. mate 对象 用于存储其他信息



