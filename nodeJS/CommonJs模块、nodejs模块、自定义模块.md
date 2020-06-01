# CommonJs
- CommonJs就是模块化的标准，nodejs就是CommonJs(模块化)的实现。
- CommonJs(NodeJs)中自定义模块的规定
   1. 我们可以把公共的功能抽离成为一个单独的js文件作为一个模块，默认情况下面这个模块
里面的方法或者属性，外面是没法访问的。如果要让外部可以访问模块里面的方法或者属性，
就必修再模块里面通过 exports 或者 module.exports 暴露属性或者方法。
   2. 在需要使用这些模块的文件中，通过require的方式引入这个模块。

# nodeJs
- node应用由模块组成，采用CommonJS模块规范。
- 在node中模块分为两类：
   1. 核心模块，由node提供，例如http模块以及url模块、fs模块都是nodejs内置的核心模块，可以直接引用。
   2. 文件模块，由用户自己编写以及定义。
   
### nodejs中自定义模块的规定
1. 将公共的功能抽离成为一个单独的js文件作为一个模块。默认情况下，外部无法访问该模块里面的方法以及属性。如果要让外部可以访问到模块里面的方法或者属性，就必须在模块里面通过exports或者module.exports来暴露属性或者方法。
2. 在需要使用这些模块的文件中，通过require的方式引入这个模块。这个时候就可以使用模块里面暴露的方法以及属性。

```
🌰：
//自定义模块demo
module/tools.js
//方法
function formatAPI(apiUrl){
    return "https://www.baidu.com/"+apiUrl;
}
exports.formatAPI = formatAPI;

//对象
var obj={
    get:()=>{
        console.log("从服务器获取数据");
    },
    post:()=>{
        console.log("提交数据给服务器");
    }
}
exports.obj= obj;//或者module.exports=obj

node_modules/axios/index.js
exports.get=()=>{
        console.log("从服务器获取数据");
    },
exports.post=()=>{
        console.log("提交数据给服务器");
}

node_modules/db/db.js
exports.add=()=>{
    console.log("添加数据");
}


//使用自定义模块demo
const http = require('http');

//引入自定义模块
const tools = require('./module/tools');
//打印一下tools里面的内容
console.log(tools);//{ formatAPI: [Function: formatAPI] }

const axios = require('./node_modules/axios/index');
axios.get();//从服务器获取数据

//在node_modules可省略
const axios = require('axios/index');
axios.post();//提交数据给服务器

//在nodejs中会自动调用node_modules里面对应的axios文件下面的index.js,与引入系统模块相似
const axios = require('axios');
axios.post();//提交数据给服务器

/*注意！！！默认找的是index.js文件，若无该文件程序会报错。
解决方法：使用npm init --yes生成 package.json配置文件
yes代表强制生成
在package中会将main改为文件夹中的js
*/
var db = require('db');
db.add();

http.createServer((req,res)=>{
    res.writeHead(200,{"Content-type":"text/html;charset='utf-8"});
    res.write('hello nodejs');
    //使用模块里面的方法
    let api = tools.formatAPI('wd=nodejs');
    res.write(api);
    res.end()
  }).listen(8081);
```
