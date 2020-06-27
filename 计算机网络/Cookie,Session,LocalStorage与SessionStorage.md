# Cookie,Session,SessionStorage与LocalStorage
- [Cookie](#Cookie)
- [Session](#Session)
- [LocalStorage](#LocalStorage)
- [SessionStorage](#SessionStorage)
- [Cookie,Session,SessionStorage与LocalStorage的区别](#Cookie,Session,SessionStorage与LocalStorage的区别)

### Cookie
- cookie的主要作用就是保存信息，储存在浏览器中。
- cookie的组成：
   - name，一个唯一确定的cookie的名称。
   - value，存储在cookie的字符串值。
   - domain，cookie有效的作用域。
   - path，表示这个cookie影响到的路径，浏览器跟会根据这项配置，像指定域中匹配的路径发送cookie。
   - expires，cookie的失效时间，默认是浏览器关闭时失效。
   - max-age，过期倒计时(单位是秒)，正常情况下，max-age的优先级高于expires。
   - secure: 安全标志，指定后，只有在使用SSL链接时候才能发送到服务器，如果是http链接则不会传递该cookie。
- cookie具有不可跨域名性。

### Session
- session主要的作用也是保存信息，但与cookie不同的是，session将信息保存在服务器上。
- session是基于cookie的。
- session执行的过程：
   1. 产生sessionID：当客户端第一次访问服务端时，服务端随机产生一个随机数，命名为sessionID，并将其放在响应头里，以cookie
   的形式返回给客户端。
   2. 保存 sessionID：服务端将要保存的数据保存在相对应的sessionID之下，再将sessionID保存到服务器端的特定的保存 session 的内存中。
   3. 使用 session：客户端再次访问服务器，会带上首次访问时获得的值为sessionID的cookie，服务端读取cookie中的sessionID，根据sessionID到保存session的内存寻找与 sessionID 匹配的数据，若寻找成功就将数据返回给客户端。

### LocalStorage
- localStorage的作用存储信息，并且它的生命周期是永久，除非用户主动清除它。并且它在浏览器中保存。
- 只有相同域名的页面才能互相读取localStorage，同源策略与cookie一致。
- 有限定的存储量，超出该存储量存储会被拒绝。

### SessionStorage
- 与LocalStorage基本一致，唯一的区别就是：SessionStorage会在会话关闭(页面关闭或者浏览器关闭)时被清空，而LocalStorage永久存在。

## Cookie,Session,SessionStorage与LocalStorage的区别

- 生命周期：
   - Cookie：可设置失效的时间，默认的失效时间是浏览器关闭后。
   - Session：服务器会定时将长时间没有活动的Session从内存中清除，也可以手动设置失效的时间。
   - LocalStorage：永久，除非用户主动清除它。
   - SessionStorage：在会话关闭(页面关闭或者浏览器关闭)时，会被清除。
- 存放数据的大小：
   - Cookie：一般为4K左右。
   - LocalStorage：一般为5M。
   - SessionStorage：一般为5M。
- 存放数据的位置：
   - 除了Session存放在服务器，其余的都存放在浏览器。



-  参考
   - [理解cookie、session、localStorage、sessionStorage的关系与区别](https://juejin.im/post/5daedc74518825374b6a17d4)
   - [详说 Cookie, LocalStorage 与 SessionStorage](https://segmentfault.com/a/1190000002723469)
   
