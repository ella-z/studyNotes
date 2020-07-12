# map函数
### map函数的特点
- 不会对空数组进行检测。
- 不会改变原始数组。

### map函数的语法
- array.map(callback, thisIndex)
   - callback为必须值，从当前的元素函数中产生新的数组元素。
   - thisIndex为选值，对象作为该执行回调时使用，传递给函数，用作"this"的值。
   
### 小栗子
```
🌰：
        let array = [1,2,5,8,4,6,2,4,8];

        let mapArray = array.map(item=>{
            return item*item;
        })

        console.log(mapArray); //[1, 4, 25, 64, 16, 36, 4, 16, 64]
```
