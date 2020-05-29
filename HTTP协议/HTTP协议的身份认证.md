# HTTP协议的身份认证
- 身份认证的信息：密码、动态令牌、数字证书、生物认证、IC卡等。

### 常见的认证方式
+ BASIC认证(基本认证)
   - 安全性等级较低，因为采用Base64的方式编码没有加密处理，所以不需要任何附加信息就可以对Base64进行解码。
   - 过程：
   <br />
   <img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/basic%E8%AE%A4%E8%AF%81%E7%9A%84%E8%BF%87%E7%A8%8B.PNG" alt="BASIC认证过程" width="400px" height="500px">
   
+ DIGEST认证(摘要认证)
   - 提供了密码的保护机制。
   - DIGEST认证与BASIC认证一样都是用了质询/响应的方式，但不会像BASIC认证那样直接发送明文密码。
   - 质询/响应的方式是指一方先发送认证要求给另一方，接着使用从另一方那收到的质询码，计算生成响应，最后将响应码返回给对方进行认证。
   <br />
   <img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/digest%E8%AE%A4%E8%AF%81%E7%9A%84%E8%BF%87%E7%A8%8B.PNG" alt="DIGEST认证过程" width="400px" height="500px">
   
+ SSL客户端认证
   - 有较高的安全等级。
   - SSL客户端认证是借由HTTPS的客户端证书完成认证的方式。凭借客户端证书认证，服务器可确认访问是否来自已登录的客户端。
   
+ FormBase认证(基于表单认证)
   - 目前使用最多的认证方式。
   - 基于表单的认证方式并不是在HTTP协议中定义的，而是使用Web应用程序各自实现基于表单的认证方式，通过Cookie和Session的方式来保持用户的状态。
   
