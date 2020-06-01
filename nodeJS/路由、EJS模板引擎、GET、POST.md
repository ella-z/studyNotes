### 路由
   - 路由是由一个URL（或者叫路径）和一个特定的HTTP方法（GET、POST等）组成的，涉及到应用如何响应客户端对某个网络节点的访问。也就是说针对不同请求的URL，处理不同的业务逻辑。

### EJS模块引擎
   - EJS模块引擎是一个第三方模块，需要通过npm安装，该模块可以把我们数据库和文件读取的数据显示到Html页面上面。
   - 安装方法： npm install ejs -save
   - 使用的方法
   ```
   let template = ejs.compile(str,options);
   template(data);
   
   ejs.render(str,data,options);
   
   ejs.renderFile(filename,data,options,(err,str)=>{
    // str => Rendered HTML string
   })
   ```
### GET、POST
   ```
   const http = require('http');
   //如何获取请求类型
   http.createServer((req, res) => {
     //输出请求的类型
     console.log(req.method);

   }).listen(3031);
   
   //如何获取get的传值
   const url = require('url');
   //url中的query中的值为get中传递过来的值
   var query = url.parse(req.url,true).query;
   
   //如何获取post的传值
   var postData = '';
   //数据块接收中,数据是以流的方式传递。
   req.on('data',(postDataChunk)=>{
    postData+=postDataChunk;
   });
   
   req.on('end',()=>{
    try {
      postData = JSON.parse(postData);
    } catch (error) {
       req.query = postData;
    }
   });
   
   ```
