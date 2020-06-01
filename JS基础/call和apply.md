# call()和apply()
- 这两个方法都是函数对象的方法，需要通过函数对象来调用。
- 当对函数调用call和apply都会调用函数执行。
- 在调用call和apply可以将一个对象指定为第一个参数。
   + 此时这个对象会成为函数执行时的this。
- call()方法可以将实参在对象之后依次传递。
- apply()方法需要将实参封装到一个数组中统一传递。
- this的情况：
   + 以函数的形式调用，this是window。
   + 以方法形式调用时，this是调用方法的对象。
   + 以构造函数的形式调用时，this是新创建的那个对象。
   + 使用call和apply调用时，this是指定那个对象。
```
🌰：
function fun(x,y){
 console.log(x,y);
};

fun();
fun.call();
fun.apply();

var obj={
  name:"obj",
  sayName:function(){ 
    console.log(this.name) 
   }，
   sayHello(A,B){
     console.log(this.a);
     console.log(this.b); 
   }
}；
var obj2 ={
  name:"obj2"
}
//此时的fun实际上是当成对象来使用的，对象可以调用方法
//call和apply方法也是函数的调用的方式

//若apply与call方法中如果没有传入参数或者传入的是null。那么调用该方法的函数对象中的this就是默认的window
fun.call();
fun.call(null);

//格式：函数名.call(对象,参数1，参数2); 方法名.call(对象,参数1，参数2);
//格式：函数名.apply(对象,数组);  方法名.apply(对象,数组);

fun.call(obj)； //a，b皆为undefind
fun.call(obj，1，2)；//a为1，b为2
fun.apply(obj，[1，2])；//a为1，b为2

obj.sayName.call(obj2);// 输出为obj2

fun.apply(obj2,[10,20]);//此时指向为obj
```

