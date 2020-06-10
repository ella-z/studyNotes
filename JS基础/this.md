# this
- 解析器在调用函数每次都会向函数内部传递进一个隐含的参数。   
   - 这个隐含的参数就是this，this指向的是一个对象
   - 这个对象我们称为上下文对象
   - 根据函数的调用方式的不同，this会指向不同的对象
      1. 以函数的形式调用时，this永远指向的都是window，🌰：fun（）//此时输出的this为window
      2. 以方法的形式调用时，this就是调用方法的那个对象，🌰：obj.fun（）//此时输出的this为obj对象
      3. 以构造函数形式调用时，this指得就是构造函数的对象。

- 函数中的this的指向
- this永远指向的是调用它的对象，在new对象的时候，指向的是new出来的对象。
   - 普通函数中的this ---> window
   - 定时器方法中的this ---> window
   - 构造函数中的this ---> 实例对象
   - 对象.方法中的this ---> 当前的实例对象
   - 原型方法中的this ---> 实例对象
```
🌰1：
var obj = {
    name: 'jack',
    sayName: function(){
        console.log('name:',this.name);
    }
}
obj.sayName(); //jack,因为sayName是obj调用的，所以this指向的是obj

🌰2:
var name = 'mikey';
var obj = {
    name: 'jack',
    sayName: function(){
        return function(){
            console.log('name:',this.name);
        }
    }
}
obj.sayName()(); // mikey，因为闭包中的this指向调用它的对象，而调用它的对象就是window对象

🌰3:
var fun = function (a){
    this.a = a;
    return function(b){
        console.log('a+b:', this.a + b);
    }
}((function(a,b){return a})(1,2));
fun(3);// 4，fun是一个立即执行的函数，它的参数也是一个立即执行的函数。(function(a,b){return a})(1,2) 执行的结果是1，再将1传入函数fun中并赋值给a。当fun(3)的时候，fun中的闭包开始执行，所以相当于function(b){console.log('a+b:', this.a + b);}(3); ，将3赋值给b，而此时调用闭包的是fun，所以闭包中的this指向的是fun，所以this.a=1。

```

### ES6中箭头函数的this
- 箭头函数没有自己的this，所以在内部使用了this时，它会指向最近一层作用域内的this。也就是说，this是继承自父执行的上下文。
```
🌰1：
var obj = {
    name: 'jack',
    sayName: function(){
        return () => {
            console.log('name:', this.name);
        }
    }
}
obj.sayName()(); //jack

```
[更多](https://blog.csdn.net/w390058785/article/details/82884032)















