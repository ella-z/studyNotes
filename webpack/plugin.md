# plugin
### MiniCssExtractPlugin
- 作用：将sass/scss转化称为css之后，将css输出到文件中去。
- 安装：npm install --save-dev mini-css-extract-plugin
```
🌰：
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
module.exports = {
  plugins: [
    new MiniCssExtractPlugin({
      // Options similar to the same options in webpackOptions.output
      // both options are optional
      filename: "[name].css",
      chunkFilename: "[id].css"
    })
  ],
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
        //This plugin should be used only on production builds without style-loader in the loaders chain,
        //especially if you want to have HMR in development.
          devMode ? 'style-loader' : MiniCssExtractPlugin.loader,
          'css-loader',
          'postcss-loader',
          'sass-loader',
        ],
      }
    ]
  }
}
```

### DefinePlugin
- 作用：允许创建一个在编译时可以配置的全局常量。如果在开发构建中，而不在发布构建中执行日志记录，则可以使用全局常量来决定是否记录日志。
- [用法](https://webpack.docschina.org/plugins/define-plugin/)

### HtmlWebpackPlugin
- 作用：帮助我们生成html文件。例如：将js文件包含进某个html文件使得它可以在浏览器上运行。
- 安装：npm install --save-dev html-webpack-plugin
```
🌰：
var HtmlWebpackPlugin = require('html-webpack-plugin');
var path = require('path');

module.exports = {
  entry: 'index.js',
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: 'index_bundle.js'
  },
  //plugins: [new HtmlWebpackPlugin()] //生成html并且包含引入了index.js
  plugins: [new HtmlWebpackPlugin({  //将该index.js插入某个html文件中
     title:'APP',   //可以在html文件中以<%= htmlWebpackPlugin.options.title %>的形式引入该title值
     filename:'assets/admin.html', //输出文件
     template:'template.html' // 输入文件
  })]
};

//产生html文件
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>webpack App</title>
  </head>
  <body>
    <script src="index_bundle.js"></script>
  </body>
</html>
```












