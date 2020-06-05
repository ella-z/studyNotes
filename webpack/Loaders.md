# Loaders
- 可以预处理源文件。

### url-loader
- 作用：当文件文件小于指定大小的时候，将文件转化成DATAURL的形式也就是base64形式(这种做法是为了避免使用雪碧图来存放小图片，而是将这些小图片打包存
放在某个文件中)。
- 安装：npm install url-loader --save-dev
```
🌰：
module.exports = {
  module: {
    rules: [ 
      {
        test: /\.(png|jpg|gif)$/i,   //为了对文件的扩展名进行过滤，当文件的扩展名符合test的正则表达式的时候，那么将会使用url-loader对文件进行处理
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 8192           //当文件小于8K的时候，将文件转化为DATAURL的形式并包含在某个文件中，大于8K则直接打包为一个物理文件
            }
          }
        ]
      }
    ]
  }
}
```

### babel-loader
- 作用：将ES6、ES7等的代码转换称为ES5的代码。
- 安装：npm install -D babel-loader @babel/core @babel/preset-env webpack
```
🌰：
module: {
  rules: [
    {
      test: /\.m?js$/,    
      exclude: /(node_modules|bower_components)/,  //排除掉的文件夹的名称
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['@babel/preset-env'],  //转换的规则
          plugins: ['@babel/plugin-transform-react-jsx'] //加入插件，支持了react的转换
        }
      }
    }
  ]
}
```

### sass-loader
- 作用：将sass/scss文件转化称为css。
- 安装：npm install sass-loader node-sass webpack --save-dev
```
🌰：
//因为使用到了style-loader与css-loader，还需要安装它们：npm install style-loader css-loader --save-dev
module.exports = {
    ...
    module: {
        rules: [{
            test: /\.scss$/,
            use: [
                "style-loader", // 将 JS 字符串生成为 style 节点
                "css-loader", // 将 CSS 转化成 CommonJS 模块
                "sass-loader" // 将 Sass 编译成 CSS，默认使用 Node Sass
            ]
        }]
    }
};
```





