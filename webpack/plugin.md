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




