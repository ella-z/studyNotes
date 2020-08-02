# TypeScript基础
- [注意点](# 注意点)
- [原始数据类型](# 原始数据类型)
- [任意值](# 任意值)
- [类型推论](# 类型推论)
- [联合类型](# 联合类型)
- [对象的类型--接口](# 对象的类型--接口)
- [数组的类型](# 数组的类型)
- [函数的类型](# 函数的类型)
- [类型断言](# 类型断言)
- [声明文件](# 声明文件)
- [内置对象](# 内置对象)

#### 注意点：
   - TypeScript只会在编译时对类型进行静态检查，如果发现有错误，编译的时候就会报错，而在运行的时候，不会对类型进行检查。
   - TypeScript编译的时候即使报错了，到那时还是会生成编译结果，我们仍然可以使用这个编译之后的文件。
   
### 数据类型
- 数据类型：Boolean、Number、String、Array、Function、Object、Symbol、undefined、null、void、any、never、元组、枚举、高级类型。


#### 布尔值
- 使用boolean定义布尔值类型：let isDone:boolean = false；
- 注意：
   - Boolean与boolean的区别：
      - boolean是基本数据类型，而Boolean是它的封装类，和其它类一样，有属性有方法，可以new。
      - Boolean是boolean的实例对象类。
   - 使用构造函数Boolean创建的对象不是布尔值，使用new Boolean()返回的是一个Boolean对象，但直接调用Boolean也可以返回一个boolean类型的对象。
   ```
   🌰：
   let createBoolean:boolean = new Boolean(1); //报错：不能将类型“Boolean”分配给类型“boolean”。“boolean”是基元，但“Boolean”是包装器对象。如可能首选使用“boolean”。
   
   //使用new Boolean()
   //ts：
   let createBoolean:Boolean =new Boolean(1);
   
   👇编译
   
   //js：
   var createBoolean = new Boolean(1);
   console.log(createBoolean);
   ```
#### 数值
- 使用number定义数值类型：let createNumber:number = 6;
```
🌰：
//ts：
let createNumber:number = 6;
console.log(createNumber);

👇编译

//js：
var createNumber = 6;
console.log(createNumber);
```

#### 字符串
- 定义：使用string定义字符串类型:let myName: string = 'Tom';
```
🌰：
//ts：
let createString:String = "xx";
console.log(createString);

👇编译

//js：
var createString = "xx";
console.log(createString);
```

#### 空值
- 在ts中可以将函数定义为空值void，即表示没有任何返回值的函数：
```
🌰：
function alertName(): void {
    alert('My name is Tom');
}
```
- 声明一个void类型的变量只能赋值为undefined和null

#### null以及undefined
- 可以使用Null以及undefined来定义：
```
🌰：
let u: undefined = undefined;
let n: null = null;
```
- null以及undefined和void的区别就是：
   - null以及undefined是所有类型的子类型，也就是说null以及undefined可以赋值给其他类型的变量，而void类型不能赋值给其他类型的变量。

### 任意值
- 任意值(any)用来表示允许赋值为任意类型。
- 如果是一个普通类型，在赋值过程中改变类型是不被允许的，而any类型是可以被赋值为任意类型。
```
🌰：
//普通类型：
let myName:string = "xxx";
myName = 7;   //报错：不能将类型“7”分配给类型“string”。

//any类型：
let myName:any = "xxx";
myName = 7;  //不报错
```
- 在任意值上访问任何属性都是允许的，也允许调用任何方法。
- 声明一个变量为任意值之后，对它的任何操作，返回的内容的类型都是任意值。
```
🌰：
let myName:any = "xxx";
console.log(myName.last); //输出undefined
console.log(myName.say()); //myName.say is not a function
```
- 对于未指定类型而且没有赋值的变量，会自动被识别为任意值类型。

### 类型推论
- 如果没有明确的指定类型，那么ts会依照类型推论的规则推断出一个类型。
```
🌰：
let str = "xxx"; //虽然没有明确指定类型，但是它的赋了一个字符串的值，所以ts会推断得它的类型是string类型
str = 7;//报错
```
- 如果定义的时候没有赋值，不管之后有没有赋值，都会被推断成any类型而完全不被类型检查。
```
🌰：
let str;
str="xxx";
str=7;
```

### 联合类型


### 对象的类型--接口


### 数组的类型


### 函数的类型


### 类型断言


### 声明文件


### 内置对象


- 参考：
   - [ts入门](https://ts.xcatliu.com/basics/primitive-data-types.html)
