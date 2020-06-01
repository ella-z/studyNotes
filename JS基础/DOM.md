# DOM
- DOM全称是Document Object Model文档对象模型
- DOM的作用：操作网页。
- 浏览器已经为我们提供文档节点、对象，而这个对象是window属性可以在页面中直接使用，文档节点代表的是整个网页，document。
- innerHTML用于获取元素内部的HTML代码的，对于自结束标签(例如:input)，这个属性没有意义。
- 如果需要读取元素节点属性，直接使用元素.属性名(除了class)，class要使用元素.className。
- 节点：Node-构成HTML文档最基本的单元
   - 节点的类别，有四类：
      - 文档节点：整个HTML文档
      - 元素节点：HTML文档中的HTML标签
      - 属性节点：元素的属性
      - 文本节点：HTML标签中的文本内容
   - 节点的属性
      - nodeName
      - nodeType
         - 文档节点：9
         - 元素节点：1
         - 属性节点：2
         - 文本节点：3
      - nodeValue
         - 文档节点：null
         - 元素节点：null
         - 属性节点：属性值
         - 文本节点：文本内容
- 获取元素节点，通过document对象调用。
   1. getElementById() 通过id属性获取一个元素节点对象。
   2. getElementsByTagName() 通过标签名获取一组元素节点对象，该方法会返回类数组对象。
   3. getElementsByName() 通过name属性获取一组元素节点对象，该方法会返回类数组对象。
- 获取元素节点的子节点，通过具体的元素节点调用
   1. getElementsByTagName(),方法，返回当前节点的指定标签名后代节点。
   2. childNodes, 属性，表示当前节点的所有子节点，会获取文本节点在内的所有节点。根据DOM标签标签间空白也会当成文本节点。**注意** 在IE8及以下浏览器中，不会将空白文本当成子节点。
   3. firstChild, 属性，表示当前节点的第一个子节点。
   4. lastChild, 属性，表示当前节点的最后一个子节点。
   5. children，属性，可以获取当前元素的所有子元素。
   6. firstElementChild 属性，获取当前元素的第一个子元素。//不建议使用，因为不支持IE8以下的浏览器
   7. lastElementChild 属性，获取当前元素的最后一个子元素。
 
   - body 属性，保存着body的引用。
   - documentElement 属性，保存着html。
   - all 属性，页面这种所有的元素 等同于 document.getElementsByTagName("*")
   - getElementsByClassName() 通过类名获取一组元素节点对象，不支持IE8以下的浏览器。
   - querySelector() 需要一个选择器的字符串作为参数，可以根据一个css选择器来查询你一个元素节点对象。
      - 该方法只会返回唯一元素，若有多个结果，只返回第一个。
   - querySelectorAll() 该方法与querySelector一致，但将多个结果封装在数组中返回。
  ```
  🌰：
  <style>
  .bj{
  width:500px;
  }
  </style>
  
  <ul id="city">
     <li class="bj">beijing</li>
     <li>newYork</li>
     <li>HongKong</li>
  </ul>
  
  var city = document.getElementById("city");//在全局查询
  var list = city.getElementsByTagName("li");//在city元素下查询
  
  //查询city的所有子节点
  var city = document.getElementById("city");//在全局查询
  console.log("city.childNodes.length");//7,因为空白(回车)也被算上了。
  console.log("city.children.length");//4
  
  document.querySelector(".bj").innerHTML;//背景
  ```

### DOM增删改
- appendChild() 把新的子节点添加到指定节点
   - 可以向一个父节点中添加一个新的子节点。
- removeChild() 删除子节点。
   - 语法，父节点.removeChild(子节点) | 子节点.parentNode.removeChild(子节点)
- replaceChild(new,old) 替换子节点。
- insertBefore(new,old) 在指定的子节点前面插入新的子节点。
- createAttribute() 创建属性节点。
- createElement() 创建元素节点。
   - 需要一个标签名作为参数，将会根据该标签名创建元素节点对象，并将创建好的对象作为返回值返回。
- createTextNode() 创建文本节点。
   - 需要一个文本内容作为参数，将会根据该内容创建文本系欸但。并将新的节点返回。
- getAttribute() 返回指定的属性值。
- setAttribute() 把指定属性设置或修改为指定的值。

```
🌰：
var li = document.createElement("li");
var gzText = document.createTextNode("广州");
li.appendChild(gzText);//<li>广州</li>

var city = document.getElementById("city");
var bj = document.getElementById("bj");

city.insertBefore(li,bj);
```

### DOM与CSS
- 通过元素修改样式
   - 语法：元素.style.样式名 = 值;
      - 若样式名中有-，需要将样式名改成驼峰命名法。
   - 通过style属性设置的样式都是内联样式，而内联样式有比较高的优先级1，所以通过JS修改的样式会立即显示。
- 读取样式
   - 语法：元素.style.样式名。
   - 通过style属性读取的样式是内联样式，无法读取样式表中的样式。
   ```
   🌰：
   var bj = document.getElementById("bj");
   bj.style.backgroundColor = "pink";
   
   bj.style.backgroundColor；//pink
   bj.style.width;//输出为空
   
   ```
- 获取元素样式
   - 获取元素当前显示的样式，若该元素的样式没有赋值，显示默认样式。
      - 语法：元素.currentStyle.样式名//只支持IE
      - 语法：getComputedStyle(),该方法是window方法，可以直接使用//不支持IE8及以下的浏览器
      // 这两个方法只能读不能改。
 ```
   🌰：
   var bj = document.getElementById("bj");
   //该方法返回一个对象，该对象中封装了当前元素对应的样式,通过样式名来读取样式，如果获取的样式没有设置，则会获取到真实的值，而不是默认值。例：若没有设置width，它不会获取auto，而是获得浏览器的长度。
   var obj = getComputedStyle(bj,null);
   obj.width;
 ```
 
 ### 样式的其他属性
 - 以下属性都不带单位，只带属性值，且这个属性只读不能改
 - clientHeight 返回元素的可见高度(包括了内外边距+内容区)
 - clientWidth  返回元素的可见宽度(包括了内外边距+内容区)
 
 - offsetHeight 返回元素的高度，能获取整个元素的大小(包括了内外边距+内容区+边框)
 - offsetWidth  返回元素的宽度，能获取整个元素的大小(包括了内外边距+内容区+边框)
 
 - offsetParent 可以获取当前元素的定位父元素(会获取到当前元素最近的开启了定位(position)的祖先元素)
 - offsetLeft   返回当前元素相对于其定位元素的水平偏移量
 - offsetTop    返回当前元素相对于其定位元素的垂直偏移量
 
 - scrollHeight 滚动区域的高度，若子元素比父元素大，滚动高度等于子元素的高度。 
 - scrollWidth 滚动区域的宽度，若子元素比父元素大，滚动高度等于子元素的宽度。
 - scrollLeft 获取水平滚动条滚动的距离。
 - scrollTop 获取垂直滚动条滚动的距离。
//当满足scrollHeigh-scrollTop == clientHeight，说明垂直滚动条到底了。
 
 
 
