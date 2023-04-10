## study
### 1.Home模块
1. 静态页面的完成（样式）
2. 静态组件
	1. header,footer；（day1已完成）
	2. 三级联动（都要用，拆成全局组件）
	3. 轮播图+尚品汇快报
	4. 今日推荐
	5. 热卖排行
	6. 猜你喜欢
	7. 家用电器/手机通信（拆分成1个，然后复用）
	8. 品牌logo
3. 发请求获取服务器数据进行展示
4. 开发动态业务

拆分组件：结构+样式+图片资源
一共要拆分为七个组件

### 1.三级联动组件--全局组件
拆分：HTML+CSS+图片资源
- `TypeNav`出现在：home，search，detail
- 全局组件：只注册一次，全局都能用
main.js
```js
import TypeNav from "@/pages/Home/TypeNav"
// 引入后全局注册Vue.component("组件名",组件)
Vue.component(TypeNav.name, TypeNav)
```
Home/index.vue
```html
  <div>
    我是首页
    <!-- 三级联动组件，全局注册，无需引入,无需注册 -->
    <TypeNav></TypeNav>
  </div>
```

...
其他组件的拆分：1. HTML+CSS+图片资源 2. 在Home/index.vue中引入、注册、使用

### 2.POSTMAN工具接口测试
- collections/GET
1. `http://39.98.123.211` 成功200
2. 根据弹幕提示改为 http://gmall-h5-api.atguigu.cn/api/product/getBaseCategoryLis 成功200%% 原接口`http://39.98.123.211/api/user/passport/login` 失败403，显示403%%
3. 整个项目，接口前缀都有`/api`

### 3.axios 二次封装
1. 向服务端发请求的方式：XMLHttpRequest(ajax原生)、fetch、JQ($)、Axios %%AJAX:客户端可以'敲敲的'向服务器端发请求，在页面没有刷新的情况下，实现页面的局部更新。%%
2. why进行二次封装：请求拦截器、相应拦截器
3. 下载axios `npm intall --save axios --legacy-peer-deps`
4. 工作的时候src目录下的`API`文件夹，一般关于axios二次封装的文件
5. 文档参考：[axios中文文档|axios中文网 | axios (axios-js.com)](http://www.axios-js.com/zh-cn/docs/index.html)
```js
// axios二次封装
// * 引入
import axios from 'axios';
// *创建axios实例
const requests = axios.create({
    // 配置对象
    baseURL: '/api',// 基础路径，给发请求的路径上都带上api（ 整个项目，接口前缀都有`/api`）
    timeout: 5000, //超时时间为两秒
});
// *请求拦截器：发请求之前就可拦截到，在请求发出前执行
requests.interceptors.request.use((config) => {
    // config:配置对象，对象里面有请求头headers
    return config;
});
// *响应拦截器
requests.interceptors.response.use((res)=>{
    return res.data;
},(error)=>{
    return Promise.reject(new Error('fail'));
});
// *对外暴露
export default requests;
```

### 4.API接口统一管理
- 项目很小：在组件声明周期中发请求
- 项目很大：API接口统一管理（src/API/indx.js）
1. 跨域问题
	1. 来源：前端项目本地服务器与后台服务器跨域
	%%前端本地服务器：`http://localhost:8080/#/Home`%%
	%%后端服务器：`http://39.98.123.211`%%
	2. 跨域:如果多次请求协议、域名、端口号有不同的地方，称之为跨域
	3. 解决：JSONP、CROS、代理%%本项目配置本地代理服务器:vue.config.js(webpack说明书)%%
	[[vue_cli#13 vue脚手架配置代理服务器]]
	<p style="color:red">服务器与服务器之间无跨域问题，浏览器与服务器有</p>
<p style="color:red">vue.config.js 配置文件修改后,要重新 npm run serve</p>
```js
  //配置代理跨域
  devServer: {
    proxy: {
      "/api": {
        target: "http://gmall-h5-api.atguigu.cn",
      },
    },
  },
```

2. 请求发送；
```js
import { reqCategoryList } from "@/API";//引入
reqCategoryList();//使用
```

### 5.nprogress进度条的使用
1. 安装 `save nprogress --legacy-peer-deps`
2. 业务分析：发请求的时候进度条开始，请求结束，进度条结束==>在请求、响应拦截器中使用
3. 使用：引入，引入样式。在请求拦截器中`nprogress.start()`，在相应拦截其中`nprogress.done()`
	1. `nprogress.start()`进度条开始
	2. `nprogress.done()`进度条使用
4. 工作的时候，修改进度条的颜色，修改源码样式.bar类名的
```JS
// axios二次封装
// * 引入
import axios from 'axios';
//? 引入进度条
import nprogress from 'nprogress';
// ?引入进度条样式
import 'nprogress/nprogress.css';
// *创建axios实例
const requests = axios.create({
    // 配置对象
    baseURL: '/api',// 基础路径，给发请求的路径上都带上api（ 整个项目，接口前缀都有`/api`）
    timeout: 5000, //超时时间为两秒
});
// *请求拦截器：发请求之前就可拦截到，在请求发出前执行
requests.interceptors.request.use((config) => {
    // config:配置对象，对象里面有请求头headers
    // ?进度条开始
    console.log('发送请求');
    nprogress.start();
    return config;
});
// *响应拦截器
requests.interceptors.response.use((res) => {
    // ?进度条结束
    console.log('响应请求');
    nprogress.done();
    return res.data;
}, (error) => {
    return Promise.reject(new Error('fail'));
});
// *对外暴露
export default requests;
```

### 6.vuex
vuex:Vue官方提供的一个插件，插件可以管理项目共用数据。
vuex：书写任何项目都需要vuex？
项目大的时候，需要有一个地方‘统一管理数据’即为仓库store
Vuex基本使用:[[vue_x]]


犯的错误:
1:项目阶段，左侧菜单目录，只能有项目文件夹
2:联想电脑安装node_modules依赖包的时候，经常丢包。npm install --save axios --force
3：单词错误
4：路由理解
KV：K--->URL  V---->相应的组件
配置路由：
     ------路由组件
     -----router--->index.js
                  import Vue  from 'vue';
                  import VueRouter from 'vue-router';
                  //使用插件
                  Vue.use(VueRouter);
                  //对外暴露VueRouter类的实例
                  export default new VueRouter({
                       routes:[
                            {
                                 path:'/home',
                                 component:Home
                            }
                       ]
                  })
    ------main.js   配置项不能瞎写


$router:进行编程式导航的路由跳转
this.$router.push|this.$router.replace
$route:可以获取路由的信息|参数
this.$route.path
this.$route.params|query
this.$route.meta

















# question
1. 进度条出不来：没有发送请求