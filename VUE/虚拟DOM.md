# 虚拟DOM
### 模板转换为视图的过程
![模板转换为视图的过程](http://zhangzqcloud.cn/file-images/%E6%A8%A1%E6%9D%BF%E8%BD%AC%E6%8D%A2%E4%B8%BA%E8%A7%86%E5%9B%BE%E7%9A%84%E8%BF%87%E7%A8%8B.png)

### 虚拟DOM是什么
- 虚拟DOM是一棵以JS对象(VNode 结点)作为基础的树，并且用对象的属性来描述结点，是真实DOM的抽象。

### 为什么要有虚拟DOM
- 因为在不使用虚拟DOM的情况下，每次通过js操作DOM的时候，都会引起页面的重排重绘，这样会增加浏览器的性能开销，降低页面的
渲染速度。


- 参考：
   - [揭秘Vue中的Virtual Dom](https://github.com/ljianshu/Blog/issues/69)
   - [vue核心之虚拟DOM](https://www.jianshu.com/p/af0b398602bc)
   
   
