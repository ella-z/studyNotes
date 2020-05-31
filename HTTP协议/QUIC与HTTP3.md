# QUIC与HTTP3 
<img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/QUIC%E4%B8%8EHTTP3%20.PNG" alt="" width="600px" height="400px">

### HTTP2.0的问题
- 队头阻塞。在正常情况下，在同一个域名下只需要一个TCP连接，但若连接中出现了丢包的情况，那么整个TCP
都要等待重传，后面的数据也就全部被阻塞住了。对于HTTP1.1来说，因为HTTP1.1是一次通信建立一次TCP连接
，就算其中一个TCP出现了丢包的情况，但是后面的数据也不会被阻塞。
- 建立连接的握手延迟大。对于短链接来说，连接的握手延迟大。

### QUIC
<br>
<img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/QUIC%E7%9A%840RTT%20.PNG" alt="" width="500px" height="300px">

- 0 RTT，显著地降低了延迟 。QUIC协议通过使用类似TCP快速打开的技术，缓存当前会话的上下文，在下次恢复会话的时候，只需将之前的缓存发送给服务器验证通过后，就可以进行传输了。
- 没有对头阻塞的多路复用。
<br>
<img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/TCP.PNG" alt="TCP" width="500px" height="300px">
<img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/QUIC%2BUDP.PNG" alt="QUIC+UDP" width="500px" height="300px">

   + 在TCP中stream2的第三个包丢失了，TCP为了保证数据的可靠性，需要发送端重传stream2的第三个包，才能够通知应用层读取接下来的数据，虽然stream3与stream4的全部数据都已经抵达到了接收端，但是被阻塞住了。
   + 因为QUIC是基于UDP的，一个连接上的stream之间没有什么依赖。虽然stream2的第三个包还是需要重传，但是stream3与stream4无需等待就可以直接发送给用户了。
   + TCP是基于端口与IP识别连接的，QUIC是根据ID来识别连接的。
- 前向纠错，每个数据包除了本身的内容外，还包括了部分其他包的内容，因此少量的丢包，可以通过其他包的冗余数据直接组装而无需重传(只能使用在一个包丢包的情况下)。
