# Generator
- ES6新引入的数据类型。
- generator和函数不同的是，generator由funtion\*定义，可以返回多次，除了使用return语句返回，还可以使用yield返回多次。
- 调用generator对象有两个方法：
   1. 使用next()，每调用一次next()返回一个返回值。
   2. 直接使用for...of循环迭代generator对象
```
🌰：
        let array = [1, 2, 5, 8, 9, 6, 4, 5];
        function* x(array, max) {
            let n = 0;
            while (n < array.length) {
                if (array[n] > max) {
                    yield array[n];
                }
                n++;
            }
            return;
        }
        let result = x(array, 4);
        //way1：
        console.log(result.next());//5
        console.log(result.next());//8
        
        //way2：
         for(let x of result){
            console.log(x);  //直接输出所有返回值
        }

```

### Generator作用
- 可以将异步回调代码变成同步。
- 用对象的属性保存状态。
