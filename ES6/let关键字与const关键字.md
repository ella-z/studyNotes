### let关键字
- let声明的变量只在所处于的块级中有效，而var声明的变量不具备块级作用域特性。
- let不存在变量提升，不可以先使用再声明。
- let具有暂时性死区。
```
🌰:
if(true){
  let a = 10;
  if(true){
    let b = 20;
  }
  console.log(b);//not defined
}
console.log(a);//not defined


if(true){
 var c = 100;
 let d = 200;
}
console.log(c);//200
console.log(d);//not defined

// 暂时性死区
var tmp = 132;
if(true){
  tmp = 'abc';//会报错，因为let暂时性死区,因为块级作用域中有用let关键字声明的变量tmp，该变量与块级作用域绑定，则变量不会再向上查询，
  这个tmp变量与该块级作用域外的tmp变量没有任何关系。
  let tmp;
}
```

#### let的小案例
```
🌰1:
var arr = [];
for(var i = 0;i<2;i++){
     arr[i] = function(){
        console.log(i)
     }
}

arr[0]();//输出为2
arr[1]();//输出为2
//分析：因为变量i是全局变量，所以函数执行时输出的都是全局作用域下的值。

🌰2:
var arr = [];
for(let i = 0;i<2;i++){
     arr[i] = function(){
        console.log(i)
     }
}

arr[0]();//输出为0
arr[1]();//输出为1
//分析：因为在每次循环都会产生一个块级作用域，每个块级作用域中的变量都是不同的，
函数执行时输出的是自己上一级(循环产生的块级作用域)作用域下的值。

```

### const关键字
- 作用：声明常量，常量就是值(内存地址)不能变化的量
- 具有块级作用域特性。
- 声明变量时必须必须赋初始值。
- 常量赋值后，值不能修改。
```
🌰1:
const PI = 3.14;
PI = 100;//报错 Assignment to constant variable; 

🌰2:
const ary = [100,200];
ary[0] = 'a';
ary[1] = 'b';
console.log(ary);//输出['a','b']，因为更改地址内部的值是被允许的
ary = ['a','b'];//报错,因为这样给ary赋值了一个新的数组改变了ary的存储地址。

```

### let、const、var的区别
1. 使用var声明的变量，其作用域为该语句所在的函数内，且存在变量提升现象。
2. 使用let声明的变量，其作用域为该语句所在的代码块内，即花括号内，不存在变量提升。
3. 使用const声明的是常量，在后面出现的代码中不能再修改该常量的值。

// 在编程过程中，若变量不需要变化，尽量使用const关键字(例如，函数的定义、PI值、数学公式中一些恒定不变的值，因为const的值不能实时变化，以及js中不用实时监控变量的变化，所以const的效率会比let高)





