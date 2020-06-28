# VUE的生命周期
![VUE的生命周期](https://cn.vuejs.org/images/lifecycle.png)

### VUE的生命周期的过程
1. 实例化Vue对象
2. 初始事件和生命周期
3. 初始化数据
4. 编译模板
5. 挂载DOM和渲染DOM
6. 更新数据和重新渲染DOM
7. 销毁

### 生命周期中的钩子函数
- beforeCreate  实例创建前执行
- created       实例创建完成执行
- beforeMount   挂载前执行
- mounted       挂载完成执行
- beforeUpdate  更新前执行
- updated       更新完成执行
- beforeDestory 销毁组件前执行
- destoryed     销毁组件完成执行

#### beforeCreated
- 实例初始化之后、创建实例之前执行的钩子函数。
- 此时的el还没有被挂在以及data还没被初始化，所以此时的数据也没有、DOM也还没生成。
```
🌰：
const vm = new Vue({
  el: '#app',
  data: {
        data: '数据初始化'
    },
    beforeCreate() {
        console.log('beforeCreate：');
        console.log(this.$data);
        console.log(this.$el);
   }
})

//输出的结果为
beforeCreate：
undefined
undefined
```
#### created
- 创建实例后执行的钩子函数。
- 此时的data已经完成了初始化，但是el还是还未挂载。
```
🌰：
const vm = new Vue({
    el: '#app',
    data: {
        data: '数据初始化'
    },
    created() {
        console.log('created钩子事件：');
        console.log(this.$data);
        console.log(this.$el);
    }
})
//输出的结果为
created：
{__ob__: Observer} //data
undefined
```
#### beforeMount
- 此时已经编译好了模板但是还没有进行挂载。
- 模板已经开始编译，但是数据还没有被渲染到模板中。

#### mounted钩子函数
- 此时模板已经编译完成并且挂载完毕，数据也渲染到了模板中

#### beforeUpdate
- 这个钩子函数在数据发生更新前，dom被重新渲染前执行。

#### updated
- 这个钩子函数会在数据更新完毕后，dom被重新渲染后执行。

#### beforeDestroy
- 该函数会在实例被销毁前执行。

#### destroyed
在实例被成功销毁之后执行，此时该实例与其他实例的关联已经被清除，Vue实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。



- 参考：
   - [标注图+部分举例聊聊Vue生命周期](https://juejin.im/post/5bd6962e51882558bd3f0696)
   












