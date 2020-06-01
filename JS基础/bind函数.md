# bind函数
- bind方法时复制的意思，参数可以在复制的时候传递进去，也可以在复制之后调用的时候传入进去。

```
🌰：
function f1(x,y){
  console.log(x+y);
} 

var ff = f1.bind(null);
ff(10,20);//30

var ff = f1.bind(10,20);
ff();//30
``` 
- apply和call是调用的时候改变this指向，bind方法是复制的时候改变this指向。

### bind应用
```
🌰：
function ShowRandom(){
  //产生1-10的随机数
  this.number = parseInterval(Math.random()*10+1);
}

ShowRandom.prototype.show1 = function(){
  //改变了定时器中的this指向了实例对象，本来指向window
  window.serInterbal(this.show2.bind(this),1000);
}

ShowRandom.prototype.show2 = function(){
    console.log(this.number);
}

var sr = new ShowRandom();
sr1.show1
```
 
