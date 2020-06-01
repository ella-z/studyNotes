# object.defineProperty方法.
- 作用:定义新属性或修改原有的属性。
- 语法：object.defineProperty(obj,prop.descriptor)
   - obj:目标对象(必需)
   - prop:定义或修改的属性的名字(必需)
   - descriptor:目标属性所拥有的特性(必需)
      + value:设置属性的值，默认为undefined
      + writable:值是否可以重写。true|false 默认为false
      + enumerable:目标属性是否可以被枚举。true|false 默认为false
      + configurable:目标属性是否可以被删除或是否可以再次修改特性true|false 默认为false

```
🌰：
var obj={
  id:1,
  price:10
};
Object.defineProperty(obj,'num',{
  value:1000,
  writable:true
});
console.log(obj.num);//1000
obj.num = 50;
console.log(obj.num);//50

Object.defineProperty(obj,'id',{
  writable:false, //不可修改
  enumerable:true，
  configurable:true
});
obj.id = 2;
console.log(obj.id);//1

Object.defineProperty(obj,'address',{
  value:'china',
  writable:false,//不可修改
  enumerable:false //不可遍历，默认为false
});
console.log(Object.keys(obj));//["id"],没有address,id,num

Object.defineProperty(obj,'name',{
  value:'a',
  configurable:false,//不允许删除该属性或重新设置该属性的第三个特性，默认为false
  enumerable:false 
});
delete obj.name;
console.log(Object.keys(obj));//["id","name"]
delete obj.id;
console.log(Object.keys(obj));//["name"]


Object.defineProperty(obj,'name',{
  value:'b',
  configurable:false,                  //报错
  enumerable:true 
})
```
