# Nodejs路由封装
### 模块化封装
  ```
  🌰：
  //模块的声明
   let app={
       login:(req,res)=>{
          //处理登录相关的业务逻辑
       },
       news:(req,res)=>{
          //处理news相关的业务逻辑
       }
   }
   
   //调用方法
   app.login('req','res');
   app['login']('req','res');
   
   //demo
   //app.js
    //首先引入http模块
    const http = require('http');

    //引入路由
    const routes= require('./module/routes');

    const url = require('url');

    http.createServer((req, res) => {

      routes.static(req,res,'static');

      //因为http://127.0.0.1:3031/login pathname='/login' ，而路由的调用方法为app['login']('req','res');，所以需要除去'/'
      let pathName = url.parse(req.url).pathname.replace('/',''); 
      try {
        routes[pathName](req,res);
      } catch (error) {
        routes['error'](req,res);
    }

    }).listen(3031);
    
    //routes.js
    //引入fs模块
    const fs = require('fs');
    //引入path模块
    const path = require('path');
    //引入url模块
    const url = require('url');

    getExtName = (extName) => {
        //该函数并不完整，因为只写了几个相关不同类型的文件的响应类型
        switch (extName) {
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


    let app = {
        static: (req, res, staticPath) => {
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
                fs.readFile('./' + staticPath + pathName, (err, data) => {
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
        },
        login:(req,res)=>{
            res.end('login');
        },
        news:(req,res)=>{
            res.end('news');
        },
        error:(req,res)=>{
            res.end('error');
        }
    }

    //暴露模块
    module.exports=app;
  ```
  
  ### 封装一个类似express框架的路由
  ```
  //express配置路由的方法
  //先引入express
  const express = require('express');
  
  //配置路由
  app.get('/',function(req,res)=>{
    //相关操作
  });
  app.get('/login',function(req,res)=>{
    //相关操作
  });
  app.post('/doLogin',function(req,res)=>{
    //相关操作
  });
  
  ```
  ```
  🌰：
  //app.js
  const http = require('http');
  const fs = require('fs');
  const route= require('./module/routes')

  http.createServer(route).listen(3031);

  //get请求
  route.get('/',(req,res)=>{
      res.writeHead(200, { "Content-type": "text/html;charset='utf-8" });
      res.end('first page');
  });

  route.get('/login',(req,res)=>{
      fs.readFile('./static/login.html', (err, data) => {
          if (err) {
              res.writeHead(404, { "Content-type": "text/html;charset='utf-8" });
              res.end('找不到该网页');
          }
          res.writeHead(200, { "Content-type": "text/html;charset='utf-8" });
          res.end(data);
      });
  });

  route.get('/news',(req,res)=>{
      res.writeHead(200, { "Content-type": "text/html;charset='utf-8" });
      res.end('news!');
    });

  //post请求
    route.post('/doLogin',(req,res)=>{
      res.writeHead(200, { "Content-type": "text/html;charset='utf-8" });
      res.end(req.body);
  });
  
  //login.html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Document</title>
      <link rel="stylesheet" href="./css/css.css">
  </head>
  <body>
     <span>login</span>
      <form action="/doLogin" method="post">
        name: <input type="text" name="name" >
        <input type="submit" value="提交">
      </form>
  </body>
  </html>
  
  //routes.js
  /**
   * 最终是以这样的方式配置路由
   *  app.get('/login',function(req,res)=>{
      //相关操作
    });
   */
  const url = require('url');
  const path = require('path');
  const fs = require('fs');

  let server = () => {
    let G = {
      _get: {},
      _post: {},
      staticPath: 'static'
    };

    function getExtName(extName) {
      //该函数并不完整，因为只写了几个相关不同类型的文件的响应类型
      switch (extName) {
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

    function initStatic(req, res, staticPath) {
      //1.获取文件，可以通过req.url获取到地址 index.html

      //let pathName = req.url;//这样写存在json文件无法访问的问题
      let pathName = url.parse(req.url).pathname;//除去访问地址中的get传值,即可访问到json文件中的数据了

      //path.extname()可以获取文件后缀名
      let extname = path.extname(pathName);
      console.log('./' + staticPath + pathName);
      //2.通过fs模块读取文件
      try {
        let data = fs.readFileSync('./' + staticPath + pathName);
        if (data) {
          //动态改变响应类型，当不同类型的文件访问的时候，根据文件的类型改变响应类型
          let text = getExtName(extname);
          res.writeHead(200, { "Content-type": "" + text + ";charset='utf-8" });
          res.end(data);
        }
      } catch (error) {

      }

    }

    let route = (req, res) => {
      let pathName = url.parse(req.url).pathname;

      //配置静态web服务
      initStatic(req, res, G.staticPath);

      //获取请求的方法
      let method = req.method.toLowerCase();

      if (G['_' + method][pathName]) {
        if (method === 'get') {
          G['_' + method][pathName](req, res);//执行方法
        } else {
          //post 获取post的数据，把它绑定到req.body上
          let postData = '';

          req.on('data', (chunk) => {
            postData += chunk.toString();
          });

          req.on('end', () => {
            req.body = postData;
            G['_' + method][pathName](req, res);
          })
        }

      } else {
        res.writeHead(200, { "Content-type": "text/html;charset='utf-8" });
        res.end('error');
      }
    }

    //配置get请求
    route.get = (str, cb) => {
      //注册方法
      G._get[str] = cb;
    }

    //配置post请求
    route.post = (str, cb) => {
      G._post[str] = cb;
    }

    //配置静态web服务目录
    route.staticPath = (staticPath) => {
      G.staticPath = staticPath;
    }

    return route;
  }


  module.exports = server();
  ```
  
