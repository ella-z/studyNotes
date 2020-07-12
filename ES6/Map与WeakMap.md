# Map与WeakMap
### Map类型
- 在对象中的键只能为字符串，而在Map中任何值都可以作为键。
```
🌰：
//第一种创建方法：
let map = new Map();
map.set('name','jack');
map.set({},'rose');
map.set(1,'xxx');

//第二种创建方法：
let map = new Map([["name","jack"],["age":18]]);//在数组中第一个为键，第二个为值。
```

#### Map的增删改查
- set()添加键对。
- get()可以查询键值。
- delete()，clear()，clear是删除所有，使用delete删除会返回一个boolean值，如成功删除为true，删除失败为false。
- has()可以查询键是否存在，若存在返回true，若不存在则返回false。

#### 遍历Map类型数据
- keys()可以返回所有的键。
- values()可以返回值。
- entries()可以获得所有的键值对。
```
🌰：
let obj = new Map([["name","jack"],["age":18]]);

//循环得到所有的键值
//way:
for(const [key,value] of obj.entries()){
  console.log(key,value);
}

//way2:
obj.forEach((value,key)=>{
  console.log(value,key);
})
```

#### Map类型转换
```
🌰：
//转化为数组
let obj = new Map([["name","jack"],["age":18]]);
console.log([...obj]);

//单独将键转化
console.log([...obj.keys()]);

//单独将值转化
console.log([...obj.values()]);
```

#### Map类型管理DOM节点
```
🌰：
//获取所有的div元素，并且以dom为区分分别存储它的name。
let map =  new Map();
document.querySelectorAll("div").forEaach(item => {
  map.set(item,{
    content:item.getAttribute("name");
  });
});

```

### weakMap
- weakMap的键只能是对象。
- 当键丢失的时候，键所指向的数值也会被清除，不用手动删除。
- keys()，values()方法无法使用。
```
🌰：
let map = new weakMap();
map.set([],"xxxx");
```











