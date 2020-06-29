# 虚拟DOM
### 真实的DOM解析的流程
1. 创建DOM树，用HTML分析器，分析HTML元素，构建DOM树。
2. 创建StyleRules，分析CSS文件和元素上的inline的样式(css的解析是从右往左逆向解析的，嵌套标签越多解析越慢)，生成样式表。
3. 创建Render树，将DOM树和样式表关联起来，构建一棵Render树。每个DOM节点都有attach方法，接受样式信息，返回一个render对象。这些返回的Render对象最终构成一棵Render树。
4. 布局Layout，为Render树上的每个结点确定坐标。
5. 绘制Painting，调用每个结点的print方法，将它们全部绘制出来。

### 虚拟DOM是什么
- 虚拟DOM是一棵以JS对象作为基础的树，并且用对象的属性来描述结点，是真实DOM的抽象。并且这个JS对象最少包含三个属性：标签名、属性和子元素对象。

### 为什么要使用虚拟DOM
- 因为在不使用虚拟DOM的情况下，每次通过js操作DOM的时候，都会引起页面的重排重绘(浏览器会从构建DOM树开始从头到尾执行一遍流程)，这样会增加浏览器的性能开销，降低页面的渲染速度。
- 使用虚拟DOM，可以先将页面更新全部反映在虚拟DOM上，等更新完成后，再将最终的JS对象映射成真实的DOM，交由浏览器绘制。
- 使用虚拟DOM可以实现跨平台，因为虚拟DOM是以JS对象为基础而不依赖真实平台环境，所以它可以跨平台使用。

### 模板转换为视图的过程
![模板转换为视图的过程](http://zhangzqcloud.cn/file-images/%E6%A8%A1%E6%9D%BF%E8%BD%AC%E6%8D%A2%E4%B8%BA%E8%A7%86%E5%9B%BE%E7%9A%84%E8%BF%87%E7%A8%8B.png)

- 渲染函数：渲染函数是用来生成虚拟DOM的。
- VNode 虚拟节点：它可以代表一个真实的 dom 节点。通过createElement方法能将VNode渲染成dom节点。
- patch(也叫做patching算法)：虚拟DOM最核心的部分，它可以将vnode渲染成真实的DOM，这个过程是对比新旧虚拟节点之间有哪些不同，然后根据对比结果找出需要更新的的节点进行更新。

### 虚拟DOM的作用
- 提供与真实DOM节点对应的虚拟节点vnode。
- 将虚拟节点vnode和旧虚拟节点oldVnode进行对比，然后更新视图。

## diff算法
- diff算法的时间复杂度为O(n)。
### diff算法的作用
- 在虚拟节点vnode和旧虚拟节点oldVnode进行对比时，找出需要更新的节点进行更新，其他的不更新。
<br />
<img src="http://zhangzqcloud.cn/file-images/diff%E7%AE%97%E6%B3%95.png" title="diff算法" width="400px" height="250px">

- diff算法的步骤：
1. 用 JavaScript 对象结构表示 DOM 树的结构；然后用这个树构建一个真正的 DOM 树，插到文档当中。
2. 当状态变更的时候，重新构造一棵新的对象树。然后用新的树和旧的树进行比较，记录两棵树差异(这个过程中使用到深度优先遍历)。
3. 把所记录的差异应用到所构建的真正的DOM树上，更新视图。

## key
### key的作用
- 使用key作为每个节点的唯一标识，从此diff算法可以正确地、快速地识别到此节点，然后在正确的位置区域插入新的节点。
- vue中在使用相同标签名元素的过渡切换时，使用key属性，使得vue可以区分它们，否则vue只会替换其内部属性而不会触发过渡效果。

<br />
<img src="http://zhangzqcloud.cn/file-images/key.png" title="key" width="500px" height="200px">

### 为什么key最好不要设定为index
- 因为使用v-for渲染元素时，使用元素自身的id属性去指定渲染元素的key值有利于单个元素的重新渲染，若采用其他如v-for提供的index, key等值，在改变渲染出来的DOM结构时，会触发所有元素的重新渲染，当数据过大时，可能会造成性能负担。



- 参考：
   - [揭秘Vue中的Virtual Dom](https://github.com/ljianshu/Blog/issues/69)
   - [vue核心之虚拟DOM](https://www.jianshu.com/p/af0b398602bc)
   - [vue中：key的作用](https://www.jianshu.com/p/0044532e4a93)
   - [v-for 响应式key, index及item.id参数对v-bind:key值造成差异研究](https://www.cnblogs.com/tim100/p/7262963.html?tdsourcetag=s_pcqq_aiomsg)
   
   
