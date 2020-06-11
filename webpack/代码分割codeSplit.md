# code split
- 代码分割作用：能够把代码分离到不同的 bundle 中，然后可以按需加载或并行加载这些文件。
- chunk：是webpack内部运行时的概念；一个chunk是对依赖图的部分进行封装的结果，可以通过多个entry-point来生成一个chunk。
   - chunk分为三类：
   1. entry chunk，包含webpack runtime code并且是最先执行的chunk。
   2. initial chunk，包含同步加载进来的module且不包含runtime code的chunk，在entry chunk执行后再执行的。
   3. normal chunk，使用require.ensure、System.import、import()异步加载进来的module，会被放到normal chunk中。
- bundle：最终输出的chunk在用户端，被称之为bundle；一般一个chunk对应一个bundle，只有在配置了sourcemap时，才会出现一个chunk对应多个bundle的情况。   
### 多入口
- 可以指定多个入口实现代码分隔
```
   entry: './src/js/index.js' //单入口
   entry:{
        //多入口：
        main:'./src/js/index.js',
        test:'./src/js/test.js'
    }  
    
  🌰：
  module.exports = {
    entry:{
        //多入口
        main:'./src/js/index.js',
        test:'./src/js/test.js'
    },
    output: {
        //name:取文件名
        filename: 'js/[name].js',
        path: path.resolve(__dirname + '/build')
    },
    plugins: [
        new htmlWebpackPlugin({
            template: './src/index.html',
            minify: {
                collapseWhitespace: true,
                removeComments: true
            }
        })
    ],
    mode: 'production'
}
 //最终build/js中有两个js文件，一个为main.js，另一个为test.js 
```

  
### optimization
- 可以node_modules中代码单独打包一个chunk输出。
- optimization会自动分析多入口chunk中，有没有公共的文件(例如：引用了同一个库)。如果有会将这个库单独打包成为一个chunk
```
optimization:{
    splitChunks:{
         chunks:'all'
    }
}
🌰：
//在webpack.config.js中
module.exports = {
    entry:{
        //多入口
        main:'./src/js/index.js',
        test:'./src/js/test.js'
    },
    output: {
        //name:取文件名
        filename: 'js/[name].js',
        path: path.resolve(__dirname + '/build')
    },
   
    plugins: [
        new htmlWebpackPlugin({
            template: './src/index.html',
            minify: {
                collapseWhitespace: true,
                removeComments: true
            }
        })
    ],
    optimization:{
        splitChunks:{
            chunks:'all'
        }
    },
    mode: 'production'
} 

//在index.js中
import jq from 'jquery'

const add = (x, y) => x + y;

// eslint-disable-next-line
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    console.log('end');
    resolve();
  }, 1000);
});

// eslint-disable-next-line
console.log(add(2, 5));


console.log(jq);

//最终jq代码会单独打包一个chunk输出

```

### 通过js代码，让某个文件被单独打包成一个chunk
- import动态导入：能将某个文件单独打包。
```
🌰：
// /*webpackChunkName:'test'*/给打包的文件命名为test
import(/*webpackChunkName:'test'*/'./test').then(
  ()=>{
    console.log('success!')
  }
).catch(()=>{
  // eslint-disable-next-line
  console.log('文件加载失败！')
})
//test.js会被单独打包
```













  
