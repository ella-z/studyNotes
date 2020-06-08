# webpack性能优化
## 开发环境性能优化
- 优化打包构建速度
- 优化代码调试

## 生产环境性能优化
- 优化打包构建速度
- 优化代码运行的性能

### 开发环境中优化打包构建速度-HMR
- HMR(模块热替换)作用：当一个模块发生变化时，只会重新打包这一个模块，而不是重新打包所有模块，这样会极大地提升构建的速度。
- html文件默认不能使用HRM功能，同时还会导致html文件不能热更新。
   - html文件不需要HRM功能，因为html文件只有一个，若这个模块改变了，必然要重打包。
   - 不能热更新的解决方法：修改entry入口，并将html文件引入。
   ```
   entry：['./src/js/index.js','./src/index.html']
   ```
- js文件默认不能使用HMR功能，js文件的HRM功能只能处理非入口js文件。
   - 解决方法：修改js代码，添加支持HRM功能
   ```
   //在入口的js文件中添加
   //全局寻找module，并且查看是否有hot属性，若hot属性的值为true，说明开启了HMR功能
    if(module.hot){
    //该方法会监听print.js文件的变化，一旦发生了变化，其他js文件不会重新打包构建，并执行回调函数。
      module.hot.accept('./print.js',function(){
      })
    }
   
   ```
- 样式文件可以使用HMR功能，因为在style-loader中内部实现了该功能。
```
var path = require('path');

module.exports = {
  //...
  devServer: {
    contentBase: path.join(__dirname, 'dist'),  //contentBase指定在哪个目录启动服务
    compress: true, //压缩
    port: 9000, //端口
    hot: true //启动热替换
  }
};
```

### 开发环境中优化调试代码-source-map
- source-map：一种提供源代码到构建后代码映射技术(若构建后代码出错了，可以通过映射追踪到源代码的错误)。
   - [inline-|hidden-|eval-][nosources-][cheap-[module-]]source-map
      1. source-map:外部，可以提供错误代码的准确信息以及源代码的错误位置。
      2. inline-source-map:内联，内联相对于外部，不会另外生成.map文件，直接内联到输出的js文件中，并且内联构建的速度更快。
      并且可以提供错误代码的准确信息以及源代码的错误位置。
      3. hidden-source-map:外部，可以提供错误代码的准确信息，但是无法提供源代码的错误位置，只能提供构建后代码的错误位置。
      4. eval-source-map：内联，可以提供错误代码的准确信息以及源代码的错误位置。
         -  inline-source-map与eval-source-map区别：inline-source-map只会生成一个内联source-map，而eval-source-map在每个文件中都生成
         对应的source-map，并且都在eval函数中。
      5. nosources-source-map：外部，可以提供错误代码的准确信息，但是没有源代码和构建后代码信息(为了隐藏代码)。
      6. cheap-source-map：外部，可以提供错误代码的准确信息以及源代码的错误位置(只精确到错误行)。
      7. cheap-module-source-map：外部，可以提供错误代码的准确信息以及源代码的错误位置(只精确到错误行)。
         - 与cheap-source-map区别：module会将loader的source map加入
- 在开发环境中，要求：速度快，调试友好度。
   - 速度(eval>inline>cheap>...)：eval-cheap-source-map>eval-source-map
   - 友好度：source-map>cheap-module-source-map>cheap-source-map
   - 最终：eval-source-map / eval-cheap-module-source-map
- 在生产环境中，要求：是否隐藏源代码以及调式友好度。
   - 内联会让代码体积变大，所以一般在生产环境中不使用内联。
   - 若不需要隐藏源代码，可以使用source-map / cheap-module-source-map
   - 若需要隐藏源代码，则使用hidden-source-map/nosources-source-map
```
var path = require('path');

module.exports = {
  //...
  devtool:'source-map'
};
```

### 生产环境性能优化-oneOf
- oneOf：提升构建速度。
```
🌰：
 module: {
        rules: [
        //！在oneOf中不能有两个配置处理同一种类型文件，所以要将其中一个配置js的loader调出oneOf！
            {
                test: /\.js$/,
                exclude: '/node_module/',
                //优先执行
                enforce: 'pre',
                //使用eslint语法检查
                loader: 'eslint-loader',
                options: {
                    //自动修复eslint的错误
                    fix: true
                }
            },
            {
                //只会匹配一个loader,匹配到一个后就不会继续往下匹配，更高性能
                oneOf: [
                    {
                        test: /\.css$/,
                        use: [...commonCss]
                    },
                    {
                        test: /\.less$/,
                        use: [
                            ...commonCss,
                            "less-loader"
                        ]

                    },
                    {
                        test: /\.js$/,
                        exclude: '/node_module/',
                        //优先执行
                        enforce: 'pre',
                        //使用eslint语法检查
                        loader: 'eslint-loader',
                        options: {
                            //自动修复eslint的错误
                            fix: true
                        }
                    },
                    {
                        test: /\.js$/,
                        exclude: /node_modules/,
                        //处理js兼容性的问题
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
                                targets: {
                                    chrome: '60',
                                    firefox: '60',
                                    ie: '9',
                                    safari: '10'
                                }
                            }]]
                        }
                    },
                    {
                        //对图片进行处理
                        test: /\.(jpg|png|gif)/,
                        loader: 'url-loader',
                        options: {
                            limit: 8 * 1024,
                            name: '[hash:10].[ext]',
                            //关闭es6模块化
                            esModule: false,
                            //指定输出路径
                            outputPath: 'imgs'
                        }
                    },
                    {
                        //处理html文件
                        test: /\.html$/,
                        use: [
                            'html-loader'
                        ]
                    },
                    {
                        //处理其他文件
                        exclude: /.(html|css|less|jpg|png|gitf|js)/,
                        loader: 'file-loader'
                    }
                ]
            }
        ]
    }
```

