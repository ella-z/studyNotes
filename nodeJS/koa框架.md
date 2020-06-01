# koa框架
- koa框架可以避免异步嵌套
- 安装koa：npm install --save koa //--save参数，表示自动修改package.json文件，自动添加依赖项

```
🌰：
var koa = require('koa');
var app = new koa();

app.use(async(ctx)=>{
    ctx.body='hello koa';
});

app.listen(3031);
```

### koa-router 路由
- 安装koa-router：npm install --save koa-router 

#### GET 
```
🌰：
const Koa = require('koa');
//引入并且实例化路由
let router = require('koa-router')();//相当于const Router = require('koa-router'); let router = new Router();

let app = new Koa();

//配置路由，ctx是上下文，包含了requset以及response等信息
//way1:
router.get('/',async (ctx)=>{
    ctx.body='首页'; //返回数据，类似于res.end()
});

router.get('/news',async (ctx)=>{    
    /**从ctx中读取get传值
     * query:返回的是格式化好的参数对象
     * queryString:返回的是请求字符串
    */
   //若url=/news?type=first&name=new
   console.log(ctx.query);//{type:first,ame=new}
   console.log(ctx.querystring);//type=first&ame=new

    //ctx.request获取请求的详细信息
    console.log(ctx.request);

    ctx.body='news';
});

/*
way2:
router.get('/',async (ctx)=>{
    ctx.body='首页'; //返回数据，类似于res.end()
}).get('/news',async (ctx)=>{
    ctx.body='news';
});
*/

app.use(router.routes());//启动路由
app.use(router.allowedMethods);//该段代码在所有路由中间件最后调用，会根据ctx.status设置response响应头

app.listen(3031);
```

#### POST
```
🌰：
const Koa = require('koa');
const Router = require('koa-router');
const common = require('./modules/common');
const views = require('koa-views');

let app = new Koa();
let router = new Router();

app.use(views('views',{  
    extension:'ejs'
}))

router.get('/',async (ctx)=>{
    let title = "hello"
    await ctx.render('index',{
        title:title //绑定数据, 向前台传递数据
    });
});

router.get('/news',async (ctx)=>{
    ctx.body='news page';
});

router.post('/dologin',async (ctx)=>{
    //接收post提交的数据。因为是异步，所以需要封装接收数据的方法
    let data = await common.getPostData(ctx);
    ctx.body = data;
})

app.use(router.routes());
app.use(router.allowedMethods);

app.listen(3031);
---------------------------------------------------------------------------------
common.js
exports.getPostData = function(ctx){
    return new Promise((resolve,reject)=>{
        let str = '';
        ctx.req.on('data',(data)=>{
            str+=data;
        });
        ctx.req.on('end',()=>{
            resolve(str);
        })
    })
}
```

#### 动态路由
```
🌰：
//动态路由，可以传入多个值
//例如：localhost:3031/newsContent/123 ctx.params的传值为{aid:'123'}
//若需要传递两个值，可写为/newsContent/:aid/:cid
router.get('/newsContent/:aid',async (ctx)=>{
    console.log(ctx.params);//获取动态路由的传值
    ctx.body = '新闻详情';
});
```

### koa 中间件
- 中间件：匹配路由之前或者匹配路由完成之后做的一些操作。
- 在多个中间件时，执行的顺序类似于栈。
- 中间件可分为：
   1. 应用级中间件
   2. 路由级中间件
   3. 错误处理中间件
   4. 第三方中间件
```
🌰：
//无论app.use的位置在哪里，在koa中都是先执行app.use再匹配路由
/**应用及中间件
 * app.use('/',(ctx,next)=>{}); 该种方式表示只匹配其中的一种路由，并执行其中的方法
 * app.use((ctx,next)=>{}); 该种方式表示匹配所有的路由，并且执行其中的方法
 * next表示匹配下一个路由，若不写next参数，则会匹配到这个路由之后不再向下匹配路由了
*/
app.use(async (ctx,next)=>{
    console.log('中间件');
    await next();
});

/**路由中间件
 * 匹配到符合条件的路由之后再继续向下匹配路由
 */
router.get('/news',async (ctx,next)=>{
    console.log('news page');
    await next();//继续向下匹配
});
router.get('/news',(ctx)=>{
    ctx.body='news page';
})

//错误处理中间件
app.use(async (ctx,next)=>{
    console.log('中间件');//先输出 中间件
    next();// 往下匹配路由

    //匹配完成路由之后再进行判断
    if(ctx.status == 404){ //若该路由查询不到
        ctx.status=404;
        ctx.body = '错误页面'
    }else{
        console.log(ctx.url);
    }
})
```

#### 第三方中间件 koa-views视图管理模块
- 安装: npm install koa-views --save
```
🌰：
const Koa = require('koa');
const Router = require('koa-router');
//引入koa-views
const views = require('koa-views');

let app = new Koa();
let router = new Router();

//配置模板引擎中间件
//app.use(views('views',{map:{html:'ejs'}})); //这种方式的调用的模板的后缀名一定要是html
app.use(views('views',{   //这种方式调用模板的后缀名为ejs

    extension:'ejs'
}))
//配置公共数据的中间件
app.use(async (ctx,next)=>{
/*若我们需要在每个路由的renader中都要渲染一个公共的数据，可以将该数据放在ctx.state中，这样的话，就可以在模板的任何地方调用这个数据*/
ctx.state ={
    name:'jack'
}
await next();
});

router.get('/',async (ctx)=>{
    let title = "hello"
    await ctx.render('index',{
        title:title //绑定数据, 向前台传递数据
    });
});

router.get('/news',(ctx)=>{
    ctx.body='news page';
});


app.use(router.routes());
app.use(router.allowedMethods);

app.listen(3031);
```

#### 第三方中间件 koa-bodyparser
- 安装: npm install koa-bodyparser --save
```
🌰：
//可以简略post请求
const Koa = require('koa');
const Router = require('koa-router');
const views = require('koa-views');
//引入koa-bodyparser
const bodyParser = require('koa-bodyparser');


let app = new Koa();
let router = new Router();

app.use(views('views',{  
    extension:'ejs'
}))

//配置bodyparser
app.use(bodyParser());

router.get('/',async (ctx)=>{
    let title = "hello"
    await ctx.render('index',{
        title:title 
    });
});

router.post('/dologin',async (ctx)=>{
   //直接引用
    ctx.body = ctx.request.body; //返回的值为object
})

app.use(router.routes());
app.use(router.allowedMethods);

app.listen(3031);

```

#### koa-static 静态资源中间件
- 安装：npm install koa-static --save
```
🌰：
const Koa = require('koa');
const Router = require('koa-router');
const views = require('koa-views');
const bodyParser = require('koa-bodyparser');
//引入koa-static
const static = require('koa-static');

let app = new Koa();
let router = new Router();

app.use(views('views',{  
    extension:'ejs'
}))

//配置koa-static中间件，指定目录
//若请求的地址为localhost:3031/basic.css，首先会在static目录下寻找是否有该文件，若有则返回该文件。若没有则执行next()
app.use(static('./static'));//way1
app.use(static(__dirname+'/static'));//way2,__dirname是当前文件的地址

app.use(bodyParser());


router.get('/',async (ctx)=>{
    let title = "hello"
    await ctx.render('index',{
        title:title 
    });
});

router.post('/dologin',async (ctx)=>{
    ctx.body = ctx.request.body;
})

app.use(router.routes());
app.use(router.allowedMethods);

app.listen(3031);
--------------------------------------------------------------------------------
//EJS中
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <!--
        1.若直接使用，无法引入css。因为在路由中并没有处理请求basic.css的地址的路由，可以使用koa-static中间件解决这个问题
        2.因为在配置koa-static中间件的时候已经指定了目录，所以这个href的地址是相对于指定目录的地址。
    -->
    <link rel="stylesheet" href="basic.css"> 
</head>
<body>
   <h2><%=title%></h2> 
   <form action="/dologin" method="post">
        account:<input type="text" name="account" id="account">
        password:<input type="password" name="password" id="password">
        <input type="submit" value="submit">
   </form>
</body>
</html>
```

### art-template 模板引擎
- 安装：1. npm install art-template --save
        2. npm install koa-art-template --save
```
const render = require('koa-art-template');

render(app,{
    root:path.join(__dirname,'view'), //视图的位置
    extname:'.art', //后缀名
    debug: process.env.NODE_ENV !== 'production' //是否开启调试模式
});

🌰：
js:
const Koa = require('koa');
const Router = require('koa-router');
const path=require("path");
//引入koa-art-template
const render = require('koa-art-template');

let app = new Koa();
let router = new Router();

render(app,{
    root:path.join(__dirname,'views'), //视图的位置
    extname:'.html', //后缀名
    debug: process.env.NODE_ENV !== 'production' //是否开启调试模式
});

router.get('/',async (ctx)=>{
    let list={
        name:'Ella',
        age:20,
        friends:['Roseanne','Jack']
    }
    await ctx.render('index',{
        list //传递数据
    });
});

router.get('/news',async (ctx)=>{    
    ctx.body='news';
});

app.use(router.routes());
app.use(router.allowedMethods);

app.listen(3031);
------------------------------------------------------------------------------
html:
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    first page
    
    <div>
        name:{{list.name}}
    </div>
    <!--判断数据-->
    <div>
        age:{{if list.age>20}} 大于20 {{else}} 小于等于20 {{/if}}
    </div>
    <!--循环数据-->
    <div>
        friends: {{each list.friends}} {{$index}} -- {{$value}} {{/each}}
    </div>
    <!--引入子模版-->
    <div>
        {{include './public/test.html'}}
    </div>
</body>
</html>
```
        
### koa cookie
- 用法: 
   - 设置cookie:ctx.cookies.set(name,value,[[options](https://github.com/koajs/koa/blob/master/docs/api/context.md)]);
   - 获取cookie的值: ctx.cookies.get('name');
```
🌰：
const Koa = require('koa');
const Router = require('koa-router');

let app = new Koa();
let router = new Router();

router.get('/',async (ctx)=>{
    let list={
        name:'Ella'
    }
    //在koa中没法直接设置中文的cookie，解决方法：使用buffer将中文转换成base64的字符串，在获取的时候再还原base64字符串
    /**例子：
     * new Buffer('hello').toString('base64');//转换为base64,aGVsbG8=
     * new Buffer('aGVsbG8=','base64').toString();//还原base64字符串:hello
     */
    ctx.cookies.set('name',list.name,{
        maxAge:60*1000*24, //设定时间
        httpOnly:true //httpOnly,若为true，那么只有在服务器能查询到这个cookie。若为false，则无论是在客户端（例如：js）还是在服务器皆可以访问。
    })
    ctx.body = 'firstPage'
});

router.get('/news',async (ctx)=>{    
    let cookiesName = ctx.cookies.get('name');
    ctx.body=cookiesName;
});

app.use(router.routes());
app.use(router.allowedMethods);

app.listen(3031);
```

### koa session
- session与cookie不同在于；session保存在服务器上，而cookie保存在客户端浏览器中。
- 安装：npm install koa-session --save
- [koa session的使用方法](https://www.npmjs.com/package/koa-session)
```
🌰：
const Koa = require('koa');
const Router = require('koa-router');
//引入express-session
const session = require('koa-session');

let app = new Koa();
let router = new Router();

//配置koa-session中间件
app.keys = ['some secret hurr'];
 
const CONFIG = {
  key: 'koa.sess', /** (string) cookie key (default is koa.sess) */
  /** (number || 'session') maxAge in ms (default is 1 days) */
  /** 'session' will result in a cookie that expires when session/browser is closed */
  /** Warning: If a session cookie is stolen, this cookie will never expire */
  maxAge: 86400000, //！！设置过期的时间
  autoCommit: true, /** (boolean) automatically commit headers (default true) */
  overwrite: true, /** (boolean) can overwrite or not (default true) */
  httpOnly: true, /** (boolean) httpOnly or not (default true) */
  signed: true, /** (boolean) signed or not (default true) */
  rolling: false, /** ！！(boolean) Force a session identifier cookie to be set on every response. The expiration is reset to the original maxAge, resetting the expiration countdown. (default is false) */
  renew: false, /** ！！(boolean) renew session when session is nearly expired, so we can always keep user logged in. (default is false)*/
  secure: false, /** (boolean) secure cookie*/
  sameSite: null, /** (string) session cookie sameSite options (default null, don't set it) */
};
app.use(session(CONFIG, app));


router.get('/',async (ctx)=>{
    let list={
        name:'Ella'
    }
    ctx.session.name=list.name;
    ctx.body = 'firstPage'
});

router.get('/news',async (ctx)=>{    
    ctx.body=ctx.session.name;
});

app.use(router.routes());
app.use(router.allowedMethods);

app.listen(3031);
```

### koa 封装mongodb数据库
- 
```
🌰：
db.js
//db库
var mongoClient = require('mongodb').MongoClient;
var config = require('./config');

//封装数据库
class Db {
    static getInstance(){ //使用单例，缩短查询时间，避免多次链接同一数据库，解决多次实例化实例不共享的问题
        if(!Db.intance){
             Db.intance = new Db();
        }
        return  Db.intance;
       
    }
    constructor() {
        this.dbClient = '';//存放db对象
        this.connect();
    }
    //链接数据库
    connect() {
        return new Promise((resolve, reject) => {
            if (!this.dbClient) { //判断是否链接过数据库
                mongoClient.connect(config.dbUrl, (err, client) => {
                    if (err) {
                        reject(err);
                    } else {
                        this.dbClient = client.db(config.dbName);
                        resolve(this.dbClient);
                    }
                })
            }else{
                resolve(this.dbClient);
            }
        })
    }
    //find
    find(collectionName, json) {
        return new Promise((resolve, reject) => {
            this.connect().then((db) => {
                let result = db.collection(collectionName).find(json);
                result.toArray((err, data) => {
                    if (err) {
                        reject(err);
                        return;
                    } else {
                        resolve(data);
                    }
                })
            })
        })

    }
    //update
    update() {

    }
    //insert
    insert() {

    }
}

var test = new Db();
test.find('students', {}).then((data) => {
    console.log(data);
});
var test1 = Db.getInstance();//使用静态方法获取实例对象

----------------------------------------------
config.js
let app={
    dbUrl:'mongodb://admin:123456@127.0.0.1:27017',
    dbName:'student'
}

module.exports = app;
----------------------------------------------
app.js
const Koa = require('koa');
const Router = require('koa-router');
const path = require("path");
const render = require('koa-art-template');
//引入封装类
const Db = require('./modules/db');

let app = new Koa();
let router = new Router();

render(app, {
    root: path.join(__dirname, 'views'), //视图的位置
    extname: '.html', //后缀名
    debug: process.env.NODE_ENV !== 'production' //是否开启调试模式
});

router.get('/', async (ctx) => {
    let list=[];
    //通过封装类获取数据
    await Db.find('students', {}).then((data) => {
        list=data;
    });
    console.log(list);
    await ctx.render('index', {
        list
    });
});
```

### koa手脚架
- 安装：npm install koa-generator -g
- 创建项目：koa koa_demo
- 安装依赖：npm install
- 启动项目：npm start

### koa路由模块化
- 将路由根据功能的不同分为数个子路由，再在app.js中引入各个路由的模块，并且配置子路由。










