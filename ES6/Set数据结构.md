# Set数据结构
- set类似于数组，但是它的成员的值都是唯一的，没有重复的值。
- set本身是一个构造函数，用来生成Set数据结构
```
🌰：
consts s1 = new Set();
//可初始化
consts s = new Set([1,2,3]);
console.log(s.size);//3
```

### 利用set数据结构做数组去重
```
🌰：
consts s = new Set([1,2,3,2,3,3]);
console.log(s.size);//3
var newS=[...s];
console.log(newS);//[1,2,3]
```

### Set的对象方法
- add(value), 添加某个值，返回Set结构本身
- delete(value), 删除某个值，返回一个布尔值，表示删除是否成功
- has(value), 返回一个布尔值，表示该值是否为Set成员
- clear(), 清除所有成员，没有返回值

### 遍历set
- set结构的实例与数组一样，也拥有forEach方法，用于对每个成员执行某种操作，没有返回值
```
🌰：
consts s = new Set([1,2,3]);
console.log(s.size);//3
s.forEach(value => console.log(value));
```
