## 1 get 请求

```javascript
$.get(url, [data], [callback], [dataType])
复制代码
```
- url:string,请求的URL 地址
- data:对象,请求携带的参数
- callback:回调函数,载入成功时回调函数
- type:设置返回内容格式，json,xml, html, script, text, _default
```js
    $('button').eq(0).click(function() {
        $.get("http://127.0.0.1:8000/jquery", {
            a: 100,
            b: 200
        }, function(data) {
            // 直接写data出现[Object,Object]。用JSON转换一下
            $('div').text(JSON.stringify(data));
            console.log(data);
        }, 'JSON');
    });
```

## 2 post 请求

```javascript
$.post(url, [data], [callback], [dataType])
复制代码
```
-   url:请求的URL 地址
-   data:请求携带的参数
-   callback:载入成功时回调函数
-   type:设置返回内容格式，xml, html, script, json, text, _default

```js
    $('button').eq(1).click(function() {
        $.post("http://127.0.0.1:8000/jquery", {
            a: 100,
            b: 200
        }, function(data) {
            // 直接写data出现[Object,Object]。用JSON转换一下
            $('div').text(JSON.stringify(data));
            console.log(data);
        }, 'JSON');
    });
```

## 3 通用方法

```javascript
$.ajax({
	// url (xhr.open(type.url))
	url: 'http://127.0.0.1:8000/jquery-server',
	// 参数 (url里或者send())
	data: {a:100, b:200},
	// 请求类型
	type: 'GET',
	// 响应体结果数据类型
	// 把返回的JSON字符串转化为js数据
	//`responseType`
	dataType: 'json',
	// 成功的回调
	success: function(data){console.log(data);},
	// 超时时间,超时则取消
	timeout: 2000,
	// 失败的回调
	//`onerror`,`ontimeout`
	error: function(){console.log('出错拉~');},
	// 请求头头信息
	//`setRequestHeader(name, value)`
	headers: {
		c: 300,
		d: 400
	}	
})
```
`$.ajax`是一个函数,接受一个参数.此参数为Object
  
 