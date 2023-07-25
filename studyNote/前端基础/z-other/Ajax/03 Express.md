# 3. Express
## 1.初始化
1. 初始化环境
`npm init --yes`

2.下载安装express包
`npm install express --save`

3.在一个js文件里(eg:`express.js`)按步骤使用express框架
```js
// 1. 引入express
const expres[00 promise基础](00%20promise基础.md)s = require('express');

// 2. 创建应用对象
const app = express();

// 3. 创建路由规则
// request 是对请求报文的封装
// response 是对响应报文的封装
app.get('/', (request, response) => {
  //  设置响应
  response.send("Hello Express");
});

// 4. 监听端口，启动服务
app.listen(8000, () => {
  console.log("服务已经启动, 8000 端口监听中...");
 })

```

4. 运行:利用node打开这个js文件 (eg:`node express.js`)
   查看网址:http://localhost:8000

1. 关闭端口:`ctrl+c`


## 2. 使用案例
### 1.get请求
#### 服务端
服务器端`server.js`
```js
// 1. 引入express
const express = require('express');

// 2. 创建应用对象
const app = express();

// 3. 创建路由规则
app.get('/server', (request, response) => {
  // 设置响应头 设置允许跨域
  response.setHeader('Access-Control-Allow-Origin', '*');
  // 设置响应体
  response.send("Hello Ajax");
});

// 4. 监听服务
app.listen(8000, () => {
  console.log("服务已经启动, 8000 端口监听中...");
 })
```
- 启动
	`node server.js`
- 打开网页
	http://localhost:8000/server
	%%一定要写"/server" 否则报错cannot get%%
- 传递参数 （query参数）
	`xhr.open('GET','http://127.0.0.1:8000/server?a=100&b=200&c=300');`
	查看:network-->Payload
#### 客户端
```html
<head>
...
  <style>
    #result {
      width: 200px;
      height: 100px;
      border: solid 1px #90b;
    }
  </style>
</head>
<body>
  <button>点击发送请求</button>
  <div id="result"></div>
  <script>
    //获取button元素
    const btn = document.getElementsByTagName('button')[0];
    const result = document.getElementById('result');
    //绑定事件
    btn.onclick = function(){
      // 1. 创建对象 
      const xhr = new XMLHttpRequest();
      // 2. 初始化 设置请求方法和url
      xhr.open('GET', 'http://127.0.0.1:8000/server')
      // 3. 发送
      xhr.send();
      // 4. 事件绑定 处理服务端返回的结果
      xhr.onreadystatechange = function(){
        // readyState 是 xhr 对象中的属性, 表示状态 ,有五个值
        // on表示 when 在...时候 
        // change 改变
        // onreadystatechange当readyState改变时,触发
        
        //判断 (服务端返回了所有的结果)
        if(xhr.readyState === 4){
          //判断响应状态码 200  404  403 401 500
          // 2xx成功
          if(xhr.status >= 200 && xhr.status < 300){
            // 处理结果 行 头 空行 体
            // 响应
            console.log('状态码', xhr.status); // 状态码
            console.log('状态字符串', xhr.statusText); // 状态字符串
            console.log('所有响应头', xhr.getAllResponseHeaders()); // 所有响应头
            console.log('响应体', xhr.response); // 响应体
            
            //设置 result 的文本
            result.innerHTML=xhr.response;
          }else{
          }
        }
      } 
    }
  </script>
</body>

 ```
- readyState:是 xhr 对象中的属性, 表示状态 ,5个值
	0:未初始化 
	1:open()调用完毕 
	2:send()调用完毕 
	3:服务端返回部分结果
	4:服务端返回所有结果

### 2.post请求
#### 服务端
```js
app.post('/server', (request, response) => {
  // 设置响应头, 设置允许跨域
  response.setHeader('Access-Control-Allow-Origin', '*');
  // 设置响应体
  response.send("Hello Ajax POST");
});

```
#### 客户端
```html
  <style>
    #result {
      width: 200px;
      height: 100px;
      border: solid 1px #903;
    }
  </style>
</head>
<body>
  <div id="result"></div>
  <script>
    // 获取元素对象
    const result = document.getElementById('result');
    // 绑定事件
    result.addEventListener("mouseover", function(){
      // 1. 创建对象
      const xhr = new XMLHttpRequest();
      // 2. 初始化 设置类型（请求方式）与url
      xhr.open('POST', 'http://127.0.0.1:8000/server');
      // 3. 发送   设置请求参数（请求体）
      xhr.send('a=100&b=200&c=300');
      // 4. 事件绑定
      xhr.onreadystatechange = function(){
        // 判断
        if(xhr.readyState === 4){
          if(xhr.status >=200 && xhr.status < 300){
            // 处理服务端返回的结果
            result.innerHTML = xhr.response;
          }
        }
      }
    });
  </script>
</body>
```
- 传递参数:
	1. `xhr.send('a=100&b=200&c=300');`
	2. `xhr.send('a:100&b:200&c:300');`
#### 设置请求头
`xhr.setRequestHeader(头的名字,头的值)`
```js
// 设置请求体内容的类型
xhr.setRequestHeader('Content-Type','application/x-www-from-urlencoded');
// 自定义头信息
xhr.setRequestHeader('name', 'ykyk');	
```
自定义头可能会报错,
1. 加上如下代码:
```js
response.setHeader('Access-Control-Allow-Headers','*');
```
2. OPTIONS请求无法回复:要在server.js中设置响应头允许自定义请求头 post改成all(表示可以接受任何类型的请求)
>[!TIP]
>一定要写在send之前

## 3.JSON请求
1. 手动转换
```js
        xhr.onreadystatechange = function() {
            if (xhr.readyState === 4) {
                if (xhr.status >= 200 && xhr.status < 300) {
                  
                     let data = JSON.parse(xhr.response)
                     console.log(data);
                     result.innerHTML = data.name;
                    // 自动转换 ：
                    // xhr.responseType = "json"
                    console.log(xhr.response);
                    result.innerHTML = xhr.response;
                }
            }
        }
```
2. 自动转换：设置响应体数据类型。数据类型要小写
```js
   const xhr = new XMLHttpRequest();
   xhr.responseType = "json";
   ...
   xhr.onreadystatechange = function() {
            if (xhr.readyState === 4) {
                if (xhr.status >= 200 && xhr.status < 300) {
                    console.log(xhr.response);
                    result.innerHTML = xhr.response;
                }
            }
        }
```

``
## 4.避免缓存问题（ie）

**问题**：在一些浏览器中(IE),由于缓存机制的存在，ajax 只会发送的第一次请求，剩余多次请求不会在发送给浏览器而是直接加载缓存中的数据。 **解决方式**：浏览器的缓存是根据url 地址来记录的，所以我们只需要修改url 地址即可避免缓存问题

```javascript
xhr.open("get","/testAJAX?t="+Date.now());
```

让url传递一个t=Date.now()时间戳

## 5.请求超时与网络异常
```js
// 超时设置 （2秒）,超时则取消
xhr.timeout = 2000;
// 超时回调，超时后调用此函数
xhr.ontimeout = function(){
	alert('网络超时，请稍后重试')
}
// 网络异常回调，网络异常调用此函数
xhr.onerror = function(){
	alert('网络异常，请稍后重试')
}
```
断网：network->offline

## 6.取消请求
```js
// 手动取消
xhr.abort()
```


## 7.避免重复请求
设立一个isSending布尔型变量的flag，判断当前是否有请求。有则取消当前请求，创建新请求
```html
<body>
    <button>点击发送请求</button>
    <div id="result"></div>
</body>
<script>
    const result = document.getElementById('result');
    const btnSend = document.getElementsByTagName('button')[0];
    const xhr = new XMLHttpRequest();
    let isSending = false;
    btnSend.onclick = function() {
        // 若正在发送，则取消当前请求，创建新请求
        if (isSending) xhr.abort();
        isSending = true;
        xhr.open('GET', 'http://127.0.0.1:8000/delay');
        xhr.send();
        xhr.onreadystatechange = function() {
            // xhr.readyState === 4 服务器返回所有结果
            if (xhr.readyState === 4) {
                // 必须写在这里，因为此请求可能失败
                isSending = false;
                if (xhr.status >= 200 && xhr.status < 300) {
                    result.innerHTML = xhr.response;
                }
            }
        }
    };
</script>
```
 # API总结

-   `XMLHttpRequest()`：创建 XHR 对象的构造函数
-   `status`：响应状态码值，如 200、404
-   `statusText`：响应状态文本，如 ’ok‘、'not found'
-   `readyState`：标识请求状态的只读属性 0-1-2-3-4
	%%0:未初始化,1:open()调用完毕,2:send()调用完毕 ,3:服务端返回部分结果,4:服务端返回所有结果%%
-   `onreadystatechange`：绑定 readyState 改变的监听
-   `responseType`：指定响应数据类型，如果是 'json'，得到响应后自动解析响应
-   `response`：响应体数据，类型取决于 responseType 的指定
-   `timeout`：指定请求超时时间，默认为 0 代表没有限制，超时则取消
-   `ontimeout`：绑定超时的监听
-   `onerror`：绑定请求网络错误的监听
-   `open()`：初始化一个请求，参数为：`(method, url[, async])`。
	%%`get` 在url中传递参数：eg ``?a=100&b=200&c=300`%%
-   `send(data)`：发送请求
	1. `xhr.send('a=100&b=200&c=300');`
	2. `xhr.send('a:100&b:200&c:300');`
-   `abort()`：中断请求 （发出到返回之间）
-   `getResponseHeader(name)`：获取指定名称的响应头值
-   `getAllResponseHeaders()`：获取所有响应头组成的字符串
-   `setRequestHeader(name, value)`：设置请求头