
[【从前端到全栈】- koa快速入门指南 - 掘金 (juejin.cn)](https://juejin.cn/post/6844903695637839885?searchId=202309121349323D26CBA5CE41E2ABDA10)

## 概述

- Koa2 是nodejs web开发框架

[Koa (koajs) -- 基于 Node.js 平台的下一代 web 开发框架 | Koajs 中文文档 (bootcss.com)](https://koa.bootcss.com/)

```
npm init -y
npm install koa --save
```

使用脚手架

```
# 全局安装
npm i -g koa-generator 
# 查看版本
koa2 --version
# 创建项目
koa2 name  
```

启动

```
npm run dev
```

无法启动，报错
```
> ./node_modules/.bin/nodemon bin/www

'.' 不是内部或外部命令，也不是可运行的程序
或批处理文件
```
解决: 更改路径
```
.\\node_modules\\.bin\\nodemon bin/www

```


## 中间件

- koa2中，路由的本质就是一个中间件

```
npm i koa-router --save
```

### 控制器

- 拿到路由分配的功能，并执行

功能
1. 获取http请求参数
2. 处理业务逻辑

编写控制器的最佳实践
1. 每个资源的控制器都放在不同的文件里
2. 尽量使用类+类方法的形式编写控制器
3. 严谨的错误处理

#### 获取http请求

> 获取post请求 需要使用中间件 `koa-bodyparser`
## rest 风格

- REST ：Representational State Transfer 

### 限制

#### 六个限制

1. Client/Server 架构
	- 关注点分离
	- 服务端专注于数据存贮，提升了简单性
	- 前端专注于用户界面，提升了可以执行

2. 无状态 Stateless
	- 所有用户会话信息都保存在客户端
	- 每次请求必须包含所有信息
	- 不能依赖上下文信息
	- 服务端不用保存会话信息
	- 简单性、可靠性、可见性

3. 缓存 Cache
	- 所有服务端响应都要被标记为可缓存或不可缓存
	- 减少前后端交互，提升性能

4. 统一接口 Uniform Interface
	- 接口设计尽可能统一使用
	- 接口实现解耦，前后端独立开发

5. 分层系统 Layered System
	- 每层只知道相邻的一层（eg：客户端只知道接口不知道是和代理还是真实服务器通信）

6. 按需代码 Code-On-Demand
	- 客户端可以下载运行服务端传来的代码
	- 通过减少一些功能，简化客户端


#### 统一接口的限制

1. 资源的标识
	- 资源是任何可以被命名的事务
	- 每个资源可以通过url（统一资源定位符）被唯一标识

2. 通过表述Representation来操作资源
	- 客户端不能直接操作服务端资源，要通过表述来操作资源
	- 表述： eg：JSON

3. 自描述信息
	- 每个消息必须提升共足够的信息让接受者理解
	eg ： 
		媒体信息 application/json
		http方法 get/post
		是否缓存 Cache-Control
 
4. 超媒体作为应用状态引擎 （点击链接跳转到另一个网页）
	- 超媒体：带文字的链接
	- 应用状态：一个网页
	- 引擎：驱动、跳转
	


### restful API

- 基本的 URI
- 标准的HTTP方法
- 传输的数据媒体类型。eg. :JSON、XML

1. 请求设计规范
	- URI使用名词，尽量使用复数， 如 /users
	- URI使用嵌套表示关联方案，如 /users/12/repos/5
	- 使用正确的HTTP方法， 如GET/POST/PUT
	- 不符合CRUD(增删改查)的情况：POST/action/子资源 

2. 响应设计规范
	- 查询
	- 分页 （返回列表较长应该加入分页信息）
	- 字段过滤 （只能返回指定的字段）
	- 状态码
	- 错误处理

3. 安全规范
	- https
	- 鉴权
	- 限流

4. 开发者友好
	- 文档
	- 超媒体






## 错误处理机制
