# HTTP状态管理
### Cookie
- Cookie实际上是一小段的文本信息。客户端请求服务器，如果服务器需要记录该用户的状态，就向客户端浏览器发送个Cookie。浏览器会把Cookie保存起来。
当浏览器再次请求该网站时，浏览器把请求的网址连同该Cookie一同提交给服务器。服务器检查该Cookie，以此来辨认用户状态。
<br />
<img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/Cookie%E7%9A%84%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86.PNG"  alt="cookie的工作原理" width="500px" height="300px" />

### Session
- Session是另一种记录客户状态的机制，保存在服务器上。客户端浏览器访问服务器的时候，服务器把客户端信息以某种形式记录在服务器上。当客户端浏览
器再次访问时只需要从该Session中查找该客户的状态就可以了。与Cookie相比更加简单，但是也加重了服务器的压力。
- 保存Session ID的方式
   + 使用Cookie存储
   + URL重写，在URL的后面添加上Session ID的相关信息
   + 隐藏表单，服务器自动修改表单添加一个隐藏的字段，表单提交之后，隐藏的ID能够传递到服务端上。
- Session的有效期
   + Session超时失效，服务器会自动把长时间没有活跃的Session从服务器上删除掉。
   + 程序调用HttpSession.invalidate()，相应的Session会主动失效。
   + 若服务器的进程被异常终止了，那么在服务器中的Session也会失效。
<br />
<img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/Session%E7%9A%84%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86.PNG"  alt="Session的工作原理" width="500px" height="300px" />

### Cookie与Session
- 存放的位置不同，Cookie存放在浏览器中，Session存放在服务器中。
- 安全性(隐私策略)的不同，Cookie存放在浏览器中对于客户端是可见的，Session存放在服务器中，对于客户端来说它是透明的，不存在敏感信息泄露的风险。
- 有效期的不同，Cookie通过设置有效期，可以在浏览器上保存很长时间，而服务器会定时清理长时间没有被访问过的SessionID，避免服务器出现过大的压力。
- 对于服务器的压力不同，若并发产生十分多的Session，会耗费我们大量的内存，而Cookie保存在客户端不太占用服务器的资源。
