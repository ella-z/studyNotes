# HTTP2.0
<img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/HTTP2.PNG" alt="HTTP2" width="400px" height="500px">

- HTTP性能增强的核心： 二进制分帧
<br />
<img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/HTTP2%E4%BA%8C%E8%BF%9B%E5%88%B6%E5%88%86%E5%B8%A7.PNG" alt="二进制分帧" width="600px" height="400px">

### 首部压缩
- 首部压缩：HTTP2.0在客户端和服务器使用一种首部表来跟踪和存储之前发送的键值对，对于相同的数据不再通过每次请求和响应发送，而在通讯期间几乎都不会改变通用的键值对。若这个请求中不包括首部，那么首部的开销就为0子节。此时所有请求的首部都使用之前发送请求的那个首部。🌰：
<br />
<img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/HTTP2.0%E9%A6%96%E9%83%A8%E5%8E%8B%E7%BC%A9.PNG" alt="HTTP2首部压缩" width="500px" height="400px">

- 由于在发送request2的时候，发现只有path的值改变了，其他数据都没有变化。所以在首部发生变化的时候，只需要发送变化的数据在HEADERS帧里面。对于新增和修改的首部帧会被再追加到一个首部表里面，这个首部表会不断更新。这个首部表在HTTP2.0是始终存在的，在客户端和服务器共同渐进地更新。
- HTTP1.1与HTTP2.0的header的区别：
   + HTTP1.1里面的header里面含有大量的信息，每次都需要重复发送。HTTP2.0使用压缩的方式来减少需要传送数据的大小。通信双方各缓存一份首部表，避免了重复的header数据的传输，减少需要传送数据的大小。
   
### 多路复用
- HTTP2.0所有通信都在一个TCP连接上完成，HTTP2.0将HTTP的通信单位缩小为帧，这些帧对应着逻辑流的信息，并且在同一个TCP上双向交换信息。
- 优势：
   + 可以减少服务链接压力，内存占用少了，连接吞吐量大了。
   + 由于TCP连接减少而使网络拥塞状况得以改观。
   + 慢启动时间减少，拥塞和丢包恢复速度更快。

<img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/HTTP2.0%E5%A4%9A%E8%B7%AF%E5%A4%8D%E7%94%A8.PNG" alt="多路复用" width="600px" height="300px">

### 并行双向字节流的请求和响应
<img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/%E5%B9%B6%E8%A1%8C%E5%8F%8C%E5%90%91%E5%AD%97%E8%8A%82%E6%B5%81%E7%9A%84%E8%AF%B7%E6%B1%82%E5%92%8C%E5%93%8D%E5%BA%94.PNG" alt="并行双向字节流的请求和响应" width="500px" height="300px">

- 在HTTP2.0中，客户端和服务器可以把HTTP的消息分解成互不依赖的帧，然后乱序发送，最后在另一端将它们重新组合。同一个连接上有多个不同方向的数据流在传输，所以客户端可以一边乱序发送数据流，然后一边接收服务器的响应，服务器也一致都是双向的。
- 优势：
   + 并行交错地发送请求，请求之间互不影响。
   + 并行交错地发送响应，响应之间互不干扰。
   + 只使用一个连接即可并行发送多个请求和响应。
   + 消除不必要的延迟，减少页面加载的时间。
   
### 请求优先级
- 高优先级的流都应该优先发送，但优先级不是绝对的，因为绝对的遵守有可能导致首部的堵塞，所以不同优先级混合也是必须的。

### 服务器推送
<img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%8E%A8%E9%80%81.PNG" alt="服务器推送" width="500px" height="300px">

- 因为服务器可以对客户端一个请求发送多个响应，所以服务器除了对最初请求响应之外，服务器还可以给客户端额外推送其他资源无需客户端明确的请求。

### HTTP2.0的缺点
- 队头阻塞。在正常情况下，在同一个域名下只需要一个TCP连接，但若连接中出现了丢包的情况，那么整个TCP 都要等待重传，后面的数据也就全部被阻塞住了。对于HTTP1.1来说，因为HTTP1.1是一次通信建立一次TCP连接 ，就算其中一个TCP出现了丢包的情况，但是后面的数据也不会被阻塞。
- 建立连接的握手延迟大。对于短链接来说，连接的握手延迟大。


   
   
   
   
   
   

