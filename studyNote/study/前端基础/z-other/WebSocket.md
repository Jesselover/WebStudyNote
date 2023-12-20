### questions

1. 有些手机不支持websocket？
["WebSocket" | Can I use... Support tables for HTML5, CSS3, etc](https://caniuse.com/?search=WebSocket)（ws兼容性）
# stduyNote

[万字长文，一篇吃透WebSocket：概念、原理、易错常识、动手实践 - 即时通讯开发 - SegmentFault 思否](https://segmentfault.com/a/1190000040793931)

## 概念

- WebSocket 是一种网络传输协议，位于 OSI 模型的应用层。
- 允许服务端主动向客户端推送数据，一次握手，持久连接、双向通信。可在单个 TCP 连接上进行全双工通信。
- Websocket 使用 ws 或 wss 的统一资源标志符（URI），其中 wss 表示使用了 TLS [^TLS]的 Websocket。
```
ws://echo.websocket.org  
wss://echo.websocket.org 
```
websocket默认使用80端口，运行在tls上，默认使用443端口
[^TLS]: : SSL安全套接字协议层，是基于HTTP之下TCP之上的一个协议层，是基于HTTP标准并对TCP传输数据时进行加密，所以HPPTS是HTTP+SSL/TCP的简称。TLS（传输层安全）是更为安全的升级版 SSL。


## 使用

1. 创建 WebSocket 对象
 ```JS
       socketObj: {
        socket: null, // WebSocket请求
        webSocketSuccess: false, // WebSocket请求是否链接成功
        wsUrl: "wss://dongzhuang.sqtxj.com:17003/dongzhuang-ywzt/websocket",
      },
```
```JS

```