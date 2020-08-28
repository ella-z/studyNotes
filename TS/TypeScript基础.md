# TypeScript基础
## 与Javascript相比TypeScript的优势是什么
- 在代码编写的时候就可以发现错误，js要在代码运行的时候才能发现错误。
- 在语言层面上，增加了类型约束，而且包括了一些语法的扩展，比如枚举类型(Enum)、元组类型（Tuple）等。
- 兼容性方面也无需担心，因为ts代码最终会被编译成js代码。
- 引入了静态类型声明。
- TypeScript具有更好的可维护性以及可读性。
- ......

## TypeScript基础知识
- [注意点](#注意点)
- [数据类型](#数据类型)
- [任意值](#任意值)
- [类型推论](#类型推论)
- [联合类型](#联合类型)
- [对象的类型--接口](#对象的类型--接口)
- [数组的类型](#数组的类型)
- [函数的类型](#函数的类型)

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
- 联合类型表示取值可以为多种类型，然后使用|分隔每个类型。
```
🌰：
let s:String|number;
s = "xxx";
s=7;
```
- 当TypeScript不确定一个联合类型的变量到底是哪个类型的时候，我们只能访问此联合类型的所有类型里共有的属性或方法。
```
🌰：
function getLength(something: string | number): number {
    return something.length; //报错：类型“string | number”上不存在属性“length”。类型“number”上不存在属性“length”。
}

function getString(something: string | number): string {
    return something.toString(); //可以访问共有属性以及方法
}

//当联合类型的变量被赋值的时候，会根据类型推论的规则推断出一个类型
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
console.log(myFavoriteNumber.length); // 5，因为此时以及被推断出是String类型了
myFavoriteNumber = 7;
console.log(myFavoriteNumber.length); // 编译时报错
```

### 对象的类型--接口
- TypeScript中的接口，除了可用于对类的一部分行为进行抽象以外，也常用于对对象的形状进行描述。
- 接口一般会首字母大写。
- 赋值的时候，变量的形状必须和接口的形状保持一致，变量中的属性必须要与接口的属性一致。
- 接口中有可选属性，使用了符号?，表示这个属性是可选属性。变量中定义的属性可以不包括它。
```
🌰：
interface Student{
    name:String;
    age?:number;
}

let stu1:Student={
    name:"xxx"
}
```
- 若要在接口中允许有任意的属性，可以使用[propName: string] 定义了任意属性取 string 类型的值。
```
🌰：
interface Person {
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    name: 'Tom',
    gender: 'male'
};
```
- 注意：一旦定义了任意属性，那么确定属性和可选属性的类型都必须是他的类型的子集。
```
🌰：
interface Person {
    name: string;
    age?: number;
    [propName: string]: string;
}

let tom: Person = {
    name: 'Tom',
    age: 18,
    gender: 'male'
};
//报错：不能将类型“{ name: string; age: number; gender: string; }”分配给类型“Person”。
  属性“age”与索引签名不兼容。
    不能将类型“number”分配给类型“string”。
    
👇修改

interface Person {
    name: string;
    age?: number;
    [propName: string]: string|number; //使用了联合类型
}

let tom: Person = {
    name: 'Tom',
    age: 18,
    gender: 'male'
};
```
- 若希望对象中的一些字段只能在创建的时候被赋值，那么可以用readonly定义只读属性。
   - 只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候。
```
🌰：
 readonly id: number;
```

### 数组的类型
- 数组可以使用[类型+方括号]来表示数组，定义完成后，数组的项中不允许出现其他的类型。
```
🌰：
let arr:number[] = [1,2,3,4,5,6,"7"]; //报错：不能将类型“string”分配给类型“number”。
```
- 数组还可以使用数组泛型来表示数组：
```
🌰：
let arr: Array<number> = [1, 1, 2, 3, 5，6];
```
- 用接口表示数组
```
🌰：
interface NumberArray {  //只要索引的类型是数字时，那么值的类型必须是数字。
    [index: number]: number;
}
let fibonacci: NumberArray = [1, 1, 2, 3, 5]; 
```

#### 类数组
- 类数组不是数组类型。
- 类数组通常使用接口来表示。
#### any在数组中的运用
```
🌰：
let list:any[] = [2,"x",{name:"xx"},[1,2,3]]
```

### 函数的类型
- 在typeScript中函数声明的方式：
```
🌰：
function sum(x: number, y: number): number {
    return x + y;
}
```
- 在 TypeScript 的类型定义中，=> 用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型。
```
🌰：
let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
    return x + y;
};
```
- 使用接口定义函数的形状
```
interface SearchFunc {
    (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
    return source.search(subString) !== -1;
}
```
- 可以使用?来表示可选参数，可选参数要在必须参数的后面。
- TypeScript 会将添加了默认值的参数识别为可选参数。
```
🌰：
function buildName(firstName: string = 'Tom', lastName: string) {
    return firstName + ' ' + lastName;
}
```
#### 剩余参数的使用
- 剩余参数只能是最后一个参数。
```
🌰：
function push(array: any[], ...items: any[]) {
    items.forEach(function(item) {
        array.push(item);
    });
}

let a = [];
push(a, 1, 2, 3);
```
#### 重载
-TypeScript 会优先从最前面的函数定义开始匹配匹配到同一类型的函数，所以多个函数定义如果有包含关系，需要优先把精确的定义写在前面。


- 参考：
   - [ts入门](https://ts.xcatliu.com/basics/primitive-data-types.html)
