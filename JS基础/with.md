# with
### with的作用
- with语句的作用就是将代码的作用域设置到一个特定的对象中。
- 语法： with (expression) statement;

```
🌰：
var qs = location.search.substring(1);
var hostName = location.hostname;
var url = location.href;

👇 利用with进行简化

with(location){
  var qs = search.substring(1);
  var hostName = hostname;
  var url = href;
}
//在此过程中，使用with关联了location对象，意味着在with语句的代码块内部，每个变量首先被认为是局部变量，而如果在局部环境中，找不到该变量的定义，就会查询location对象中是否有同名属性。
如果发现了同名属性，则以location对象属性的值作为变量的值。
```

### with语句延长作用域
```
🌰：
var b={
  a:2
};
function sayA(){
  var a = 1;
  with(b){
    console.log(a);
  }
  console.log(a);
}
//输出结果：2，1

var b={
  a:2
};
function sayA(){
  var a = 1;
  with(b){
    a:5,
    c:7
  }
  console.log(c);
}

sayA();//7
console.log(b.c)//undefind
console.log(b.a)//5

//原因：
1. 在with语句中，只是改变了变量的遍历顺序(从with语句的对象开始)。当尝试在with语句中修改变量时，会搜索with语句的对象是否有该变量。有就改变对象的值，没有就创建，但是创建的以量依然属于
with语句块所在的执行环境，不属于with对象。离开with语句后，遍历顺序就会再次变成从执行环境开始。

```

### with的缺点
- 大量使用with语句会导致性能下降，同时也会给调试代码造成困难，因此在开发大型应用程序时，不建议使用with语句。
- 在严格模式下，不允许使用with语句，否则视为语法错误。
