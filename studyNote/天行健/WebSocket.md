HTTP: 服务端不能主动向客户端推送消息
WebSocket：h5开始提供的在单个tcp连接上进行的 **全双工** 通信协议

## 实现方式

WebSocket 需要和tcp一样， 先建立链接， 客户端、服务端握手链接， 连接成功后才能相互通信

1. WebSocket 构造函数
```JS
var wa = new webSocker("相关服务器地址")
```
执行上面语句后，客户端就会与服务器进行链接

2. webSocket.readState 实例对象当前状态
```JS
CONNECTING:0 正在连接
OPEN:1 连接成功，可以通信
CLOSING:2 连接正在关闭
CLOSED:3 连接已经关闭||打开连接失败
```
3. webSocket.onopen 指定连接成功后的回调函数
```js
ws.onopen = function(){
alert('ws连接成功！')
}
```
4. webSocket.onerror 指定连接失败后的回调函数
```js
ws.onerror = function(){
alert('ws连接失败！')
}
```
3. webSocket.onmessage 指定收到服务器数据后的回调函数
```js
ws.onmessage= function(data){
alert(data)
}
```