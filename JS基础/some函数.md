# some函数
- some()方法用于检测数组中的元素是否满足指定条件。
- 它的返回值是布尔值，如果查找到这个元素，就返回true，如果查找不到就返回false。
- 如果查找到第一个满足条件的元素，则终止循环不再继续查找。
- 语法：array.some(function(currentValue,index,arr))
   - currentValue:数组当前项的值
   - index：数组当前项的索引
   - arr：数组对象本身

```
🌰：
var arr = [12,66,86,1];
var falg = arr.some(function(index,value){
     return value >= 20;
});
console.log(falg);//true
```

### some函数与forEach函数、filter函数的区别
- 若在some函数中return true，函数会终止循环，而forEach函数与filter函数遇到return true 不会终止遍历。
