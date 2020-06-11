# externals
- 目的：将不怎么需要更新的第三方库脱离webpack打包，不被打入bundle中，从而减少打包时间，但又不影响运用第三方库的方式，例如import方式等。
```
🌰：
//在 webpack.config.js中
externals{
//拒绝jQuery被打包进来
  jquery:'jQuery'
}

//若要使用，需要在html中通过cdn连接引入进来
<script src="https://cdn.bootcss.com/jquery/1.12.4/jquery.min.js"></script>

```
