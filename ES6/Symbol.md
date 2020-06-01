# Symbol
- Symbol是一个新的基本数据类型，而且是一个值的类型的
- Symbol跟字符串差不多，但是使用Symbol函数得到一个数据是完全不同的，每一个都是相互独立的。
   - Symbol可以接受一个参数，是对这个Symbol数据的描述，即时描述一样。但是，值也是不同的。
- Symbol的定义方法：
```
🌰1：
 //若用Symbol声明，则每次声明都产生新的Symbol。
 let sym1 = Symbol("a");
 let sym2 = Symbol("a");
 console.log(sym1==sym2);// false
 //利用属性来获取对这个Symbol的描述。
 sym1.description //a
 
 🌰2：
 //Symbol.for()方法与Symbol()方法不同的是该方法会在系统记录新增的Symbol，如再次用相同的参数创建Symbol，则系统会自动获取该Symbol。
 不会产生新的Symbol，该方法声明的Symbol具有全局特性。
 let cms1 = Symbol.for("hdcms");
 let cms2 = Symbol.for("hdcms");
 let cms3 = Symbol.for("hdcms2");
 console.log(cms);//Symbol(hdcms);
 console。log(cms1 == cms2)；//true
 console.log(cms1 == cms3);//false
 
  🌰3：
  //Symbol.keyFor()方法可以获取到Symbol方法的描述
  let cms1 = Symbol.for("hdcms");
  let cms2 = Symbol("hdcms1");
  console.log(Symbol.keyFor(cms1));//"hdcms"
  console.log(Symbol.keyFor(csm2));//undefined
```

### 使用Symbol解决字符串耦合问题
```
🌰：
let grade = {
  jack:{
    js:100,
    css:95
  },
  jack:{
    js:50,
    css:35
  }
}
console.log(grade);//只出现了最后一个jack，因为key关键字相同，所以前面了jack会被后面的jack给覆盖掉。

let user1 = 'jack';
let user2 = 'jack';
let grade = {
  user1:{
    js:100,
    css:95
  },
  user2:{
    js:50,
    css:35
  }
}
console.log(grade);//{user1:{...},user2:{...}},所以在对象中使用变量要使用中括号

let user1 = 'jack';
let user2 = 'jack';
let grade = {
[user1]:{
    js:100,
    css:95
  },
 [user2]:{
    js:50,
    css:35
  }
}
console.log(grade);//只出现了最后一个jack，因为key关键字相同，所以前面了jack会被后面的jack给覆盖掉。

let user1 = {
  name:"jack",
  key:Symbol()
};
let user2 = {
  name:"jack",
  key:Symbol()
};
let grade = {
[user1.key]:{
    js:100,
    css:95
  },
 [user2.key]:{
    js:50,
    css:35
  }
}
console.log(grade); //{Symbol(){js:100,css:95},Symbol(){js:50,css:35}}
//查看第一个jack的成绩
console.log(grade[user1.key]);//{js:100,css:95}

```

### Symbol在缓存容器中的使用

```
🌰：
class Cache{
  static data = {};
  static set(name,value){
    return (this.data[name] = value);
  }
  static get(name){
    return this.data[name];
  }
} 

let user={
  name:"a",
  desc:"用户"，
  key: Symbol("会员数据")
},
let cart = {
  name:"a",
  desc:"购物车",
  key:Symbol("购物车数据")
}
Cache.set(user.key,user);
Cache.set(cart.key,cart);
console.log(Cache.get(user.key));//{name:"a",desc:"用户"，key: Symbol("会员数据")}
console.log(Cache.get(cart.key));//{name:"a",desc:"购物车",key:Symbol("购物车数据")
}

```

### Symbol的扩展特性与对象保护

```
🌰1：
let symbol = Symbol("这是一个Symbol类型");
let hd = {
  name:"symbol",
  [symbol]:"aaaa"
};

for(const key in hd){
  console.log(key);//只能输出属性name
}
for(const key of Object.keys(hd)){
  console.log(key);//只能输出属性name
}
//输出Symbol类型的属性
for(const key of Object.getOwnPropertySymbols(hd)){
  console.log(key);//输出Symbol("这是一个Symbol类型")
}
//输出包括Symbol类型的所有属性
for(const key of Reflect.ownKeys(hd)){
  console.log(key); //name Symbol("这是一个Symbol类型")
}
}

🌰2：
let site = Symbol("这是一个Symbol");
class User{
  constructor(name){
    this.name = name;
    this[site] = "worker";
  }
  getName(){
    return `${this[site]}${this.name}`
  }
}
let jack = new User("jack");
console.log(jack.getName());//worker jack

//在forin循环中隐藏了Symbol类型的属性，给予了对象保护
for(const key in jack){
  console.log(key);//name
}

```




