# 实现Promise
- 手动实现一个简单的promise
```
🌰：
function Promise(fn) {
    let data = null;
    let reason = null;
    let state = "pending"
    succallbacks = [];
    failcallbacks = [];

    this.then = function (fulfilled, rejected) {
        if (state === "pending") {
            succallbacks.push(fulfilled);
            failcallbacks.push(rejected);
            return this; //实现链式
        } else if (state === "fulfilled") {
            fulfilled(data);
        } else if (state === "rejected") {
            rejected(reason);
        }

    }

    function resolve(value) {   //添加setTimeout达到延时的效果,这样能保证then在resolve以及reject之前执行，避免succallbacks为空
        setTimeout(() => {
            data = value;
            state = "fulfilled"
            succallbacks.forEach(callback => {
                callback(value);
            })
        }, 0)
    }
    function reject(value) {
        setTimeout(() => {
            data = reason;
            state = "rejected"
            failcallbacks.forEach(callback => {
                callback(value);
            })
        }, 0)
    }

    fn(resolve, reject);
}

function fn(num) {
    return new Promise((resolve, reject) => {
        resolve(num)
    })
}

fn(3).then(res => {
    console.log(res);
}).then(res => {
    console.log(res + 1)
});  //输出3，4
```
