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

### 报文
- 报文头分为：通用报文头、请求报文头、响应报文头和实体报文头。
   - 通用报文头：
   <img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/%E9%80%9A%E7%94%A8%E6%8A%A5%E6%96%87%E5%A4%B4.PNG" alt="通用报文头" width="300" height="100" align="center" />
   
   - 请求报文头：
   <img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/%E8%AF%B7%E6%B1%82%E6%8A%A5%E6%96%87%E5%A4%B4.PNG" alt="请求报文头" width="300" height="100" align="center" />
   
   - 响应报文头：
   <img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/%E5%93%8D%E5%BA%94%E6%8A%A5%E6%96%87%E5%A4%B4.PNG" alt="响应报文头" width="300" height="100" align="center" />
   
   - 实体报文头：
   <img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/%E5%AE%9E%E4%BD%93%E6%8A%A5%E6%96%87%E5%A4%B4.PNG" alt="实体报文头" width="300" height="100" align="center" />
   
#### 请求报文
<img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/%E8%AF%B7%E6%B1%82%E6%8A%A5%E6%96%87.PNG" alt="请求报文" width="500" height="300" align="center" />
  
- HTTP的请求报文的一些常见属性：
1. Accept
   - 作用：浏览器端可以接受的媒体类型。例如：Accept:text/html 代表浏览器可以接受服务器回发的类型为 text/html，也就是html文档。如果服务器无法返回text/html类型的数据，服务器应该返回一个406错误。(Non Acceptable)。
   - Accept:*/* 代表浏览器可以处理所有类型。
   - 给显示的媒体类型添加优先级时，使用q=来额外表示权重值，权重值q的范围是0~1(可精确到小数点后3位)，且1为最大值。不指定权重q值时，默认权重为q=1.0。当服务器提供多种内容时，将会首先返回权重值最高的媒体类型。
2. Accept-Encoding
   - 作用：浏览器申明自己接收的编码方法，通常指定压缩方法，是否支持压缩，支持哪种(gzip,deflate)压缩方法。
3. Accept-Language
   - 作用：浏览器申明自己接收的语言。
   - Accept-Language也可以设定权重值。
4. Connection
   - Connection:keep-alive 当一个网页打开完成后，客户端和服务器之间用于传输HTTP数据的TCP连接不会关闭，如果客户端再次访问这个服务器上的网页，会继续使用这一条已经建立的连接。
   - Connection:close 代表一个Request完成后，客户端和服务器之间用于传输HTTP数据的TCP连接会关闭，当客户端再次发送Request，需要重新建立TCP连接。
5. Host
   - 作用：请求报头域主要用于指定被请求资源的Internet主机和端口号，它通常从HTTP URL中提取出来的。例如：我们再浏览器中输入：http://www.baidu.com:8080 。那么浏览器发送的请求报文中，就会包括Host请求报头域：Host：www.baidu.com:8080。
6. Referer
   - 作用：当浏览器向web服务器发送请求的时候，一般都会带上Rerferer，告诉服务器请求是从那个页面连接过来的，服务器借此可以获得一些信息用于处理。
7. User-Agent
   - 作用：告诉HTTP服务器，客户端使用的操作系统和浏览器的名称和版本。
8. Content-Type
   - 作用：说明了报文体内对象的媒体类型。
   <br />
   <img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/%E5%AA%92%E4%BD%93%E7%B1%BB%E5%9E%8B2.PNG" alt="媒体类型" width="300" height="100" align="center" />
   <img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/%E5%AA%92%E4%BD%93%E7%B1%BB%E5%9E%8B.PNG" alt="媒体类型" width="300" height="100" align="center" />

- HTTP的请求方法
+ 常用的：
1. GET方法
   - 用来请求访问已被URI识别的资源，指定的资源经服务器端解析后返回响应内容。
   - GET方法也可以用来提交表单和其他数据，但是对于不同的浏览器对于提交的URL的长度也有所限制。
2. POST方法
   - 一般用来传输实体的主体。
   - 克服了GET方法中数据无法保密以及数据量太小的问题。
- GET方法与POST方法的区别：
   1. 在客户端，GET方法是通过url来提交数据，POST方法数据是放在报文体中提交。
   2. GET方法提交数据有大小限制，而POST请求没有大小限制。
   3. 在安全性方面，POST方法比GET方法更具安全性，这种安全性是相对的，并不是绝对的。无论是GET方法还是POST方法在请求的时候都有被拦截的可能。
+ 不常用的：
1. PUT方法
   - 从客户端向服务器传送的数据取代指定的文档的内容。
   - PUT方法与POST方法最大的不同是：PUT是幂等的，而POST是不幂等的(幂等就是不管执行多少次重复操作都是实现相同的解，幂等是覆盖的)。
2. HEAD方法
   - 类似于GET请求，只不过返回的响应中没有具体的内容，用于获取报头。
3. DELETE方法
   - 请求服务器删除特定的资源。
4. OPTIONS方法
   - 用来查询针对请求URI指定的资源支持的方法。
     
#### 响应报文
<img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/%E5%93%8D%E5%BA%94%E6%8A%A5%E6%96%87.PNG" alt="响应报文" width="500" height="300" align="center" />

- 状态码：
<img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/HTTP%E7%8A%B6%E6%80%81%E7%A0%81%E8%AF%A6%E8%A7%A3.PNG" alt="状态码详解" width="500" height="300" align="center" />

+ 常见的HTTP状态码：
   - 200 OK：请求已成功，请求所希望的响应头或数据体将随此相应返回。
   - 202 Accpeted：已接受，已经接收请求，但未处理完成。
   - 206 Partial Content：部分内容，服务器成功处理了部分GET请求。
   - 301 Moved Permanently：永久移动，请求的资源已被永久的移动到新的URI，返回信息会包括新的URI，浏览器会自动定向到新的URI。今后任何新的请求都应使用心得URI代替。
   - 302 Found：临时移动，与301类似，但资源只是临时被移动/客户端应继续使用原有的URI。
   - 400 Bad Request：客户端请求的语法错误，服务器无法理解。
   - 401 Unauthorized：请求要求用户的身份认证。
   - 403 Forbidden：服务器理解请求客户端的请求，但是拒绝执行此请求。
   - 404 Not Found：服务器无法根据客户端的请求找到资源(网页)。
   - 500 Internal Server Error：服务器内部错误，无法完成请求。
   - 502 Bad Gateway：充当网关或代理服务器，从远端服务器接收到了一个无效请求。
   






