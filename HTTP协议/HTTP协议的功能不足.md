### HTTP协议的功能不足
- 一条连接上只可发送一个请求。
- 请求只能从客户端开始，客户端不可以接收除响应以外的指令。
- 请求/响应头部不经压缩就发送。
- 每次互相发送相同的头部造成的浪费较多。
- 非强制压缩发送。是否压缩发送时可选择的。

### 影响HTTP网络请求的因素
- 带宽
- 延迟
   1. 浏览器的阻塞导致延迟，浏览器对于同一个域名一般只能有四个链接(根据浏览器的内核决定)，超过浏览器的链接限制，后面的请求就会
   被阻塞。
   2. DNS的查询导致延迟。浏览器需要知道服务器的IP地址才能去链接，利用DNS将域名解析为IP需要时间可能导致延迟，不过可以利用DNS缓存
   结果来减少这个时间。
   3.  建立连接导致延迟。HTTP是基于TCP协议建立连接的，浏览器最快也要在第三次握手的时候发送请求的报文达到真正的建立连接，但是这些
   连接无法复用，会导致每次请求都需要经过三次握手来启动连接。在HTTP1.1中增加了缓存的处理以及长连接一定程度上缓解了延迟。
