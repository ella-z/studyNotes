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
   
   
   
   
   
   
   
   
   
   
   
   





