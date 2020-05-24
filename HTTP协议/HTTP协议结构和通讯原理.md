# HTTP协议结构和通讯原理
### HTTP协议的特点
1. 支持客户/服务器模式。
   - 客户/服务器模式工作的方式是由客户端向服务器发出请求，服务器端响应请求，并进行相应的服务。
   <br />
   <img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/%E5%AE%A2%E6%88%B7%E7%AB%AF%E4%B8%8E%E6%9C%8D%E5%8A%A1%E5%99%A8.PNG" alt="客户端/服务器" width="300" height="200" align="center" />
2. 简单快速
   - 客户向服务器请求服务时，只需传送请求方法和路径。
   - 请求方法常用的有GET、POST、HEAD。每种方法规定了客户端与服务器联系的类型不同。
   - 由于HTTP协议简单，使得HTTP服务器的程序规模小，因而通信速度很快。
3. 灵活
   - HTTP允许传输任意类型的数据对象。
   - 正在传输的类型由Content-Type(Content-Type是HTTP包中用来表示内容类型的标识)加以标记。
4. 无连接
   - 无连接的含义是限制每次连接只处理一个请求。
   - 服务器处理完客户端的请求，并受到客户的应答后，即断开连接。
5. 无状态
   - HTTP协议是无状态协议。
   - 无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。在另一方面，在服务器不需要先前信息时它的应答就较快。
   
### URI与URL
<img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/URI.PNG" alt="URI" width="200" height="100" align="center" />

- URI：一个紧凑的字符串用来标示抽象或物理资源。URI可以分为URL、URN或同时具备locators和names特性的一个东西，而URN的作用就好像一个人的名字，URL就像一个人的地址。换个方式理解就是，URN确定了东西的身份，URL提供了找到它的方式。
- URL：是URI的子集，除了确定一个资源，还提供一种定位该资源的主要访问机制。URL是URI的一种，但不是所有的URI都是URL。

