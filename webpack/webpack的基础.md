# webpack的基础
- webpack是一个前端资源加载/打包工具。它将根据模块的依赖关系进行静态分析，然后将这些模块按照指定的规则生成对应的静态资源。
- webpack4采用了0CJS(只适用于小项目)，0CJS的含义是0配置，webpack不再强制需要webpack.config.js作为打包的入口配置文件，而是默认入口为'./src'
以及默认出口'./dist'。

### webpack的属性
- mode属性，值可以为development或production(默认的值是production)。例如：webpack --mode development
   - development开发者模式中webpack提供的特性：
      1. 浏览器调试工具
      2. 注释、开发阶段的详细错误日志和提示
      3. 快速和优化的增量构建机制
   - production生产模式
      1. 开启所有的优化代码
      2. 更小的bundle大小
      3. 去除掉只在开发阶段运行的代码
      4. Scope hoisting(将多个bundle放入一个大闭包里，加快代码的运行速度)和Tree-shaking(去除引用模块中没有使用的代码段)

### webpack的使用
- 命令：webpack 输入文件.js -o 输出文件.js
```
🌰：
//index.js中
import {sayHello} form './index_module.js'
var test="123";

//index_module.js中
export default{
   sayHello:funciton(){
      console.log("hello");
   }
}

//webpack打包
webpack index.js -o output.js
webpack mode --development index.js -o output1.js

```

