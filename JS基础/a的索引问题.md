# a的索引问题
```
🌰：
for(var i=0;i<allA.length;i++){
/*
for循环会在页面加载完成之后立即执行，而响应函数会在超链接被点击时才执行。
当响应函数执行时，for循环早已执行完毕。
*/
allA[i].onclick = function(){
alert(i);//只会为allA.length
}
}
```
