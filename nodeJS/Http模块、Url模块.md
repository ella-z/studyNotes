# Http模块、url模块
### Http模块
```
🌰：
//创建web服务器
//引入http模块
var http = require('http');
//创建web服务
/*
request获取客户端传递过来的信息。
response给浏览器响应信息。
*/
http.createServer(function (request, response) {
  //设置响应头
  response.writeHead(200, {'Content-Type': 'text/plain'});
  //在页面上输入一句话并且结束响应
  response.end('Hello World');
}).listen(8081);
//8081代表网络端口
```
### url模块
- url.parse() 解析url
- url.format(urlObject) 是url.parse()操作的逆向操作
- url.resolve(from,to) 添加或替换地址
```
🌰：
const url  = require('url');

var api = 'http://www.itying.com?name=jack&age=20';
//true 将query的字符串，转化为对象
console.log(url.parse(api,true));
```
