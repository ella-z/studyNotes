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
