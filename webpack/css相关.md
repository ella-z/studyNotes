# css相关
## 提取css成单独文件
### MiniCssExtractPlugin
- 作用：将sass/scss转化称为css之后，将css输出到文件中去，将css提取成单独的文件。
- 安装：npm install --save-dev mini-css-extract-plugin
```
🌰1：
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
          MiniCssExtractPlugin.loader,//MiniCssExtractPlugin.loader instead of style-loader
          'css-loader',
          'postcss-loader',
          'sass-loader',
        ],
      }
    ]
  }
}

🌰2：
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
    entry: './src/js/index.js',
    output: {
        filename: 'js/built.js',
        path: path.resolve(__dirname, './build')
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use:[  
                    // MiniCssExtractPlugin.loader代替style-loader，作用：提取js中的css成到哪都文件
                    MiniCssExtractPlugin.loader,
                    'css-loader'
                ]
            }
        ]
    },
    plugins:[
        new HtmlWebpackPlugin({
            template:'./src/index.html'
        }),
        new MiniCssExtractPlugin()
    ],
    mode:'development'
}
```

## css兼容性处理
- 使用postcss-preset-env与postcss-loader
### postcss-preset-env（postcss-loader的插件）
- 作用：识别环境(可以精确到某个浏览器的版本)以及加载配置，帮助postcss找到package.json中browserslist里面的配置，然后通过配置加载
指定的css兼容性样式，browserslist需要手动加入package.json文件中。
   ```
   🌰：
   //package.json中
   "browerslist"：{
    "development"：[
    // 若要改成开发环境，需要设置node环境变量：process.env.NODE_ENV='development'
    // 兼容最近的chrome、firefox、safari版本
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ],
    "production":[
     //默认是生产环境
      ">0.2%",  //兼容大于98%的浏览器
      "not dead", //不要兼容已经弃用的浏览器
      "not op_mini all" //不要兼容op_mini浏览器
    ]
   }
   ```
```
🌰：
//webpack.config.js中
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

//设置nodejs环境变量
process.env.NODE_ENV='production';

module.exports = {
    entry: './src/js/index.js',
    output: {
        filename: 'js/built.js',
        path: path.resolve(__dirname, './build')
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use:[  
                    MiniCssExtractPlugin.loader,
                    'css-loader',
                    //使用loader的默认配置直接写：postcss-loader
                    //修改loader的配置
                    {
                        loader:'postcss-loader',
                        options:{
                            ident:'postcss',
                            plugins:()=>[
                                //postcss插件
                                require('postcss-preset-env')()
                            ]
                        }
                    }
                ]
            }
        ]
    },
    plugins:[
        new HtmlWebpackPlugin({
            template:'./src/index.html'
        }),
        new MiniCssExtractPlugin({
            filename:'css/bulit.css'
        })
    ],
    mode:'development'
}
```













