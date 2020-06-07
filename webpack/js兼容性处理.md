# js兼容性处理
- js兼容性处理会将ES6以及以上的语法转换成ES5和ES5以下的语法，用来兼容各种浏览器。

### 使用babel-loader进行兼容性处理
#### @babel/preset-env
- 基本js兼容性处理，只可以转换基本的js语法，但类似promise的语法无法转换。
- 需要使用到babel-loader @babel/core @babel/preset-env
```
🌰：
//webpack.config.js中配置
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
                test:/\.js$/,
                exclude:/node_modules/,
                loader:'babel-loader',
                options:{
                    //对babel进行预设
                    presets:['@babel/preset-env']
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

//js文件中
const add = ( x,y )=>{
    return x+y;
}

console.log(add(2,5));

//打包输出的js文件中，转化成为了ES5和ES5以下的语法
eval("var add = function add(x, y) {\n  return x + y;\n};\n\nconsole.log(add(2, 5));\n\n//# sourceURL=webpack:///./src/js/index.js?");
```

#### @babel/polyfill
- 可以处理全部js兼容性，因为@babel/polyfill会将所有需要用到的js兼容性的代码全部引入。
   - 问题：若js只需部分兼容性的代码，引入全部代码会导致文件体积太大，造成不必要的浪费。
- 需要使用到babel-loader @babel/core @babel/polyfill
```
🌰1：
//将@babel/polyfill下载完成后，可以在js文件中直接使用import引用
//@babel/polyfill会将所有需要用到的js兼容性的东西全部引进来
import '@babel/polyfill';

const add = ( x,y )=>{
    return x+y;
}

const promise = new Promise((resolve,reject)=>{
    setTimeout(()=>{
        console.log('end');
        resolve();
    },1000)
})

console.log(add(2,5));
```

### core-js
- 可以按需加载转化js所需要的库
- 需要安装core-js：npm i core-js -D 
```
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
                test: /\.js$/,
                exclude: /node_modules/,
                loader: 'babel-loader',
                options: {
                    //对babel进行预设
                    presets: [['@babel/preset-env', {
                        //按需加载
                        useBuiltIns: 'usage',
                        //指定core-js的版本
                        corejs: {
                            version: 3
                        },
                        //指定兼容性兼容到哪个版本的浏览器
                        targets:{
                            chrome:'60',
                            firefox:'60',
                            ie:'9',
                            safari:'10'
                        }
                    }]]
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
```









