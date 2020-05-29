# WebSocket与HTTP
#### WebSocket与HTTP区别
- WebSocket弥补了HTTP协议的功能不足。
- 相对于HTTP协议来说，WebSocket是一个持久化的协议。
- 在HTTP协议中的监听服务器的内容可以使用AJAX轮询以及Long Poll，但是AJAX轮询需要服务器有很快的处理速度，来处理AJAX的请求。LongPull需要服
务器有很高的并发，无论是AJAX轮询还是Long Poll都十分消耗资源，并且同步有延迟。
而在WebSocket协议中，当服务器的内容发生改变时，会主动发起response给客户端。
- 在HTTP协议中，一个request对应着一个response，并且这个response是被动的，不能主动发起response。而在WebSocket协议中，服务器
可以主动发起response给客户端。
- HTTP是是一种单向通信协议，而WebSocket是一种双向通信协议。
- HTTP协议每请求一次就要连接一次，而WebSocket在整个通信过程中只需要一次连接，避免了HTTP协议的非状态性。

### WebSocket
- WebSocket是一种全双工通信协议。
- WebSocket协议建立于TCP协议之上。
- 与 HTTP 协议有着良好的兼容性。默认端口也是80和443，并且握手阶段采用 HTTP 协议，因此握手时不容易屏蔽，能通过各种 
HTTP 代理服务器。
- 可以发送文本，也可以发送二进制数据。
- 没有同源限制，客户端可以与任意服务器通信。
<br />
<img src="" alt="WebSocket" width="700px" height="500px">



#### WebSocket的握手请求
<br />
<img src="" alt="WebSocket的握手请求" width="500px" height="300px">

```
//告知代理服务器使用的是WebSocket协议
Upgrade：websocket //表示要升级协议
Connection：Upgrade //表示要升级到websocket协议

Sec-WebSocket-Protocol：chat，superchat //用户定义的字符串，来区分在同一个url下不同的服务器所需要的协议。
Sec-WebSocket-Key//与后面服务端响应首部的Sec-WebSocket-Accept是对应的，提供基本的防护(使用base64进行了加密)，防止恶意的
连接，或者无意的连接。
Sec-WebSocket-Version: 13//表示websocket的版本。如果服务端不支持该版本，需要返回一个Sec-WebSocket-Versionheader，里面包含
服务端支持的版本号。

```

#### WebSocket的握手响应
<br />
<img src="" alt="WebSocket的握手响应" width="500px" height="200px">

```
//状态代码101表示协议切换
101

//告知客户端协议已经升级为websocket了
Connection:Upgrade
Upgrade: websocket

//表示最终所使用的协议
Sec-WebSocket-Protocol：chat

//Sec-WebSocket-Accept根据客户端请求首部的Sec-WebSocket-Key计算出来。
Sec-WebSocket-Accept

```







