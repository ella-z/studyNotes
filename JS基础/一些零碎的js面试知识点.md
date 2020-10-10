# 一些零碎的js面试知识点
   1. !!运算符可以将右侧的值强制转换为布尔值。
   2. js的数据类型的转换只有三种情况：
      - 转换为布尔值，调用Boolean()方法。
      - 转换为数字，调用Number()、parseInt()和parseFloat()。
      - 转换为字符串，调用.toString()、String()（注意：null和undefined没有.toString()）
   3. JS中数据类型的判断（ typeof，instanceof，constructor，Object.prototype.toString.call())
      (1) typeof
         - 原始类型来说，除了null都可以正确显示。
         - 引用类型来说，除了函数能正确显示为function，其余的都为object。
      (2) instanceof
         - instanceof 运算符用来测试一个对象在其原型链中是否存在一个构造函数的 prototype 属性。其意思就是判断对象是否是某一数据类型（如Array）的实例。
         - instanceof可以精准判断引用数据类型（Array，Function，Object），而基本数据类型不能被instanceof精准判断。
         ```
            🌰：
            console.log(2 instanceof Number);                    // false
            console.log(true instanceof Boolean);                // false 
            console.log('str' instanceof String);                // false  
            console.log([] instanceof Array);                    // true
            console.log(function(){} instanceof Function);       // true
            console.log({} instanceof Object);                   // true    
            //因为这里的2，true，‘str’是字面量值，不是实例，所以为false。
         ```
      (3) constructor
         ```
            🌰：
            console.log((2).constructor === Number); // true
            console.log((true).constructor === Boolean); // true
            console.log(('str').constructor === String); // true
            console.log(([]).constructor === Array); // true
            console.log((function() {}).constructor === Function); // true
            console.log(({}).constructor === Object); // true
         ```
         - 缺点：如果创建一个对象，更改它的原型，constructor就会变得不可靠了
         ```
            🌰：
            function Fn(){};
            Fn.prototype=new Array();
            var f=new Fn();
            console.log(f.constructor===Fn);    // false
            console.log(f.constructor===Array); // true 
         ```
      (4) Object.prototype.toString.call() 借用Object的 toString 方法,几乎所有的数据类型都能成功显示。
         ```
           🌰：
           const o = Object.prototype.toString;
           console.log(o.call('a')); // [object String]  
         ```
   4. null和undefined的区别
      - null和undefined都是基本数据类型，这两个基本数据类型分别都只有一个值，就是undefined和null。
      - undefined代表未定义，null代表空对象(其实不是真的对象)。一般变量声明了但还没有定义的时候会返回 undefined，null 主要用于赋值给一些可能会返回对象的变量，作为初始化。
      - 使用双等 号对两种类型的值进行比较时会返回 true，使用三个等号时会返回 false。
    
   
   
### 参考
- [js知识点](https://mp.weixin.qq.com/s/MycjHhppKvspHWgI6qvXCA)
