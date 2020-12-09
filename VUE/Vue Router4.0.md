# Vue Router4.0
- 现在的路由主要分为三个模块:
   1. History 实现:处理地址栏,并且特定于Vue Router运行的环境.
   2. Router 匹配器:处理类似 /users/:id的路由解析和优先级处理.
   3. Router:将一切连接在一起,并处理路由特定功能,例如导航守卫.
   
## Vue Router4.0的改变
### 动态路由
- Vue Router4.0新增了自动优先级排名.
- 在之前的Vue Router中,当有两个路由同时都匹配输入的路径的时候,Vue Router会根据路由声明的顺序来判断执行哪一个路由.而Vue Router4.0中是根据得分系统来计算该匹配哪个路由与路由放置的顺序无关.
### Vue Devtools
- 在新的Vue Devtools中,可以利用时间轴记录路由变化以及有完整的route目录,能够帮你轻松进行调试.
### 路由守卫
- 在Vue Router4.0的路由守卫中,现在确认跳转不用再手动执行next这个函数了,而是根据返回值来决定,而且现在同样支持异步返回Promise.
### 一致编码
- 编码方式（Encoding）做了统一的适配，现在将在不同的浏览器和路由位置属性（params, query 和 hash）中保持一致。 作为参数传递给 router.push() 时，不需要做任何编码，在你使用 $route 或 useRoute()去拿到参数的时候永远是解码（Decoded）的状态。

## 参考
- [Vue Router 4.0 正式发布](https://mp.weixin.qq.com/s/I5lNX0R-E6xzVGuYPZ8o8Q)
   
