# 2. HTTP相关问题
- MDN 文档
	https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Overview
## 1.基本原理
-  HTTP 请求交互的基本过程
	前后应用从浏览器端向服务器发送HTTP 请求(请求报文)
	后台服务器接收到请求后, 调度服务器应用处理请求, 向浏览器端返回HTTP响应(响应报文)
	浏览器端接收到响应, 解析显示响应体/调用监视回调
	
### 超文本传输协议HTTP:

1. 定义了了浏览器(万维网客户进程)怎样向万维网服务器请求万维网文档,以及服务器怎样把文档传给浏览器.
		- 用户:输入url;点击超链接
		- 网络:建立tcp连接=>用户发送HTTP请求报文=>服务器发送HTTP响应报文=>释放TCP连接
		- 服务器:收到请求,相应请求;发送HTTP响应报文
2. 具体过程:
		- 浏览器分析URL
		- 浏览器向DNS请求解析IP地址
		- DNS解析出IP地址
		- 浏览器与服务器建立TCP连接
		- 浏览器发出取文件命令
		- 服务器响应
		- 释放TCP连接
		- 浏览器显示
3. 特点:
		- HTTP无状态,网站采用cookie识别用户
		- HTTP协议本身面向无连接(双方在交换报文前不需要建立HTTP连接),但其采用的传输层协议TCP面向连接.
4. 连接方式:非持久连接;持久连接(流水线式,非流水线式)
5. 报文:HTTP报文面向文本,报文中的每一个字段都是一些ASSCII码串
		![](study/前端基础/vue/图片/Pasted%20image%2020220826111602.png)
	

## 2. HTTP报文

### 1. HTTP 请求报文
1. 请求行:**请求类型:post,get url路径 HTTP协议版本**
	method url
	GET /product_detail?id=2
	POST /login
1. 多个请求头:**键值对形式**
	Host: www.baidu.com
	Cookie: BAIDUID=AD3B0FA706E; BIDUPSID=AD3B0FA706;
	Content-Type: application/x-www-form-urlencoded 或者application/json
1. 空行
2. 请求体:get为空(Query String Parameter,对url的解析),post(Form Data)不可以为空
	username=tom&pwd=123
	{"username": "tom", "pwd": 123}
	- post 请求体参数格式
	Content-Type: application/x-www-form-urlencoded;charset=utf-8
	用于键值对参数，参数的键值用=连接, 参数之间用&连接
	例如: name=%E5%B0%8F%E6%98%8E&age=12
	Content-Type: application/json;charset=utf-8
	用于 json 字符串参数
	例如: {"name": "%E5%B0%8F%E6%98%8E", "age": 12}
	Content-Type: multipart/form-data
	用于文件上传请求

### 2.HTTP 响应报文

1. 响应状态行: **HTTP版本 响应状态码 响应状态字符串**(与响应状态码对应)
	status statusText
2. 多个响应头:**键值对形式**
	Content-Type: text/html;charset=utf-8
	Set-Cookie: BD_CK_SAM=1;path=/
3. 空行
4. 响应体:主要返回内容
	html 文本/json 文本/js/css/图片...

### 3. 常见的响应状态码

200 OK 请求成功。一般用于GET 与POST 请求
201 Created 已创建。成功请求并创建了新的资源
401 Unauthorized 未授权/请求要求用户的身份认证
404 Not Found 服务器无法根据客户端的请求找到资源
500 Internal Server Error 服务器内部错误，无法完成请求
200 - 300 成功

### 4. 不同类型的请求及其作用

GET: 从服务器端读取数据（查）
POST: 向服务器端添加新数据 （增）
PUT: 更新服务器端已经数据 （改）
DELETE: 删除服务器端数据 （删）

## 3. 其他知识

2.8 API 的分类
REST API: restful （Representational State Transfer (资源)表现层状态转化）
(1) 发送请求进行CRUD 哪个操作由请求方式来决定
(2) 同一个请求路径可以进行多个操作
(3) 请求方式会用到GET/POST/PUT/DELETE
非REST API: restless
(1) 请求方式不决定请求的CRUD 操作
(2) 一个请求路径只对应一个操作
(3) 一般只有GET/POST

### 2.9 区别 一般http请求 与 ajax请求
ajax请求 是一种特别的 http请求
对服务器端来说, 没有任何区别, 区别在浏览器端
浏览器端发请求: 只有XHR 或fetch 发出的才是ajax 请求, 其它所有的都是非ajax 请求
浏览器端接收到响应
(1) 一般请求: 浏览器一般会直接显示响应体数据, 也就是我们常说的刷新/跳转页面
(2) ajax请求: 浏览器不会对界面进行任何更新操作, 只是调用监视的回调函数并传入响应相关数据