# PWA
- PWA渐进式网络开发应用。
- 作用：在移动端利用提供的标准化框架，在网页应用中实现和原生应用相近的用户体验的渐进式网页应用。
- 优点：
   1. 即时加载，即使在不确定的网络条件下也不会受到影响。
   2. 速度快
   3. 沉浸式体验，感觉就像设备上的原生应用程序，具有沉浸式的用户体验。
- 使用PWA后，可实现离线访问。
- 通过workbox实现，在webpack中需要使用workbox-webpack-plugin插件
   - 在webpack.config.js中配置插件，通过插件注册serviceworker(注意：serviceworker的代码必须运行在服务器上，可以通过nodejs部署到服务器上)
    
```
🌰：
//在webpack.config.js中
//生成servicework的配置文件，通过该配置文件注册serviceworker
new workboxWebpackPlugin.GenerateSW({
   //帮助serviceworker快速启动
   clientsClaim:true,
   //删除旧的serviceworker
   skipWaiting:true
})

//在入口的js文件中
// 注册serviceworker，并且处理其兼容性的问题(检查浏览器是否支持serviceworker)
if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    // 祖册serviceworker
    navigator.serviceWorker.register('/service-worker.js/').then(() => {
      console.log('servicework success!');
    }).catch(() => {
      console.log('servicework failed!');
    });
  });
}

//最终生成service-worker.js文件
// 若网络离线之后，会到service-worker中寻找资源并显示出来。
```
