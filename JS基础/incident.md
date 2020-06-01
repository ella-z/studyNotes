# 事件
- onmousemove 该事件将会在鼠标在元素中移动时被触发。

### 事件对象
- 当事件的响应函数被触发时，浏览器每次都会将一个事件对象作为实参传递进响应函数。
   - 在事件对象中封装了当前事件相关的一切信息，比如，鼠标的坐标，键盘哪个按键被按下，鼠标滚轮滚动的方向。
      - altKey 返回当事件被触发时，"ALT"是否被按下。
      - button 返回当事件被触发时，哪个鼠标按钮被点击。
      - clientX 返回当事件被触发时，鼠标指针的水平坐标。
      - clientY 返回当事件被触发时，鼠标指针的垂直坐标。
      - shiftKey 返回当事件被触发时，"shift"键是否被按下。
      - target 返回触发事件的元素。
      - onmousedown 当鼠标被按下时
      - onmouseup 当鼠标松开时
      - onmousewheel 鼠标滚轮滚动事件
      - onkeydown 某个键盘按钮被按下。
      - onkeyup 某个键盘按钮被松开。
      - onkeypress 某个键盘按钮被按下并松开。

```
🌰：
<div class="father">
      <div class="kid"></div>
</div>

var father = document.getElementById("father");
var kid = document.getElementById("kid");

father.onmousemove = function(event){
/*在IE8及以下的浏览器中，是将事件对象作为window对象的属性保存的
   要使用 window.event;
*/   
  //解决兼容性问题
  event = event || window.event
  var X = event.clientX;
  var Y = event.clientY;
  kid.innerHTML="x="+X+",y="+Y;
}
  /*
  若想要给元素设置偏移量，一定要开启定位
  */
  documen..onmousemove = function(event){
  /* 获取滚动条的距离
      chrome认为浏览器的滚动条是body的，可以通过body.scrollTop来获取，而火狐等浏览器认为浏览器的滚动条是html的。
  */
  /*
    clientX和clientY用于获取鼠标在当面可见窗口的坐标
    div的偏移量是相对与整个页面的，包括overflow的页面
    pageX和pageY可以获取鼠标相对于当前页面的坐标 //不兼容IE8及以下浏览器
  */
  var left = event.pageX;
  var top = event.pageY;
  
  var st =document.body.scrollTop || document.documentElement.scrollTop;
  
  bj.style.left = left + "px";
  bj.style.top = top +st+ "px";
  
  }
```
### 事件冒泡
- 冒泡是指事件的向上传导，当后代元素上的事件被触发时，其祖先元素的相同事件也会被触发。
- 在开发中大部分情况冒泡都是有用的，如果不希望发生事件冒泡可以通过事件对象取消冒泡
```
🌰：
<div class="box1">
   <span class="span1"></span>
</div>


var span1 = document.getElementById('span1');
span1.onclick = function(event){
      //取消冒泡
      //可以将事件对象的cancelBubble设置为true，即可取消冒泡
      event.cancelBubble = true;
}

```

### 事件的委派
- 事件委派是指将事件统一绑定给元素的共同的祖先元素，这样当后代元素上的事件触发时，会一直冒泡到祖先元素，从而通过祖先元素的响应函数来处理事件。
- 事件委托是利用了冒泡，通过委派可以减少事件绑定的次数，提高程序的性能。
```
🌰：
<ul id="u1">
   <li class="click">click</li>
   <li class="click">click</li>
   <li class="click">click</li>
</ul>

var u1 = document.getElementById("u1");
u1.onclick = funtion(event){
   //如果触发事件的对象是我们期望的元素，则执行否则不执行
   if(event.target.className == "link"){
      alert("我是单击响应函数");
   }
}

```

### 事件的绑定
- 使用 对象.事件 = 函数的形式绑定响应函数，它只能同时为一个元素的事件绑定一个响应函数。
不能绑定多个，如果绑定了多个，则后面会覆盖掉前面

```
🌰：

<button class="btn01">点击我</button>

/*
   addEventListener()
   - 通过这个方法也可以为元素绑定响应函数。
   - 参数：
      1. 事件的字符串，不要on
      2. 回调函数，当事件触发时，该函数会被调用。
      3. 是否在捕获阶段触发事件，需要一个布尔值，一般都传false
   - 使用addEventListener()可以同时为一个元素的相同事件同时绑定多个响应函数，
   这样当事件被触发时，响应函数将会按照函数的绑定顺序执行。
   - 该方法不支持IE8及以下浏览器
*/
btn01.addEventListener("click",function(){
      console.log(1)
},false);

btn01.addEventListener("click",function(){
      console.log(2)
},false);

btn01.addEventListener("click",function(){
      console.log(3)
},false);
/*
   attachEvent()
   - 在IE5~IE10可以使用。
   - 参数：
      1. 事件的字符串，要on
      2. 回调函数
   - 这个方法也可以同时为一个事件绑定多个处理函数，不同的是后绑定先执行，执行顺序与addEventListener()相反。
*/

//解决兼容性问题
//定义一个函数，用来为指定元素绑定响应函数
/*
   addEventListener()的this，是绑定事件的对象。
   attachEvent()的this，是window
      需要统一两个方法的this
*/
/*
   参数：
      obj 要绑定事件的对象
      eventStr 事件的字符串
      callback 回调函数
*/
function bind(obj,eventStr,callback){
   if(obj.addEventListener){
      obj.addEventListener(eventStr,callback,false)
   }else{
      /*
         this是谁，是由调用方法决定的
         callback.call(obj)//改变调用函数的对象
      */
      obj.attachEvent("on"+eventStr,function(){
         callback.call(obj);//this变成obj了
      });
   }
}
bind(btn01,"click",funtion(){
   alert("click一下');
})
```

### 事件传播
- 事件传播分为3个阶段
   1. 捕获阶段
      - 在捕获阶段时从最外层的祖先元素，向目标元素进行事件的捕获，但默认此时不会触发事件
   2. 目标阶段
      - 事件捕获到目标元素，捕获结束开始在目标元素上触发事件
   3. 冒泡阶段
      - 事件从目标元素向它的祖先元素传递，依次触发祖先元素上的事件
   
   - 如果希望在捕获节点就触发事件时，可以将addEventListener()的第三个参数设置为true

### 拖拽
- setCapture()对事件进行捕获 //只有IE支持
```
🌰：
<style>
        #box1{
            width: 100px;
            height: 100px;
            background-color: pink;
            position: absolute;
        }
        #box2{
            width: 100px;
            height: 100px;
            background-color: skyblue;
            position: absolute;
            top:300px;
            left: 300px;
        }
</style>

<div id="box1"></div>
<div id="box2"></div>

<script>
        /*
        实现拖拽的步骤：
        1.onmousedown 当鼠标点下时，选定元素
        2.onmousemove 当鼠标移动时，元素跟随鼠标移动
        3.onmouseup 当鼠标松开时，元素定在当前位置
        */
        var box1 = document.getElementById('box1');
        box1.onmousedown = function(event){
            event = event || window.event;
            var ol = event.clientX - box1.offsetLeft;
            var ot = event.clientY - box1.offsetTop;

            //使用document，因为元素要在全局移动
            document.onmousemove =function(event){
                event = event || window.event; //兼容IE8
                var left = event.clientX - ol;
                var top = event.clientY - ot;
                box1.style.left = left+"px" ;
                box1.style.top = top+"px";
            }
        }
        //若使用box1，在box2中松开鼠标并不会触发box1松开的事件，因为box1与box2是兄第关系，并不会冒泡
        document.onmouseup = function(){
            document.onmousemove = null;
        }
        /*
        当我们拖拽一个网页中的内容时，浏览器会默认区搜索内容，
        此时会导致拖拽功能的异常，这个时浏览器提供的默认行为，
        如果不希望发生这个行为，则可以通过return false来取消默认行为
        */
</script>
```

### 滚轮事件
- onmousewheel //在火狐中不兼容
   - 火狐中DOMMouseScroll 来绑定事件，该事件需要通过addEventListener()函数来绑定
      - 使用addEventListener()函数来绑定响应函数，取消默认行为时不能使用return false，需要使用event.preventDefault()来取消默认，但是在IE8不能使用。
- event.wheelDelta //获取滚轮的方向，火狐浏览器不支持
   - 这个函数我们只看正负，不看数值，正为往上滚，负为往下滚。
- event.detail //在火狐中支持
   - 负为往上滚，正为往下滚。

```
🌰：
    <div id="box1"></div>
    <script>
        var box1 = document.getElementById('box1');
        box1.onmousewheel = function(event){
           if(event.wheelDelta>0 || event.detail < 0){
                box1.style.height = box1.clientHeight - 10 +"px";
           } else{
                box1.style.height = (box1.clientHeight+10) +"px";
           }
        }
    </script>
```

### 键盘事件
- onkeydown 某个键盘按钮被按下。
   - 若一直按着某个按键不松手，则事件会一直触发。
   - 当onkeydown连续触发时，第一次和第二次之间会间隔长一点，其他的会非常的快 //防止误操作
   - 若在onkeydown中取消了默认行为，则输入的内容，不会出现在文本框中。
- onkeyup 某个键盘按钮被松开。
   - 不会连续触发
- onkeypress 某个键盘按钮被按下并松开。
- 键盘事件一般都会绑定给一些可以获取到焦点的对象或者是document
- event对象
   - 通过keyCode来获取按键的编码，通过它可以判断哪个按键被按下
   - altKey
   - shiftKey
   - ctrlKey
      - 这三个用来判断alt、ctrl和shift是否被按下，如果按下，则返回true，否则返回false








