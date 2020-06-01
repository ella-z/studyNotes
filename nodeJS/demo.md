### 利用Http模块、Url模块、Path模块、Fs模块创建一个静态的WEB服务器
   - 可以让我们访问web服务器上面的网站。
   - 可以让我们下载web服务器上面的文件。
   ```
   //利用Http模块、Url模块、Path模块、Fs模块创建一个静态的WEB服务器，类似apache、nginx
/**
 * 1.可以让我们访问web服务器上面的网站
 * 2.可以让我们下载web服务器上面的文件，当识别到地址的extName为zip或其他网页会自动下载该文件。
 */

//首先引入http模块
const http = require('http');
//引入fs模块
const fs = require('fs');
//引入path模块
const path = require('path');
//引入url模块
const url = require('url');

//引入自定义模块，根据后缀返回不同的文件的响应类型
const common = require('./module/common.js')

http.createServer((req, res) => {
    //1.获取文件，可以通过req.url获取到地址 index.html

    //let pathName = req.url;//这样写存在json文件无法访问的问题
    let pathName = url.parse(req.url).pathname;//除去访问地址中的get传值,即可访问到json文件中的数据了

    //设置默认访问index.html
    pathName = pathName == '/' ? '/index.html' : pathName;

    //path.extname()可以获取文件后缀名
    let extname = path.extname(pathName);
    console.log(extname);

    //2.通过fs模块读取文件
    if (pathName !== '/favicon.ico') {
        fs.readFile('./static/' + pathName, (err, data) => {
            if (err) {
                res.writeHead(404, { "Content-type": "text/html;charset='utf-8" });
                res.end('找不到该网页');
            }
            //动态改变响应类型，当不同类型的文件访问的时候，根据文件的类型改变响应类型
            let text = common.getExtName(extname);
            res.writeHead(200, { "Content-type": "" + text + ";charset='utf-8" });
            res.end(data);
        });
    }
}).listen(3031);
   ```
   
   ### NodeJs封装静态web服务
   ```
   //app.js
   /**
   封装静态web服务器
    */

   //首先引入http模块
   const http = require('http');

   //引入路由
   const routes= require('./module/routes');

   http.createServer((req, res) => {

   routes.static(req,res,'static');

   }).listen(3031);
   ```
   
   ```
   //routes.js
   //引入fs模块
   const fs = require('fs');
   //引入path模块
   const path = require('path');
   //引入url模块
   const url = require('url');

   getExtName=(extName)=>{
       //该函数并不完整，因为只写了几个相关不同类型的文件的响应类型
       switch(extName){
           case '.html':
               return 'text/html';
           case '.css':
               return 'text/css';
           case '.js':
               return 'text/javaScript';
           default:
               return 'text/html';
       }
   }

   exports.static = ((req,res,staticPath)=>{
       //1.获取文件，可以通过req.url获取到地址 index.html

       //let pathName = req.url;//这样写存在json文件无法访问的问题
       let pathName = url.parse(req.url).pathname;//除去访问地址中的get传值,即可访问到json文件中的数据了

       //设置默认访问index.html
       pathName = pathName == '/' ? '/index.html' : pathName;

       //path.extname()可以获取文件后缀名
       let extname = path.extname(pathName);
       console.log(extname);

       //2.通过fs模块读取文件
       if (pathName !== '/favicon.ico') {
           fs.readFile('./' +staticPath+ pathName, (err, data) => {
               if (err) {
                   res.writeHead(404, { "Content-type": "text/html;charset='utf-8" });
                   res.end('找不到该网页');
               }
               //动态改变响应类型，当不同类型的文件访问的时候，根据文件的类型改变响应类型
               let text = getExtName(extname);
               res.writeHead(200, { "Content-type": "" + text + ";charset='utf-8" });
               res.end(data);
           });
       }
   })
   ```

