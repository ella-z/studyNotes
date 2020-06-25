# token
### 什么是token
- token是服务端生成的一串字符串，以作为客户端进行请求的一个令牌。
- Token 的身份验证的过程：
   1. 当用户完成第一次登录后，服务器就会生成一个token，并将改token返回到给客户端。
   2. 客户端收到token之后可以将其存储到Cookie里或者LocalStorage。
   3. 每次客户端请求服务端的时候只需带上这个token到服务端请求数据即可，不需要再带上用户名以及密码。
   4. 服务端收到请求后，去验证客户端中请求中带着的token，如果验证成功，就向客户端返回请求的数据。

### 为什么要使用token
- 因为token能减轻服务器的压力，减少查询数据库的次数，
- 请求中发送token而不再是发送cookie能够防止CSRF(跨站请求伪造)。
- 在客户端存储的Tokens是无状态的，并且能够被扩展。基于这种无状态和不存储Session信息，负载负载均衡器能够将用户信息从一个服务传到其他服务器上。
- token具有可扩展性，能够创建与其它程序共享权限的程序。
- token可以多平台跨域， 只要用户有一个通过了验证的token，数据和资源就能够在任何域上被请求到。

### 如何生成token
- 使用**JWT**，即JSON Web Tokens。
### JWT
- JWT标准的Token有三部分：
   - header
   - payload
   - signature
   - 各个部分之间会用点分开，并且都会使用Base64编码。
#### Header
- Header主要有两部分的内容：
   1. Token的类型。
   2. 使用的算法
   ```
   🌰：
   {
      "typ" : "JWT",
      "alg" : "HS256"
   }
   ```
#### Payload
- Payload中存储着Token的具体内容，这些内容中有一些标准的字段：
   - iss：Issuer，发行者
   - sub：Subject，主题
   - aud：Audience，观众
   - exp：Expiration time，过期时间
   - nbf：Not before
   - iat：Issued at，发行时间
   - jti：JWT ID
   
#### Signature
- Signature由三个部分组成，先使用Base64 编码的 header 和 payload 组合成一个字符串，再将这个字符串用加密算法加密一下，在加密的时候要加入一个Secret(相当于密码)，这个Secret会存储在服务端。
```
var encodedString = base64UrlEncode(header) + "." + base64UrlEncode(payload); 
HMACSHA256(encodedString, 'secret');
```
- 最后在服务端收到的token就是由这三部分组成。

### token的应用
- token一般引用在两个方面
   1. 防止表单重复提交
   2. 防止CSRF攻击，即跨站点请求伪造
- 这两个原理上都是通过 session + token来实现的。
- 当客户端请求页面时，服务器会生成一个随机数Token，并且将 Token 放置到 session 当中，然后将 Token 发给客户端。下次客户端提交请求时，Token 会随着表单一起提交到服务器端。
- 对于防止CSRF攻击，服务器端会对 Token 值进行验证，判断是否和session中的Token值相等。若相等，则可以证实请求是有效的。
- 对于防止表单重复提交，服务器端第一次验证相同过后，会将 session 中的Token值更新，若用户重复提交，第二次的验证判断将失败，因为用户提交的表单中的 Token 没变，但服务器端 session 中 Token 已经改变了。
- 使用session + token方法的缺点：当多页面多请求时，必须采用多Token同时生成的方法，这样占用更多资源，执行效率会降低。可以使用 cookie +token 代替session + token，但是若token存储在cookie中容易遭受XSS攻击。


- 参考：
   > https://www.cnblogs.com/lufeiludaima/p/pz20190203.html

   > https://blog.csdn.net/qq_35246620/article/details/55049812
