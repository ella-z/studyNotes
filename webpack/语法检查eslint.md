# 语法检查eslint
- 语法检查作用：规范书写格式，检查常见的语法错误，主要针对于js文件。
- 在webpack中使用eslint-loader --依赖于-> eslint，所以安装eslint-loader同时也要安装eslint。
- 需要在package.json中eslintConfig中设置检查的规则(推荐使用airbnb，相关插件：eslint-config-airbnb-base(有base的，是不包括react plugins的。没有base，则是包括了react plugins))
   - 若使用 airbnb ，需要下载eslint-config-airbnb-base 、 eslint 、 eslint-plugin-import。
   - 因为eslint不能识别window、navigator等全局变量，所以需要修改package.json中eslintConfig的配置
   ```
   🌰：
    //在package.json中
    "eslintConfig":{
       "extends":"airbnb-base",
       "env":{
         "browser":true //支持浏览器端全局变量
         "node":true //支持nodejs的变量
       }
     }
   ```
```
🌰：
//在webpack.config.js中设置语法检查
const path = require('path');
const HtmlWebpackPlugins = require('html-webpack-plugin');

module.exports = {
    entry: './src/js/index.js',
    output: {
        filename: 'js/built.js',
        path: path.resolve(__dirname + '/build')
    },
    module: {
        rules: [
            {
                test: /\.js/,
                //排除第三方库的检查，只检查自己写的源代码
                exclude: /node_modules/,
                loader: 'eslint-loader',
                options:{
                    //自动修复eslint的错误
                    fix:true
                }
            }

        ]
    },
    plugins: [
        new HtmlWebpackPlugins({
            template: './src/index.html'
        })
    ],
    mode: 'development'
}
//在package.json中设置语法检查使用那种规则
    "eslintConfig":{
       "extends":"airbnb-base"
     }
     
// 若出现了语法不规范，就会跳出下面这种类似的错误
  1:15  error    A space is required after ','                    comma-spacing
  1:18  error    Missing space before opening brace               space-before-blocks
  1:19  error    Expected linebreaks to be 'LF' but found 'CRLF'  linebreak-style
  2:1   error    Expected indentation of 2 spaces but found 4     indent
  2:13  error    Operator '+' must be spaced                      space-infix-ops
  2:16  error    Expected linebreaks to be 'LF' but found 'CRLF'  linebreak-style
  3:2   error    Expected linebreaks to be 'LF' but found 'CRLF'  linebreak-style
  4:1   error    Expected linebreaks to be 'LF' but found 'CRLF'  linebreak-style
  5:1   warning  Unexpected console statement                     no-console
  5:18  error    A space is required after ','                    comma-spacing
  5:23  error    Newline required at end of file but not found    eol-last
  
  //若在js中注释了eslint-disable-next-line，就会使得一下行eslint所有规则失效，也就是下一行不进行eslint检查。
  🌰：
  //eslint-disable-next-line
  console.log(x);
```











