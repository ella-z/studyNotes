# vue双向绑定
### 双向绑定的原理
- vue的双向绑定是通过使用Object.defineProperty()进行数据劫持，再结合发布者-订阅者模式来实现的。
   - Object.defineProperty()，该方法可以直接在一个对象定义一个新属性，或者修改一个对象的现有属性，并返回此对象。
      - Object.defineProperty(obj, prop, descriptor)，obj定义属性的对象，prop要定义或修改的属性的名称或者Symbol，descriptor
要定义或修改的属性描述符。
         - get，属性的getter函数，若没有getter，默认为undefind。当访问该属性时，会调用此函数。执行时不传入任何参数，但是会传入this对象，该函数的返回值会被用作属性的值。
         - set，属性的setter函数，若没有setter，默认为undefind。当属性值被修改时，会调用此函数。该方法接受一个参数（也就是被赋予的新值），会传入赋值时的 this 对象。

### 双向绑定的实现
- mvvm的实现主要包含两个方面：数据变化更新视图、视图变化更新数据。
   - 视图改变更新数据，可以通过事件监听实现。
   - 数据变化更新视图，可以通过Object.defineProperty()实现，给属性设置一个set函数，当给属性赋新的值的时候，同时对视图进行更新。
- 实现双向绑定需要：
   1. 监听器Observer，用来劫持并监听所有属性，如果监听的数据有变动，就通知订阅者。
   2. 订阅者Watcher，可以收到属性变化通知并执行相应的函数，从而更新视图。
   3. 解析器Compile，扫描和解析每个节点的相关指令，并根据初始化模板数据以及初始化相应的订阅器。
   4. 消息订阅器Dep，专门收集订阅者，然后在Observer和订阅者Watcher之间进行统一管理。
   <br />
   <img src="" width="500px" height="400px">
   
#### 代码实现
- 简单实现
```
🌰：
function defineReactive(data, key, val) {
    observer(val); // 递归遍历所有子属性
    var dep = new Dep(); //添加订阅器
    Object.defineProperty(data, key, {
        enumerable: true,
        configurable: true,
        get: function () {
            if (Dep.target) { //添加订阅者
                dep.addSub(Dep.target);
            }
            return val;
        },
        set: function (newVal) {
            val = newVal;
            console.log('属性' + key + '已经被监听了，现在值为：“' + newVal.toString() + '”');
        }
    });
}

function observer(data) { //监听器
    if (!data || typeof data !== 'object') {
        return;
    } else {
        //Object.keys() 方法会返回一个由一个给定对象的自身可枚举属性组成的数组,数组中属性名的排列顺序和正常循环遍历该对象时返回的顺序一致 。
        Object.keys(data).forEach(function (key) {
            defineReactive(data, key, data[key]);
        });
    }
}

function Dep() {
    this.subs = [];
}
Dep.prototype = {
    addSub: function (sub) { //添加订阅者
        this.subs.push(sub);
    },
    notify: function () { //通知订阅者
        this.subs.forEach(function (sub) {
            sub.update();
        });
    }
}

function Watcher(vm, exp, cb) { //订阅者，vm：组件实例化对象，exp：要观察的表达式，cb回调函数
    this.cb = cb;
    this.vm = vm;
    this.exp = exp;
    this.value = this.get();  // 将自己添加到订阅器的操作
}

Watcher.prototype = {
    update: function () {
        this.run();
    },
    run: function () {
        var value = this.vm.data[this.exp];
        var oldVal = this.value;
        if (value !== oldVal) {
            this.value = value;
            this.cb.call(this.vm, value, oldVal);
        }
    },
    get: function () {
        Dep.target = this;  // 缓存自己
        var value = this.vm.data[this.exp]  // 强制执行监听器里的get函数
        Dep.target = null;  // 释放自己
        return value;
    }
};

function SelfVue(data, el, exp) { //监听者与观察者关联起来
    this.data = data;
    observer(data);
    el.innerHTML = this.data[exp];  // 初始化模板数据的值
    new Watcher(this, exp, function (value) {
        el.innerHTML = value;
    });
    return this;
}

var ele = document.querySelector('#name');
var selfVue = new SelfVue({
    name: 'jack'
}, ele, 'name');

window.setTimeout(function () {
    console.log('name值改变了');
    selfVue.data.name = 'rose';
}, 2000);
```


- 参考：
- [vue的双向绑定原理及实现](https://juejin.im/entry/5923973da22b9d005893805a)
- [object.defineproperty](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)












