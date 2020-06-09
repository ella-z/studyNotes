# tree shaking
- tree shaking：去除在应用程序中没有使用的代码，使得代码的体积变小。
   - js使用tree shaking的前提条件：
      1. 需要使用ES6模块化
      2. 开启production环境
 - 可以在package.json中配置"sideEffects"来决定文件是否进行tree shaking
 ```
 🌰：
 "sideEffects":false //所有文件都进行tree shaking
 "sideEffects":["*.css","*.LESS"] //css、less文件不进行tree shaking 
 ```
