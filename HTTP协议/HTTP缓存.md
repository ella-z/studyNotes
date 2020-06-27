# HTTP缓存
- 缓存的内容：CSS、JS等更新频率不大的静态资源文件。
- 浏览器操作对HTTP缓存的影响:
<br />
<img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/%E6%B5%8F%E8%A7%88%E5%99%A8%E6%93%8D%E4%BD%9C%E5%AF%B9HTTP%E7%BC%93%E5%AD%98%E7%9A%84%E5%BD%B1%E5%93%8D.PNG" alt="" width="500px" height="250px">

### HTTP缓存头部字段
- Cache-Control 请求/响应头，缓存控制字段，控制http缓存的最高指令。
   + no-store:所有内容都不缓存。
   + no-cache:缓存，但是浏览器使用缓存前，都会请求服务器判断缓存资源是否是最新。
   + max-age=x(单位秒)请求缓存后的X秒不再发起请求，是http1.1的属性。
   + s-maxage=x(单位秒) 代理服务器请求源站缓存后的X秒不再发起请求，只对CDN缓存有效。
   + public 客户端和代理服务器(CDN)都可缓存。
   + private 只有客户端可以缓存。
- Expires 响应头，代表资源过期时间，由服务器返回提供，是http1.0的属性，在于max-age共存的情况下，优先级要低。若时间到了，资源过期了，
则可以再次请求缓存，若时间还没到，则继续使用本地的缓存。
- Last-Modified 响应头，资源最新修改时间，由服务器告诉浏览器。
- if-Modified-Since 请求头，资源最新修改时间，由浏览器告诉服务器，可以与Last-Modified进行对比，来发现资源是否被修改。
- Etag 响应头，资源标识，文件内容唯一对比标记，由服务器告诉浏览器，优先级比Last-Modified高，若文件修改过Etag会改变。
- if-None-Match 请求头，缓存资源标识（其实就是上次服务器给的Etag），由浏览器告诉服务器。

### md5/hash缓存
- 通过不缓存html，为静态文件添加MD5或者hash标识，解决浏览器无法跳过缓存过期时间主动感知文件变化问题。

### CDN缓存
- CDN是构建在网络之上的内容分发网络，依靠部署在各地的边缘服务器，通过中心平台的负载均衡、内容分发、调度等功能模块，使用户就近获取所需内容，降低
网络拥塞，提高用户访问响应速度和命中率。

### 强缓存与协商缓存
<br />
<img src="https://github.com/ella-z/studyNotes/blob/master/HTTP%E5%8D%8F%E8%AE%AE/images/%E5%BC%BA%E7%BC%93%E5%AD%98%E4%B8%8E%E5%8D%8F%E5%95%86%E7%BC%93%E5%AD%98.png" alt="强缓存与协商缓存" width="700px" height="550px">

- HTTP的缓存可以分为强缓存与协商缓存。
- 强缓存：强制缓存在缓存数据未失效的情况下（即Cache-Control的max-age没有过期或者Expires的缓存时间没有过期），那么就会直接使用浏览器的缓存数据，不会再向服务器发送任何请求。

- 协商缓存：当浏览器的强缓存失效的时候或者请求头中设置了不使用强缓存(即返回的响应头中没有Cache-Control和Expires或者Cache-Control和Expires过期或者它的属性设置为no-cache时)会使用协商缓存。
- 过程： 
   - 浏览器第一次向服务器请求数据的时候，会在响应头中返回协商缓存的头属性：ETag和Last-Modified。
   - 浏览器第二次向服务器请求数据的时候，请求头中设置了If-Modified-Since 或者 If-None-Match 的时候，会将这两个属性值到服务端去验证是否缓存是否失效，若未失效，返回304状态码，浏览器拿到此状态码直接使用缓存数据了，否则服务器会直接返回数据。

### 缓存的优点
1. 减少了冗余的数据传递，节省宽带流量。
2. 减少了服务器的负担，减少了访问数据库的次数，提升网站的性能。
3. **加快了客户端加载网页的速度。**


- 参考
   - [图解 HTTP 缓存](https://juejin.im/post/5eb7f811f265da7bbc7cc5bd)
   - [HTTP----HTTP缓存机制](https://juejin.im/post/5a1d4e546fb9a0450f21af23)
   - [一文读懂http缓存（超详细）](https://www.jianshu.com/p/227cee9c8d15)

