# 实现Promise.all()
- Promise.all()接收一个数组为参数，当数组的所有Promise都为resolve的状态的时候，Promise.all()才会成功，若有一个失败，都会被认为是失败的。
```
🌰：
//都为resolve状态的时候
let promise1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve(1);
    }, 1000);
})
let promise2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve(2);
    }, 1000);
})
let promise3 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve(3);
    }, 1000);
})
Promise.all([promise1,promise2,promise3]).then(res=>{
    console.log(res); //[1, 2, 3]
})

//其中一个为reject时
let promise1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve(1);
    }, 1000);
})
let promise2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve(2);
    }, 1000);
})
let promise3 = new Promise((resolve, reject) => {
    setTimeout(() => {
       throw Error('x');
    }, 1000);
})
Promise.all([promise1,promise2,promise3]).then(res=>{
    console.log(res); 
})
//执行报错：Uncaught Error: x at 实现Promise.all.js:15
```

### 手动实现Promise.all
```
🌰：
let promise1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve(1);
    }, 1000);
})

let promise2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve(2);
    }, 1000);
})

let promise3 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve(3);
    }, 1000);
})

function isPromise(obj) {
    return !!obj && (typeof obj === 'object' || typeof obj === 'function') && typeof obj.then === 'function';  
}

const myPromiseAll = (arr)=>{
    let result = [];
    return new Promise((resolve,reject)=>{
        for(let i=0;i<arr.length;i++){
            if(isPromise(arr[i])){
                arr[i].then(res=>{
                    result.push(res);
                    if(result.length===arr.length){
                        resolve(result);
                    }
                },reject)
            }else{
                result[i] = arr[i];
            }
        }   
    })
}

myPromiseAll([promise1,promise2,promise3]).then(res=>{
    console.log(res); //[1,2,3]
})
```







