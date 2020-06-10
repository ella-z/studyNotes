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

#### 强缓存与协商缓存
