# webpack的配置文件
- webpack.config.js：webpack默认的配置文件
```
🌰1：
const path = require('path');

module.exports={
  entry:'./input.js',
  output:{
    path:path.resolve(_dirname,'dist'),
    filename:'output.bundle.js'
  }
}
//webpack的入口文件是input.js，出口文件是output.bundle.js

🌰2：
const path = require('path');

module.exports={
  entry:{
    home:'./home.js',
    about:'./about.js'
  },
  output:{
    path:path.resolve(_dirname,'dist'),
    filename:'[name].bundle.js'  //会根据入口文件的不同生成不同的出口文件：home.bundle.js、about.bundle.js
  }，
  mode:"development" //声明模式
}

/*
在开发环境中，运行项目指令：
  webpack:会将打包结果输出出去
  npx webpack-dev-server 只会在内存内编译打包，不会输出
*/
```

### webpack配置之entry
- entry，入口文件，有三种值类型：
   1. string，单入口
   ```
   🌰：
       entry：'./src/index.js'
      //结果：打包形成一个chunk(chunk默认得名称为main)，输出一个bundle文件。
   ```
   2. array，多入口，该种方法只有在特殊情况使用，例如在HMR功能中让html热更新生效的时候。
   ```
   🌰：
      entry：['./src/index.js','./src/add.js']
      //结果：所有入口文件最终只会形成一个chunk，输出出去只有一个bundle文件。
   ```
   3. object，多入口
   ```
   🌰：
   entry:{
   // 格式为key:path
    index:'./src/index.js',
    add:'./src/add.js'
   }
   //结果：有几个入口文件就形成几个chunk，输出几个bundle文件，此时chunk得名称为key。
   ```
   4. 特殊用法
   ```
   🌰：
   entry:{
    index:['./src/index.js','./src/count.js'],
    add:'./src/add.js'
   }
   // 结果：输出了两个chunk，一个为index的chunk，另一个为add的chunk，但在index的chunk中包含了两个js，一个是index.js，另一个是count.js。
   ```
   
### webpack配置之output
- filename：文件的名称(可以指定目录+名称)
- path：输出文件目录，也就是所有资源输出的公共目录
- publicPath：所有资源引入公共路径的前缀
   ```
   🌰：
   publicPath:'/'
   //在html中引入js文件时
   <script src="index.js"></script>  变为  <script src="/index.js"></script>
   ```
- chunkFilename：非入口chunk的名称
   ```
   🌰：
   chunkFilename:'js/[name]_chunk.js'
   
   //在index.js中
   import (./add.js).then(({add})=>{
        console.log(add(1,2));
   });
   
   //在输出的时候add.js会变为以0_chunk.j方式命名
   ```
- library：库向外暴露的变量名
- libraryTarget：控制 webpack 打包的内容是如何暴露的，默认时var
   ```
   🌰：
    library:'demo'
    libraryTarget: "window" - 将库的返回值分配给window对象，main.js中window["demo"] = _entry_return_;
    引用方式：window.demo.doSomething();
    
    libraryTarget: "global" - 将库的返回值分配给global对象，main.js中global["demo"] = _entry_return_;
    引用方式：global.demo();
    
    libraryTarget: "commonjs" - 将库的返回值分配给exports对象，main.js中exports["demo"] = _entry_return_;
    引用方式：require("demo").doSomething();
    
   ```
   
### webpack配置之module
```
🌰：
module:{
  rules:[
    //loader配置
    {
      //检查文件是否符合格式
      test:/\.css$/,
      //多个loader用use来配置
      use:['style-loader','css-loader']
    },
    {
      test:/\.js$/
      //排除node_modules下的js文件
      exclude:/node_module/
      //只检查src下的js文件
      //include:resolve(__dirname,'src'),
      //优先执行
      enforce:'pre',
      //延后执行
      //enforce:'post',
      //单个loader直接用loader配置
      loader:'eslint-loader'
    },
    {
      //以下的loader配置只会生效一个
      oneOf:[]
    }
  ]
}
```

### webpack配置之resolve
- 作用：解析模块的规则
- alias：配置解析模块路径别名。
- extension：配置省略文件路径的后缀名
- modules：用来设置模块搜索的目录，设定目录以后，import模块路径，就可以从一个子目录开始写，这样就可以缩短模块引入路径。
```
   🌰：
    //配置alias
    resolve:{
        alias:{
            $css:path.resolve(__dirname,'src/css')
        }
    }
    
   //配置alias之前，若在js中引入css
   import '../css/index.css';
   
   //配置alias之后，若在js中引入css，直接使用$css代替路径
   import '$css/index.css';
   
   🌰：
   //配置extensions
    resolve:{
        alias:{
            $css:path.resolve(__dirname,'src/css')
        },
        extensions:['.json','.css']
    }
    
    //配置之前
    import '../css/index.css';
    
    //配置之后
    import '../css/index';
    
    🌰：
    //配置modules
    resolve:{
      modules: ['./src/components']
    }
    //若要引入src/components的demo模块
    import 'demo' //省略了前面的路径，让webpack自己查找
    
```

### webpack配置之deServer
- deServer用于开发环境
- contentBase：运行代码的目录
- watchcontentBase：监视contentBase目录下的所有文件，一旦文件发生了变化就会reload
- watchOptions：用来定制 Watch 模式的选项
   ```
   🌰：
    watchOptions:{
        //忽略监视node_modules文件
        ignored:/node_modules/
    }
   ```
- compress：是否启动压缩
- port：端口号
- host：域名
- open：是否自动打开浏览器
- hot：是否开始热替换
- clientLogLevel：是否显示启动服务器日志信息
- quiet：是否显示基本启动信息以外的其他信息
- overlay：出错时，需不需要全屏提示
- proxy：服务器代理，主要解决开发环境跨域问题
```
🌰：
    devServer:{
        contentBase:resolve(__dirname,'build'),
        watchcontentBase:true,
        watchOptions:{
            ignored:/node_modules/
        },
        compress:true,
        port:5000,
        host:'localhost',
        open:true,
        hot:true,
        clientLogLevel:'none',
        quiet:true,
        overlay:false,
        proxy:{
            //一旦devServer(5000)服务器接收到 /api/xxx 的请求，就会把请求转发到另一个服务器(3000)
            '/api':{
                target:'http://localhost:3000',
                //发送请求时，请求路径重写，将/api/xxx  --> /xxx
                pathRewrite:{
                    '^/api':''
                }
            }
        }
    }
```

### webpack配置之optimization
- optimization用于生产环境
```
🌰：
optimization：{
  //代码分割
  splitChunks:{
     chunks:'all',
     //默认值
     /*
     minSize: 30*1024, //分割chunk最小为30kb，小于30kb不分割
     minChunks: 1, //要提取的chunk最少被引用一次
     maxAsyncRequests: 5, // 按需加载时并行加载的文件的最大数量
     maxInitialRequests: 3, //入口js文件最大并行请求数量
     automaticNameDelimiter: '~', //名称连接符
     name: true, //可以使用命名规则
     cacheGroups: {
     // 分割chunk组
         vendors: {
         // node_modules文件会被打包到vendors组的chunk中。
         // 需要满足上面的公共规则，例如：大小超过30kb，至少被引用一次
             test: /[\\/]node_modules[\\/]/,
             //优先级
             priority: -10
         },
         default: {
             minChunks: 2,
             priority: -20,
             //如果当前要打包的模块1，和之前已经被提取的模块是同一个，就会复用之前哪个模块，而不是重新打包模块。
             reuseExistingChunk: true
         }
     }
     */
  },
  //将当前模块的记录其他模块的hash单独打包为一个文件 runtime。
  // 解决了修改a文件会导致b文件的contenthash的变化。
  //例如：在index.js中引入了a.js(单独打包)，若a发生了改变，因为它的contenthash值存储在index中，那么会导致index也会发生改变。若将当前模块的记录其他模块的hash单独打包为一个文件 runtime中，那么a发生改变了，跟着改变的是runtime文件，而不是index文件。
  runtimeChunk:{
    name:entrypoint => `runtime-${entrypoint.name}`
  },
  minimizer:{
  //配置生产环境的压缩方案，主要是js和css，需要用到terser-webpack-plugin插件
  new terserWebpackPlugin({
    //开启缓存
    cache:true,
    //开启多进程打包
    parallel:true,
    //启动source-map
    sourceMap:true
  })
  }
}
```





   
   
   
   
   
   
   





