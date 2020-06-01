# Promise
- promise是一个对象，对象和函数的区别就是对象可以保存状态，函数不可以（闭包除外）。
- 并未剥夺函数return的能力，因此无需层层传递callback，进行回调获取数据。
- promise的状态
   - pending，未完成
   - resolved，成功
   - rejected，失败
 - 当任务完成时，使用then方法处理状态,在then()函数中，第一个回调函数处理回调成功的状态，第二个回调函数处理回调失败的状态。
 - promise中有微任务队列，轮循微任务的队列，完成微任务之后，再将任务抛到主线程中。
 - 在微任务和宏任务中，以微任务为先。
   
 ```
 🌰：
  new Promise((resolve,reject) =>{
    resolve("成功"); //发出成功通知，将任务加入微任务序列中
  }).then(value => {
    console.log("成功的业务处理")
  },reason =>{
    console.log("业务处理失败");
  });
 
 ```
 
 ### 宏任务与微任务的执行顺序
 - 同步任务的优先级是最高的，其次是微任务，最后是宏任务。
  ```
 🌰：
 setTimeout(()=>{
  console.log("setTimeout");
 },0);
 console.log("x");
 //执行结果
 x
 setTimeout
 ---------------------------------------
 
 setTimeout(()=>{
  console.log("setTimeout");  //宏任务
 },0);
 
 new Promise(resolve => {
   resolve();
   console.log("promise");//同步任务
 }).then(value => {
    console.log("成功") //微任务
    })；   
 
 console.log('x');   //同步任务
  //执行结果
  promise
  x
 成功
 setTimeout
 ----------------------------------------
 
new Promise(resolve => {
  setTimeout(()=>{
    console.log("setTimeout");  //宏任务
    resolve();
  },0);
  console.log("promise");//同步任务
 }).then(value => {
        console.log("成功") //微任务
      })；   
 console.log('x');   //同步任务
  //执行结果
  promise
  x
  setTimeout
  成功
 ```
 
 ### Promise单一状态与状态中转
 - 只有改变promise状态的情况下才会执行微任务
 - 若resolve和reject传递的值为一个promise，那么该Peomise会根据值的promise状态调用微任务。
 - promise状态产生之后就会调用微任务，该状态是不可逆的，是不可改变的，不可撤销的。
 ```
 🌰：
 new Promise((resolve,reject) => {
   /*
   setTimeout(() => {
      console.log(resolve);  //延迟两秒再执行微任务
   },2000)
   */
   resolve("fulfilled");
   reject("拒绝");
 }).then(
   msg => {      
      console.og(msg);
   },
   error => {
      console.log(error);
   }
 );
 //只会执行resolve的微任务，不再执行reject的。
 
 --------------------------------
 
 //若resolve和reject传递的值为一个promise，那么该Peomise会根据值的promise状态调用微任务。
 let p1 = new Promise((resolve,reject) => {
   reject("拒绝");
 });
 
 new Promise((resolve,reject) => {
   resolve(p1);
 }).then(
   msg => {
      console.log('success'+msg);
   },
   error => {
      console.log('error'+error);
   }
 )
 //执行结果为 error拒绝
 
 ```
 
 ### Promise.then的基本语法
 - then方法中两个微任务，一个是接收成功状态后执行的，另一个是接收失败状态后执行。只关注成功或者失败，则写其中一个微任务即可，但因为传参的顺序，可在空的微任务中传递一个null值。
 - 若前一个then中没有对状态进行处理则会传递给下一个then进行处理
  ```
 🌰：
  new Promise((resolve,reject) => {
   resolve(p1);
 })
 .then()
 .then(resolve => {
   console.log(`成功${resolve}`);
 })
 .then(
   null,
   error => {
      console.log('error'+error);
   }
 )
 
 ```
 
 ### Promise.then也是一个Promise
 - 每一个then执行后返回的都是promise，若then不设置返回值，那么默认返回的是成功状态
 ```
 🌰：
 let p1 = new Promise((resolve,reject) => {
   resolve("success");
 });
 let p2 = p1.then(
   value => console.log(value);
   reason => console.log(reason);
 );
 
/* console.log(p1);//Promise<resolved>;"success"}
 console.log(p2);//Promise<pending>,因为在此时微任务还没执行，还在一个准备阶段*/
 
 setTimeout(() =>{
   console.log(p1);//Promise<resolved>;"fulfilled"}
   console.log(p2);//Promise<resolved>:undefined,因为微任务早执行于宏任务。
 },2000)
 
 -----------------------------
 
 let p1 = new Promise((resolve,reject) => {
   resolve("success");
 });
 let p2 = p1.then(
   value => console.log(value);
   reason => console.log(reason);
 ).then(a=>console.log("成功"),b=>console.log("失败"));

 //执行结果
 success
 成功
 
 let p1 = new Promise((resolve,reject) => {
   reject("reject");
 });
 let p2 = p1.then(
   value => console.log(value);
   reason => console.log(reason);
 ).then(a=>console.log("成功"),b=>console.log("失败"));
 
 //执行结果
 reject
 成功//因为上一个then默认返回的是成功 
 ```
 
 ### then返回值的处理
 ```
 🌰：
 let p1 = new Promise((resolve,reject) => {
   resolve("success");
 });
 let p2 = p1.then(
   value => {
      return value;
   },
   reason => console.log(reason);
 ).then(a=>console.log(a),b=>console.log(b));
 
 //执行结果
success

--------------------------------------------
🌰1：
 let p1 = new Promise((resolve,reject) => {
   resolve("success");
 });
 let p2 = p1.then(
   value => {
      return new Promise((resolve,reject) => {
         resolve("success");
      });
   },
   reason => console.log(reason);
 )
 .then(a=>console.log(a),b=>console.log(b));

 //执行结果
 success
 
 🌰2：
 let p1 = new Promise((resolve,reject) => {
   reject("reject");
 });
 let p2 = p1.then(
   value => {
      return new Promise((resolve,reject) => {
         resolve("success");
      });
   },
   reason => console.log(reason);
 )
 .then(a=>console.log(a),b=>console.log(b));

 //执行结果
 reject
 
 🌰3：
 let p1 = new Promise((resolve,reject) => {
   resolve("success");
 });
 let p2 = p1.then(
   value => {
      return new Promise((resolve,reject) => {
         setTimeout(() => {
            reject("reject");
         },2000)
      }).then(null,reason => {
         return new Promise(resolve,reject) => {
            reject(reason);
         });
      });
   },
   reason => console.log(reason);
 )
 .then(a=>console.log(a),b=>console.log(b));

 //执行结果
 reject
 
 ```
 
 ### 其他类型的Promise封装
 ```
 🌰：
 //常规版本
  let p1 = new Promise((resolve,reject) => {
   resolve("success");
 }).then(
   value => {
      return new Promise((resolve,reject) => {
            resolve("success");
      )
 .then(value => {
   console.log(value);
 });
 //执行结果
 success
 
 --------------------------------------
 //then对象版本
  let p1 = new Promise((resolve,reject) => {
   resolve("success");
 }).then(
   value => {
      return then(resolve,reject) => {
            resolve("success");           //功能与常规版本无差别
         }
 )
 .then(value => {
   console.log(value);
 });
 
 ---------------------------------------
 //类对象版本
 let p1 = new Promise((resolve,reject) => {
   resolve("success");
 }).then(
   value => {
   class Hd {
         then(resolve,reject) => {
            resolve("success");           //功能与常规版本无差别
         }
   }
   return new Hd(); 
 )
 .then(value => {
   console.log(value);
 });
 
 ----------------------------------------
 //类静态函数版本
  let p1 = new Promise((resolve,reject) => {
   resolve("success");
 }).then(
   value => {
   return class {
      static then(resolve,reject){
            resolve("success");           //功能与常规版本无差别
         }
   }
 )
 .then(value => {
   console.log(value);
 });
 
 ```
 
 ### 使用promise封装ajax异步请求
 - 使用promise封装ajax异步请求避免了使用回调函数。
 ```
 🌰：
 //封装前ajax
 function ajax(url,callback){
   let xhr = new XMLHttpRequest();
   xhr.open("GET",url);
   xhr.send();
   xhr.onlodad = function(){
      if(this.status == 200){
         callback(JSON.parse(this.response));
      }else{
         throw new Error("加载失败");
      }
   }
 }
 
//使用promise封装后
function ajax(url){
  return new Promise((resolve,reject) => {
      let xhr = new XMLHttpRequest();
      xhr.open("GET",url);
      xhr.send();
      xhr.onlodad = function(){
         if(this.status == 200){
            resolve(JSON.parse(this.response));
         }else{
            reject("加载失败");
         }
      })
    }
 }
 
 // 使用方法
 <script>
  var url = "http://localhost:8888/user.php"
  ajax(`${url}?name=jack`).then(user=>{
    return ajax(`${url}?id=${user.id}`)
  },
  reason=>{
   console.log(reason)
  }).then(value => {
   console.log(value);
  });
 </script>
 
 ```
### Promise多种错误监测与catch使用
- 若Promise中无对reject状态的回调函数，则错误会被catch捕捉到并执行catch里面的代码段。若Promise中有对reject状态的回调函数，则执行对reject状态的回调函数，catch函数不会捕捉到错误。
```
🌰：
 new Promise((resolve,reject) => {
   //resolve('成功');
   //reject(new Error("fail"));//way1
   //throw new Error("fail");//way2
   //x+1 //报错，因为x没有被定义。错误会自动抛出，相当于将该代码段写在try-catch中
 })
 .then(resolve => {
    return new Promise((resolve,reject) => {
      reject("reject");
    }).then(resolved => {
      console.log(resolved);
    });
 })
 .catch(error){
   console.log(error); //无论是第一个Promise,还是第二个Promise，抛出错误，都会被catch捕捉到。
 };
```
 
 ### 自定义错误
```
🌰：
class ParamError extend Error{
   construtor(msg){
      super(msg);
      this.name = 'ParamError';
   }
}
function ajax(url){
  return new Promise((resolve,reject) => {
      if(!/^http/.test(url)){
         throw new ParamError("请求地址错误");
      }
      let xhr = new XMLHttpRequest();
      xhr.open("GET",url);
      xhr.send();
      xhr.onload = function(){ //不能在onload中throw错误，可以使用reject来代替，因为onload是异步的。
         if(this.status == 200){
            resolve(JSON.parse(this.response));
         }else{
            reject("加载失败");
         }
      })
    }
 }
 
 // 使用方法
 <script>
  var url = "ttp://localhost:8888/user.php" //假如http不完整，程序报错，我们由此来自定义错误
  ajax(`${url}?name=jack`).then(user=>{
    return ajax(`${url}?id=${user.id}`)
  })
  .then(value => {
   console.log(value);
  })
  .catch(error){
   console.log(error.message);
  };
 </script>
 
 //执行结果
 ParamError:请求地址格式错误
 
```


### finally关键字
- 无论我们是成功还是失败，finally内的代码都会执行
```
🌰：
const promise = new Promise((resolve,reject) => {
   resolve("success");
})
.then(msg => {
   console.log(msg);
})
.catch(error => {
   console.log(error.message)
})
.finally(() => {
   console.log("始终会执行");
})
```

- 使用finally实现动画
```
🌰：
//加载动画
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>加载动画效果</title>
    <style>
        div{
            width: 100px;
            height: 100px;
            background-color: skyblue;
            display: none;
        }
    </style>
</head>
<body>
    <div id='loading'>loading~~~</div>
    <script>
        function ajax(url){
            return new Promise((resolve,reject) => {
                loading.style.display = "block";   //在加载过程中，始终显示
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
        ajax('test.php').then(value => {
            console.log(value);
        })
        .catch(error => {
            console.log(error);
        })
        .finally(() => {
            loading.style.display = "none"; //加载完毕，无论成功还是失败都隐藏。
        })
    </script>
</body>
</html>

```

- Promise异步加载图片
```
🌰：
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>异步加载图片</title>
</head>
<body>
    <script>
        function promise(src){
            return new Promise((resolve,reject) => {
                let image = new Image();
                image.src = src;
                image.onload = ()=>{
                     resolve(image);
                };
               image.onerror = reject;
               document.body.appendChild(image);
            })
        }
        promise('h.PNG').then(value => {
            value.style.border = '10px solid black';
        })
        .catch(error => {
            console.log(error);
        });
    </script>
</body>
</html>
```

### 封装setTimeout
```
🌰：
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>封装setTimeout</title>
</head>
<body>
    <script>
        function settimeout(delay){
            return new Promise(resolve => {
                setTimeout(resolve,delay);
            })
        }
        settimeout(2000).then(()=>{
            console.log("2秒后");
            return settimeout(5000);
        }).then(()=>{
            console.log("又过了5秒");
        })
    </script>
</body>
</html>
```

### 构建扁平化的setInterval
- Promise不等于不使用回调函数，但是Promise可以避免回调嵌套的问题。
 ```
 🌰：
 //使用普通回调函数，实现异步计时器
 function interval(callback,delay){
           let id =setInterval(()=>{
                callback(id);
           },delay);
        }
        interval((id) => {
            const div = document.querySelector("div");
            let left = parseInt(window.getComputedStyle(div).left);
            div.style.left = left + 10 + "px";
            if( left >= 500){
                clearInterval(id);                                           //函数嵌套
                //清除定时器，运动停止
                interval(timeId => {
                    let width = parseInt(window.getComputedStyle(div).width);
                    div.style.width = width - 10+"px";
                    if(width <= 20){
                        clearInterval(timeId);
                    }
                },1000)
            }
        },10);
  
 //使用Promise封装的异步计时器
 function interval(delay,callback){
            return new Promise(resolve => {
                let id =setInterval(()=>{callback(id,resolve),delay});
            });
        }
        interval(10,(id,resolve)=>{
                const div = document.querySelector("div");
                let left = parseInt(window.getComputedStyle(div).left);
                div.style.left = left + 10 + "px";
                if( left >= 500){
                    clearInterval(id); 
                    resolve(div);
                }  
        })
        .then(value => {
            return interval(100,(timeId,resolve)=>{
                let width = parseInt(window.getComputedStyle(value).width);    //没有回调嵌套，一个then写一个回调
                value.style.width = width - 10+"px";
                if(width <= 20){
                    clearInterval(timeId);
                    resolve(value);
                }
            });
        })
        .then(value => {
            value.style.backgroundColor = 'pink';
        });
 ```

### Promise.resolve缓存后台数据
- Promise.resolve为默认成功状态。
```
 🌰：
 //当同一个页面多次请求后台同一数据的时候，可以将该数据暂时缓存。
 function query(name){
   //创造缓存的属性
   const cache = query.cache || (query.cache = new Map());
   if(cache.has(name)){
      console.log('已缓存过了');
      //提取缓存的内容
      return Promise.resolve(cache.get(name));
   }
   //ajaxF为ajax请求后台的封装函数
   return ajaxF(`xxxxx.php?name=${name}`).then(user => {
      console.log('还未缓存过');
      cache.set(name,user);
      return user;
   })
 }
 query('jack').then(user => {
   console.log(user);
 });
 //每隔一秒请求同一数据
 setTimeout(()=>{
   query('jack').then(user => {
      console.log(user);
   });
 },1000);
 
 ------------------------------------------------------------------
 //Promise.resolve函数解刨
 Promise.resolved = function(value) {
   return new Promise(resolve => {
      resolve(value);
   });
 };
 Promise.resolved('jack').then(value => {
   console.log(value);
 });
 //以上代码相当于Promise.resolve('jack');
```

### Promise.reject使用
```
🌰：
new Promise((resolve,reject) => {
   resolve('xx')
}).then(value => {
   if(value !== 'success'){
      //throw new Error('fail');
      return Promise.reject('参数错误'); //直接使用Promise.reject抛出任务
   }
})
.catch(error => {
   console.log(error + 'fail');
});
```

### Promise.all批量获取数据
- 若all中所有的Promise状态都得为已解决状态，若有其中一是失败的，那么整个all都是失败的，all中可以用catch来捕捉错误。
```
🌰：
const a1 = new Promise((resolve,reject) => {
   setTimeout(() => {
      //resolve('success1');
      reject('fail');
   },1000)
}).catch(error => {
   console.log(error);
}); //a1的catch已经捕捉到了a1抛出的错误，所以在all中的catch不再捕捉错误，而且catch会返回一个已解决状态的promise。
const a2 = new Promise((resolve,reject) => {
   setTimeout(() => {
      resolve('success2');
   },1000)
});
Promise.all([a1,a2]).then(result => {
   console.log(result);
})
.catch(error => {
   console.log(error);
})
-------------------------------------------

//批量获取数据
const promises = ['jack','tom'].map(name => {
   //ajaxF为ajax请求后台的封装函数
   return ajaxF(`xxxxx.php?name=${name}`);
});
Promise.all(promises).then(users => {
   console.log(users);
});
```

### Promise.allSettled使用
- allSettled中的元素可以为未解决状态。
- 若allSettled中的元素未不存在元素，那么还是会有该元素的结果输出，但status状态为空。
```
🌰:
const p1 = new Promise((resolve,reject) =>{
   reject('rejected');
});

const p2 = new Promise((resolve,reject) =>{
   resolve('resolved');
});

Promise.allSettled([p1,p2]).then(result => {
  // console.log(result);
  let users = result.filter(user => {
   return user.status == 'fulfilled';
  });
  console.log(users);//只输出了完成状态的
});
//执行结果
两个peomise的结果都会被输出，但是status不一样。
```
 
### Promise.race
- 可传入多个Promise，但只取一个Promise运行，选择最快的那个promise执行。
``
🌰:
```let promises = [
 //模拟请求后台数据
 new Promise((resolve,reject) => {
   setTimeout(() => {
      resolve('请求成功');
   },1000);
 }),
 new Promise((resolve,reject) => {
   setTimeout(() => {
      reject('请求失败');
   }，2000);
 })
 ]
 Promise.race(promises).then(value => {
   console.log(value);
 })
 .catch(error => {
   console.log(error);
 })
 //若resolve时长长于reject，那么只会执行reject的promise，抛出请求失败的错误。若短于，则执行resolve的promise
 ```
 
### Promise队列原理
- Promise队列，按顺序执行我们的Promise，下一个promise要等上一个promise执行完之后才能执行。
```
🌰:
let promise = Promise.resolve('jack');
promise = promise.then(value => {
   return new Promise(resolve => {
      setTimeout(() => {
         console.log(1);
         resolve();
      },1000)
   })
})
.then(value => {
   return new Promise(resolve => {
         console.log(2);
         resolve();
   })
});
//执行结果
1
2
```

### 使用map实现Promise队列
```
🌰：
//小栗子：
function queue(num){
   let peomise = Promise.resolve();
   num.map(v => {
      promise = promise.then(value => {  //promise.then中的promise是上一个promise返回的Promise
         return new Promise(resolve => {
            setTimeout(() => {
               console.log(v);
               resolve();
            },1000)
         ]);
      });
   });
}
queue([1,2,3,4,5]);//执行结果为输出1，2，3，4，5
----------------------------------------------------------------
//封装队列函数
functoin queue(num){
   let promise = Promise.resolve();
   num.map(v => {
      promise = promise.then(value => {
         renturn v();
      });
   });
}
function p1(){
   return new Promise(resolve => {
      setTimeout(() => {
         console.log('p1');
         resolve();
      });
   });
}
function p2(){
   return new Promise(resolve => {
      setTimeout(() => {
         console.log('p2');
         resolve();
      });
   });
}

queue([p1,p2]);

```

### reduce封装Promise队列
```
🌰：
function queue(num){
   num.reduce((promise,n) => {
      return promise.then(v => {
         return new Promise(resolve => {
            console.log(n);
            resolve();
         });
      });
   },Promise.resolve());
}

```

### 使用队列渲染数据
- 使用队列的形式从后台获取数据，并将数据渲染到页面中。
```
🌰：
class User{
   ajax(user){
      let url = `http://localhost:8888/user.php`;
      return new Promise((resolve,reject) => {
         let xhr = new XMLHttpRequest();
         xhr.open("GET",`${url}?name=${user}`);
         xhr.send();
         xhr.onload = function(){
            if(this.sattus == 200){
               resolve(JSON.parse(this.response));
            }else.if(this.status == 404){
               reject(new HttpError("用户不存在"));
            }else{
               reject('加载失败');
            }
         };
         xhr.onerror = function(){
            reject(this);
         };
      });
   }
   //使用队列，对数组里面的函数逐一求解
   render(users){
      users.reduce((promise,user) => {
         return promise
         .then(value => {
            return this.ajax(user);
         })
         .then(user => {
            this.view(user);
         });
      },Promise.resolve());
   }
   //显示在页面函数
   view(){}
} 

new User().render(['jack','tom']);
```




 
