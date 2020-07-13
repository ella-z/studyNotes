# Http协议基础
- HTTP(超文本传输协议)：是一种通信协议，它允许将超文本标记语言(HTML)文档从web服务器传送到客户端的浏览器。
- HTTP协议是一个属于应用层的面向对象的协议。
- HTTP协议是构建再TCP/IP协议之上的，是TCP/IP的一个子集。
- HTTP事务处理过程：当客户端访问web站点时，首先会通过DNS服务查询到域名的IP地址，浏览器就会根据IP地址再与web服务器进行通信，而这个通信得协议就是http协议。然后浏览器生成HTTP请求（请求报文），并通过TCP/IP协议发送给web服务器，web服务器接收到请求后会根据请求生成的响应内容（响应报文），并通过TCP/IP协议返回给客户端，浏览器再根据响应报文渲染页面。
<br />
<img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/HTTP%E4%BA%8B%E5%8A%A1%E5%A4%84%E7%90%86%E8%BF%87%E7%A8%8B.PNG" alt="HTTP事务处理过程" width="500" height="300" align="center" />

### 域名
- 域名：是由一串用点分隔的名字组成的Internet上某一台计算机或计算机组的名称，用于在数据传输时对计算机的定位标识。例如：baidu.com就是一个域名。
   - 域名级数：是指一个域名由多少级组成，域名的各个级别被“.”分开，最右边的那个词称为顶级域名。例如：.com就是一级域名，baidu.com就是二级域名。

### IP地址
- IP地址：是IP协议提供的一种统一的地址格式，它为互联网上的每一个网络和每一台主机分配一个逻辑地址，以此来屏蔽物理地址的差异。

### TCP/IP
- TCP/IP协议是一系列与互联网相关的协议集合起来的总称。
- TCP/IP协议族是由一个四层协议组成的系统，这四层分别为：应用层、传输层、网络层和数据链路层。
   1. 应用层：一般是我们编写的应用程序，决定了向用户提供的应用服务。应用层可以通过系统调用与传输层进行通信。如：FTP、DNS、HTTP。
   2. 传输层：通过系统调用向应用层提供处于网络连接中的两台计算机之间的数据传输功能。在传输层的两个性质不同的协议：TCP和UDP(TCP是面向链接的
   (在客户端和服务器之间传输数据之前要先建立连接)，UDP是无连接的)。
      - TCP三次握手： 
          <br />
         <img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/%E4%B8%89%E6%AC%A1%E6%8F%A1%E6%89%8B.PNG" alt="三次握手" width="500" height="300" align="center" />
         1. 第一次握手：客户端发送带有SYN标志的连接请求报文段，然后进入SYN_SEND状态，等待服务端的确认。
         2. 第二次握手：服务端接收到客户端的SYN报文段后，需要发送ACK信息对这个SYN报文段进行确认。同时，还要发送自己的SYN请求信息。服务段会将
         上述的信息放到一个报文段(SYN+ACK报文段)中，一并发送到客户端，此时服务端将会进入SYN_RECEIVED状态。
         3. 第三次握手：客户端接收到服务端的SYN+ACK报文段后，会向服务端发送ACK确认报文段，这个报文段发送完毕后，客户端与服务端都进入ESTABLISHED状态，完成TCP三次握手。
      - [三次握手与四次挥手](https://zhuanlan.zhihu.com/p/53374516)  
      - [详解三次握手与四次挥手](https://baijiahao.baidu.com/s?id=1654225744653405133&wfr=spider&for=pc)
   3. 网络层：用来处理在网络上的流动数据包，数据包时网络传输的最小数据单位，该层规定了通过怎样的路径(传输路线)到达对象的计算机，并且把数据包
      传输给对方。
      <br />
      <img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/%E6%95%B0%E6%8D%AE%E5%8C%85%E7%9A%84%E5%B0%81%E8%A3%85%E8%BF%87%E7%A8%8B.PNG" alt="数据包的封装过程" width="500" height="300" align="center" />
       
   4. 链路层：用来处理链接网络的硬件部分，包括控制操作系统、硬件设备驱动、网络适配器以及光纤等物理课件部分。硬件上的范畴均在链路层的作用范围
      之内。
   <br />
   <img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/HTTP%E6%95%B0%E6%8D%AE%E7%9A%84%E4%BC%A0%E8%BE%93%E8%BF%87%E7%A8%8B.PNG" alt="HTTP数据的传输过程" width="500" height="300" align="center" />
   
      
### DNS域名解析
- DNS提供域名与IP地址之间的解析服务。
- DNS的服务是具有层次的，并且采用了就近原则。
   - 在进行DNS解析的时候，系统会优先从host文件中去寻找对应的IP地址，若没有，就到本地的DNS服务器中寻找，再没有，就到上一级DNS服务器中寻找直至根DNS服务器。
<br />
<img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/DNS%E5%9F%9F%E5%90%8D%E8%A7%A3%E6%9E%90.PNG" alt="DNS域名解析" width="500" height="300" align="center" />




