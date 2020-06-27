# Event Loop
### Event Loop是什么
- Event loop是事件循环机制。

### 为什么要用Event Loop
- 因为JavaScript是一种单线程的语言，所有的任务都在一个线程上完成，所以任务在执行时采用的是排队这种方法，只有等前面的任务完成后，才能执行后面的任务。如果遇到大量任务或者遇到一个耗时的任务，在这个任务执行完成之前，后面的任务都无法执行，需要一直等到这个任务完成才能执行，Event Loop就是为了解决这个问题。

### Event Loop这种机制是如何运行的
- 将任务分为两种类型：
   - 同步任务
   - 异步任务
- 而同步任务与异步任务执行的时候，执行的方式会有区别。
   - 同步任务执行进入的是主线程，异步任务进入Event Table并且注册函数。
   - 当异步任务中指定的事情完成之后，Event Table会将这个函数移入Event Queue。
   - 当主线程的任务都执行完成之后，就会到Event Queue中读取对应的函数到主线程执行。
```
🌰1：
console.log(1);
setTimeout(()=>{
  console.log(2);
},1000);
console.log(3);

//输出的结果为
1
3
2
// 因为setTimeout是异步函数必须要等到同步任务完成之后才会执行
🌰2：
let i = 0;
setTimeout(()=>{
  console.log(++i);
},1000);
setTimeout(()=>{
  console.log(++i);
},1000);
//输出的结果为
1
2
//因为任务共享内存
```

### Promise与process.nextTick(callback)
- 对于任务更进一步定义可分为宏任务与微任务。
   - 宏任务：包括整体代码script，setTimeout，setInterval，DOM渲染(若有任务阻塞再DOM渲染之前，就会造成页面白页的情况)
   - 微任务：Promise，process.nextTick
- 而不同类型的任务会进入对应的Event Queue，例如setTimeout，setInterval会进入同一Event Queue。
- 事件循环执行的顺序为：整体代码script(宏任务) -> 微任务 -> 宏任务 -> 微任务。(每次执行完成一个宏观任务都需要查看是否由微观任务在Event Queue中，若有就要执行完成所有微观任务，再执行下一个宏观任务，以此循环)
```
🌰：
        console.log('1');

        setTimeout(function () {
            console.log('2');
            new Promise(function (resolve) {
                console.log('3');
                resolve();
            }).then(function () {
                console.log('4')
            })
        })

        new Promise(function (resolve) {
            console.log('5');
            resolve();
        }).then(function () {
            console.log('6')
        })
        
        setTimeout(function () {
            console.log('7');
            new Promise(function (resolve) {
                console.log('8');
                resolve();
            }).then(function () {
                console.log('9')
            })
        })
        
        //执行的结果是：1、5、6、2、3、4、7、8、9
```

- 参考：
   > [这一次，彻底弄懂 JavaScript 执行机制](https://juejin.im/post/59e85eebf265da430d571f89)
