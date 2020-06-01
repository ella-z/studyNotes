# filter函数
- filter()方法会创建一个新的数组，新的数组中的元素是通过检查指定数组中符合条件的所有元素，主要用于筛选数据。
- 语法：array.filter(function(currentValue,index,arr))
   - currentValue:数组当前项的值
   - index：数组当前项的索引
   - arr：数组对象本身

```
🌰：
var arr = [12,66,86,1];
var newArr = arr.filter(function(index,value){
     return value >= 20;
});
console.log(newArr);//66,86
```
