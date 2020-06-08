# 压缩html和js
### js压缩
- 生产环境下，会自动加载UglifyJsPlugin插件，该插件会压缩js。所以要实现js压缩，只需将mode改为production模式，在production模式下，会自动压缩js文件。
```
🌰：
const path = require('path');
const HtmlWebpackPlugins = require('html-webpack-plugin');

module.exports = {
    entry: './src/js/index.js',
    output: {
        filename: 'js/built.js',
        path: path.resolve(__dirname + '/build')
    },
    
    plugins: [
        new HtmlWebpackPlugins({
            template: './src/index.html'
        })
    ],
    //生产环境下会自动压缩js代码
    mode: 'production'
}
```

### html压缩
- 可以通过HtmlWebpackPlugins来压缩html。
```
🌰：
const path = require('path');
const HtmlWebpackPlugins = require('html-webpack-plugin');

module.exports = {
    entry: './src/js/index.js',
    output: {
        filename: 'js/built.js',
        path: path.resolve(__dirname + '/build')
    },
    
    plugins: [
        new HtmlWebpackPlugins({
            template: './src/index.html',
            minify:{
                //通过collapseWhitespace移除空格，removeComments移除注释来对html进行压缩
                collapseWhitespace:true,
                removeComments:true
            }
        })
    ],
    mode: 'production'
}
```






