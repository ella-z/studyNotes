# async与await
- async与await是语法糖
- async是Promise的语法糖。
- async函数默认返回的是一个Promise对象。
- await是then的语法糖。
- await必须放在async函数里面使用。
- await后面是一个promise的实例，如果不是也会默认转为promise的实例。
- await会直接执行promise实例的回调，并返回执行时的参数。
```
🌰：
//不使用语法糖
new Promise(resolve => {
  console.log("jack");
  resolve("jackxx");
}).then(v => console.log(v));

//使用语法糖
async function hd(){}
console.log(hd());//Promise{<resolved>:undefined}
---------------------------------------------------

async function hd(){
//return的内容是then的参数
  throw new Error('Sorry');
  return 'jack';
}
hd().then(v => console.log(v)).catch((e)=>{console.log(e)});
//then的参数是return的内容，catch会捕捉到抛出的错误。
---------------------------------------------------

async function hd(){
  return new Promise(resolve => {
    console.log('jack');
    reoslve('tom');
  });
}
hd().then(v => console.log(v));
//执行结果
jack
tom
----------------------------------------------------


//没有使用语法糖
new Promise(resolve => {
  resolve();
})
.then(v => return "jack")
.then(value => console.log(value));
//执行结果
jack
-------------------------------------

//使用语法糖
async function hd() {
  let name =await 'jack';
  console.log(name);
}
hd();
//执行结果
jack
--------------------------------------
async function hd() {
  let name =await new Promise(resolve => {
    setTimeout(()=>{
      resolve('jack');
    },1000)
  });
  let name1 =new Promise(resolve => {
    setTimeout(()=>{
      resolve('jack');
    },1000)
  });
  console.log(name);
  console.log(name1);
}
hd();
//执行结果
jack
Promise (<spending>);
```

### async与await执行异步请求
```
🌰：
//ajax封装函数
function ajax(url){
  return new Promise((resolve,reject) => {
       let xhr = new XMLHttpRequest();
       xhr.open('GET',url);
       xhr.send(); 
       xhr.onload = function(){
          if(this.status == 200){
               resolve(this.response);
          }else{
                reject("加载失败");
               }
         }
  })            
}

//async与await执行异步请求
async function get(name){
  let host = "http://localhost:8888/php";
  let user = await ajax(`${host}/user.php?name=${name}`);
  let lessons = await ajax(`${host}/lessons.php?id=${user.id}`);
  console.log(lessons);
}
get('jack');
```

### async延时函数
```
🌰：
async function sleep(delay = 2000){
  return new Promise(resolbe => {
    setTimeout(() =>{
      resolve();
    },delay)
  });
}

async funcion show(){
  for(const user of ['jack','tom']){
    await sleep();
    console.log(user);
  }
}
show();
```

#### async与await进度条小案例
```
🌰：
<style>
    #loading{
        height: 10vw;
        width: 0vw;
        background-color: skyblue;
        display: flex;
        justify-content: center;
        align-items: center;
        color: #fff;
    }
</style>
<body>
    <div id="loading">0%</div>
    <script src="ajax.js"></script>
    <script>
       // ajax("http://localhost:81/进度条/user.php?name=jack");
        //ajax("http://localhost:81/进度条/user.php?name=mike");

        function query(name){
            return ajax(`http://localhost:81/进度条/user.php?name=${name}`);
        }

        async function loadUser(users){
            for(let i=0;i<users.length;i++){
                await query(users[i]);
                let progress = (i+1)/users.length*100;
                loading.style.width = progress+"vw";
                //Math.round避免小数
                loading.innerHTML = Math.round(progress)+"%";
            }
        }
        loadUser(['mike','jack','bob']);
    </script>
</body>

//ajax封装函数
function ajax(url){
    return new Promise((resolve,reject) => {
        let xhr = new XMLHttpRequest();
        xhr.open('GET',url);
        xhr.send();
        xhr.onload = function(){
            if(this.status == 200){
                console.log(this.response);
                resolve(this.respnse);
            }else if(this.status == 404){
                reject(new HttpError('用户不存在'));
            }
            else{
                reject('加载失败');
            }
        }
    })
}

//user.php
<?php
header("content-Type: text/html; charset=utf-8");//字符编码设置 
header('Access-Control-Allow-Origin: *');//跨域
$name=$_GET['name'];
$nameArray=array('jack','mike','aisa');
$result=in_array($name,$nameArray);

if($result == true){
    echo $name;
}else{
    echo 'error';
}
?>
```

### class与await的结合
- 若一个类中包含一个then，那么这个类将被包装为一个Promise
```
🌰：
class User{
  constructor(name){
    this.name = name;
  }
  then(resolve,reject){
    let user = ajax(`http://localhost:81/进度条/user.php?name=${name}`);
    resolve(user);
  }
}

async function get(name){
  let user = await new User(name);
  console.log(user);//能获取到关于name的数据
}

get('jack');
```

### 异步封装在类内部
```
🌰：
class User{
  /*get(name){
    //ajax函数属于异步操作，而user.name += '-hello'属于同步操作，所以传递过去的name为jack-hello而不是jack。
    //可以使用async与await解决上述问题
    let user = ajax(`http://localhost:81/进度条/user.php?name=${name}`);
    user.name += '-hello'; 
    console.log(user);
    return user;
  }*/
  async get(name){
    let user = await ajax(`http://localhost:81/进度条/user.php?name=${name}`);
    //执行完await才会执行下面的代码;
    user.name += '-hello'; 
    console.log(user);
    return user;
  }
}

new User().get('jack').then(user => {
  console.log(user);
});
```

### async 与 await多种声明
```
🌰：
//普通函数
async function get(){
  return await ajax(`http://localhost:81/进度条/user.php?name=${name}`);
}
get('jack').then(value => {
  console.log(user);
});

// 对象
let hd = {
  async get(name){
    return await ajax(`http://localhost:81/进度条/user.php?name=${name}`);
  }
}
hd.get('jack');

//封装成类
class User{
  async get(name){
    let user = await ajax(`http://localhost:81/进度条/user.php?name=${name}`);
    //执行完await才会执行下面的代码;
    user.name += '-hello'; 
    console.log(user);
    return user;
  }
}

new User().get('jack').then(user => {
  console.log(user);
});
```

### async 与 await基本错误处理
```
🌰：
async function get(name){
  //console.log(a);//普通未定义错误
  //throw new Error('fail');// 抛出错误
  //return ajax(`http://localhost:81/进度条/user.php?name=${name}`);//异步错误
}
get('jack')
.then(value => {
  console.log(user);
})
.catch(error => {
  console.log('Error'+error);
});
```

### await的错误处理流程
```
🌰：
async function get(name){
  //console.log(a);//普通未定义错误
  //throw new Error('fail');// 抛出错误
  return ajax(`http://localhost:81/进度条/user.php?name=${name}`);//异步错误
  console.log(123);
}
get('jack')
.then(value => {
  console.log(user);
})
.catch(error => {
  console.log('Error'+error);
});
// 执行结果
//123没有被输出，因为在该代码中错误被catch之后，便不在会再执行下面的代码
-------------------------------------------------------------------------------


async function get(name){
  try{
     let user = await ajax(`http://localhost:81/进度条/user.php?name=${name}`);//异步错误
     let lessons = await ajax(`http://localhost:81/进度条/user.php?name=${user.id}`);
     console.log(lessons);
  }catch(error){
    console.log(error.message);
  }
  console.log(123);
}
get('jackk')
.then(value => {
  console.log(user);
})
// 执行结果
该用户不存在
123
```

### await并执行的技巧
```
🌰：
function p1(){
  return new Promise(resolve => {
    setTimeout(() => {
      resolve('p1')
    },2000);
  })
}
function p2(){
  return new Promise(resolve => {
    setTimeout(() => {
      resolve('p2')
    },2000);
  })
}

async function test(){
  let h1 = await p1();
  console.log(h1);
  let h2 = await p2();
  console.log(h2);
}
//执行结果
p1
过了两秒后
p2
//因为必须要等上一个await执行完成之后才能执行下一个await。两个await是异步的.
-------------------------------------------------------------------------------------

//想要两个promise同时执行,并行执行
//方法1：
async function test(){
//没有加await,所以p1,p2的Promise同时进行。
  let h1 =  p1();
  let h2 =  p2();
  let p1value = await h1;
  let p2value = await h2;
  console.log(p1value,p2value);//同时获得p1,p2的值
}

方法2：
async function test(){
 let res = await Promise.all([p1(),p2()]);
 console.log(res);
}
//执行结果
["p1","p2"]

```























