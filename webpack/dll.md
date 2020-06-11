# dll
- dll：动态链接库，对某些库进行单独打包，并且将多个库打包成为一个chunk。
```
🌰：
// 利用dll技术对jquery进行单独打包
/*
  当运行的命令是webpack时，默认查找的是webpack.config.js配置文件。
  当需要运行webpack.dll.js文件，需要将运行的命令修改为webpack --config webpack.dll.js，使用dll对jquery进行打包后，就不需要在构建的时候再对
  jquery进行打包了，需要用的时候，直接引入即可。
*/

//在webpack.dll.js中
const path = require('path');
const webpack = reuqire('webpack');

module.exports={
  entry:{
    // 最终打包生成的文件名称是jquery
    // ['jquery']表示要打包的库是jquery
    jquery:['jquery']
  },
  output:{
    filename:"[name].js",//[name]=jquery
    path:path.resolve(__dirname,'dll'),
    library:'[name]_[hash]' //指定了打包的库里面向外暴露的名字
  },
  plugins:[
    //打包生成一个manifest.json，该文件提供了jquery的映射
    new webpack.Dllplugin({
      name:'[name]_[hash]',//映射库的暴露的内容名称
      path:resolve(__dirname,'dll/manifest.json')//输出文件路径
    })
  ]
}

//在webpack.config.js中
const webpack = require('webpack');
const addAssetHtmlWebpackPlugin = require('add-asset-html-webpack-plugin');

plugins:[
//利用告诉webpack哪些库不需要打包
  new webpack.DllReferencePlugin({
    manifest:resolve(__dirname,'dll/manifest.json')
  }),
  //将某个文件打包输出去，并在html中自动引入该资源
  new addAssetHtmlWebpackPlugin({
    filepath:resolve(__dirname,'dll/jquery.js')
  })
]






```
