### 项目安装与启动

1. 使用私有仓库
[[npm 私有仓库使用说明]]
``` bash
npm login --registry=https://npm.mctech.vip --scope=@mctech
```

*npm* 会询问 *username、password、email* 等信息。

<!--同样，这里也不需要额外使用 *yarn login* 进行登录设置。-->
```bash
# username
dutf
# password 输入密码时看不见
6DvyhzTBOLupt
#email
2864380034@qq.com
```

2. 打包与运行
package.json里看如何打包
例如：pc-pubilc-website 
- `yarn build:dev`
- 打包完成后：运行和调试（CTRL+shift+D）--> launch, 得到端口号
- localhost:端口号  （如若跳转失败，手动输入地址）



### 目录说明

#### 目录
.vscode
	setting.json %%vscode编辑器和插件的相关配置%%
	launch.json %%调试配置文件%%
dist
node_modules
src
	server %%接口转调,隐藏真实url,前端数据校验,对后端的数据可以进行二次加工,前端的服务与真实服务采用rpc请求,采用restful风格定义url%%
		文件夹/index.js %%接口转调%%
		common/index.js %%公共接口转调%%
		index.js %%接口的启动,包括自定义接口,公共接口等的汇总%%
		constants.js %%接口全局固定变量%%
		mount-apis.js %%接受定义的接口,并注册%%
		rpc-services.js %%rpc服务注册%%
		.henhouserc %%本地调试的时候使用%%
		bootstrap.yml
		individual.yml
	web-content
		assert %%一般只放新增的字体图标%%
		component %%公共组件%%
		css %%样式，在css/index.js中暴露css文件%%
		module %%写vue实例，每个实例都有自己的入口index.js%%
			example
				view (index.vue)
				index.js
				store.js / store
		template / page.html %%打包时使用%%
		utils  %%前端常用的一些方法%%
		.eslintrc
		include.js %%将page.html,component,css等同意暴露，减少代码量%%
.cz-config.js %%与Visual Studio Code Commitizen Support 插件配合使用%%
.eslintrc %%"rules"%%
.eslintignore %%eslint需要忽略的文件%%
.gitignore %%git提交时的忽略文件%%
package.json %%应用包配置文件%%
babel.config.js %%babel配置文件%%
Dockerfile %%给容器一个标识%%
jsconfig.json
[vue项目中 jsconfig.json是什么_唐璜Taro的博客-CSDN博客](https://blog.csdn.net/weixin_44067347/article/details/125632655)
yarn.lock
mcpack.config.js
vue.config.js
babel.config.js: babel的配置文件[es6转es5]
package.json: 应用包配置文件  
README.md: 应用描述文件
package-lock.json：包版本控制文件


#### vue.config.js
[配置参考 | Vue CLI (vuejs.org)](https://cli.vuejs.org/zh/config/#vue-config-js)

#### jsconfig.json
[vue项目中 jsconfig.json是什么_唐璜Taro的博客-CSDN博客](https://blog.csdn.net/weixin_44067347/article/details/125632655)
#### package.json
	[package.json 配置完全解读 - 掘金 (juejin.cn)](https://juejin.cn/post/7161392772665540644)	
	[npm安装依赖(dependency)版本号及更新规则 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/462091907)
```json
// "scripts" 指定项目的一些内置脚本命令，这些命令可以通过 npm run / yarn 来执行。通常包含项目开发，构建 等 CI 命令，比如
"scripts": { "build": "webpack" }
// dependencies 运行依赖
//devDependence 开发依赖
```
package.json
```json
"scripts": {
    "build": "mcpack3",
    "build:dev": "mcpack3 --env development --w",
    // yarn build:dev 打包
    "start": "node ./src/server/index.js --profiles=debug"
    // profiles=debug 传递变量，默认为开发环境。在.vsocde中用"program","args"配置

  },
```
launch.json
![[b0ae295d30a528e9e815c05d945156a.png]]

![[Pasted image 20230412153001.png]]

#### launch.json
[VSCode launch.json配置详解 - 掘金 (juejin.cn)](https://juejin.cn/post/6844904198702645262)
```json
launch.json

{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "pwa-node",
      "request": "launch",
      "name": "Launch Program",
      "skipFiles": [
        "<node_internals>/**"
      ],
      // 调式入口文件地址
      "program": "${workspaceFolder}\\src\\server\\index.js",
      // 传递给程序的参数
      "args" : [
      "--profiles=debug"
      ]
    }
  ]
}
```

#### .henhouserc
本地调式的时候使用
```json
{
  "proxy": {
  # 身份验证成功后,要去的端口
    "port": 6003,
    "mappings": {
    # 路径中有pc ,走此地址
      "/pc/": "http://localhost:9090/pc/",
    # 没有pc,找线上
      "*": "http://dev.mctech.vip"
    }
  }
}
```

#### bootstrap.yml
```yml
application:
  name: "pc-public-website"
# 启动的端口号
  port: ${port:9090}
  profiles:
    active: '${profiles:dev,debug}'
eureka:
  client:
  # 读取`eureka的url
    serverUrls: '${eureka.servers:http://eureka.mc/eureka}'
```


#### individual.yml
```yml

application.profiles: debug
public: true
# 不注册到开发环境的 Eureka 中
eureka:
  registerEureka: false
# rpc 服务配置
mctech:
  rpc:
    service:
      #key为服务的serviceId, value为对应的目标服务器的地址信息
      # 'pc-public-website': 'http://localhost:9090/pc/'
      # 'pc-public-service': 'http://localhost:9203'
      # 如果后端在本地，在本地调试时需要这样配置，会直接走配置好的地址
# 登陆成功后的身份验证
cas:
  client:
    enableDomain: false
    # cas身份认证，认证中心登录页所在的站点，跳转到登录页的地址会在后面加上'/login'路径，客户端重定向
    casServerUrlPrefix: 'http://dev.mctech.vip/cas'
    # cas身份认证，客户端验证返回的ticket有效性的地址前缀，有可能是内网地址。服务端内部调用路径
    casValidateUrlPrefix: 'http://dev.mctech.vip/cas/'
    # 身份认证成功后带着ticket回跳的服务器地址前缀，浏览器要访问的目标地址前缀
    casServiceUrl: 'http://localhost:6003/'
    # 需要跳过身份验证的地址，','分隔，支持ant格式的路径
    ignorePathes: /js/**,/images/**
    
# 前端如果需要文件服务，在这里配置
file:
  type: native
  native:
    bucketName: 'iwop-dev'
    accessKeyId: 'LTAIMMsdxTwnzzof'
    accessKeySecret: 'ZluzSTJNgco6W9uilp6sKB5HCQy4jz'
    privateEndPoint: 'http://dev.mctech.vip/fss/'
    publicEndPoint: 'http://dev.mctech.vip/fss/'
    tempDir: 'tmp-files'

# 前端如果需要redis，在这里配置
redis:
  host: 127.0.0.1
  port: 6379
  db: 0**
```


#### mcpack.config.js
```js
const variables = require('@mctech/mussel/src/variables.common')
const { resolve } = require('path')
module.exports = {
  browser: {
    alias: {
      entries: [
        { find: '@', replacement: resolve(__dirname, 'src/web-content') },
        {
          find: '@component',
          replacement: resolve(__dirname, 'src/web-content/component')
        },
        {
          find: '@utils',
          replacement: resolve(__dirname, 'src/web-content/utils')
        },
        {
          find: '@svg',
          replacement: resolve(__dirname, 'src/web-content/svg')
        }
      ]
    },
    vue: {
      css: false,
      config: {
        devtools: process.env.NODE_ENV === 'development'
      }
    },
    postcss: {
      variables
    },
    globals: {
      '@mctech/vue-kaka-grid': 'VueKakaGrid'
    }
  },
  sourcemap: true,
  clear: ['dist'],
  
 //web-content/template/page.html 中script引入的需要copy一下,不然找不到文件
 
  copy: {
    'src/server/bootstrap.yml': 'dist/bootstrap.yml',
    'src/server/individual.yml': 'dist/individual.yml',
    'node_modules/vue/dist': 'dist/web-content/lib/vue',
    'node_modules/vuex/dist': 'dist/web-content/lib/vuex',
    'node_modules/vue-router/dist': 'dist/web-content/lib/vue-router',
    'node_modules/axios/dist': 'dist/web-content/lib/axios',
    'node_modules/element-resize-detector/dist':
    'dist/web-content/lib/element-resize-detector',
    'node_modules/@mctech/kaka-grid/dist': 'dist/web-content/lib/kaka-grid',
    'node_modules/@mctech/mussel/dist': 'dist/web-content/lib/mussel',
    'node_modules/@mctech/perfect-scrollbar/dist':
    'dist/web-content/lib/perfect-scrollbar',
    'node_modules/@mctech/vue-kaka-grid/dist':
    'dist/web-content/lib/vue-kaka-grid',
    'node_modules/@mctech/web-frame/dist': 'dist/web-content/lib/web-frame',
    'node_modules/@mctech/infra-cloud/src/error/templates':
    'dist/web-content/templates',
    'src/web-content/asset': 'dist/web-content/asset',
    'src/web-content/utils': 'dist/web-content/utils'
  },

//module 里面所有的组件必须注册入口和打包后的出口

  entries: {
    // 服务
    server: {
      input: 'src/server/index.js',
      output: 'dist/app.js'
    },
    change_part: {
      title: '变更部位统计与计算',
      input: 'src/web-content/module/change-part/index.js',
      output: 'dist/web-content/assets/change-part.js'
    },
    change_management_account: {
      title: '变更管理台账',
      input: 'src/web-content/module/change-management-account/index.js',
      output:'dist/web-content/assets/change-management-account.js'
    },
    change_cost: {
      title: '变更费用申请',
      input: 'src/web-content/module/change-cost/index.js',
      output: 'dist/web-content/assets/change-cost.js'
    },
    change_category_dictionary: {
      title: '变更类别字典',
      input: 'src/web-content/module/change-category-dictionary/index.js',
      output: 'dist/web-content/assets/change-category-dictionary.js'
    },
    change_account_config: {
      title: '变更统计台账配置',
      input: 'src/web-content/module/change-account-config/index.js',
      output: 'dist/web-content/assets/change-account-config.js'
    },
    change_bill_account:{
      title: '清单变更台账',
      input: 'src/web-content/module/change-bill-account/index.js',
      output:'dist/web-content/assets/change-bill-account.js'      
    },
    change_material_account: {
      title: '材料变更台账',
      input: 'src/web-content/module/change-material-account/index.js',
      output: 'dist/web-content/assets/change-material-account.js'
    },
    change_document_template: {
      title: '变更文件范本',
      input: 'src/web-content/module/change-document-template/index.js',
      output: 'dist/web-content/assets/change-document-template.js'
    }
  }
}
```

### 部署与发布

##### 模块注册
![[Pasted image 20230412165846.png]]
模块编码：项目名-模块名
模块地址：
	mcpack.config.js / output地址 :`output: 'dist/web-content/assets/change-category-dictionary.js'`
	模块地址：`/tp/change-category-dictionary.html` **html**

应用配置 添加菜单
![[Pasted image 20230412171136.png]]

##### 权限
![[Pasted image 20230412170538.png]]

### 组件
#### kaka-grid 例子

E:\project\pc-public-website\node_modules\@mctech\kaka-grid\examples
- field 表格里放的数据


iyvjaopkasodcml;a


