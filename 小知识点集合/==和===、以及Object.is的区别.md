# ==和===、以及Object.is的区别
- == 表示相等，也就是值相等。
- === 表示恒等，也就是类型与值都要相等。
- Object.is 是ES6新增用来比较两个值是否严格相等，与 === 类似，但有些许不同。

### == 与 ===的区别
- js在比较的时候，== 会先进行类型转换再进行比较，=== 不会进行类型转换而是直接进行比较。

```
🌰：
console.log(0 == false); //true
console.log(0 === false); //false

console.log('1' == 1); //true
console.log('1' === 1); //true

console.log(undefined == undefined); //true
console.log(undefined === undefined); //true

//注意：无论是 == 还是 === ，NaN == NaN 都是false
console.log(NaN == NaN); //false
console.log(NaN === NaN); //false
```

### === 与 object.is的区别
- 在object.is中 NaN 与 NaN 相等，而在 === 中它们不相等。
- 在object.is中 -0 与 +0 不相等，而在 === 中它们相等。
```
🌰：

console.log(Object.is(NaN,NaN)); //true
console.log(NaN === NaN); //false

console.log(Object.is(+0,-0)); //false
console.log(+0 === -0); //true

```






