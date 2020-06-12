# BFC与IFC
## BFC
- BFC又称为块格式化上下文。
- BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。

### BFC布局规则特性
- 内部的Box会在垂直方向，从顶部开始一个接一个地放置。
- Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生叠加
- 每个元素的margin box的左边，与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
- BFC的区域不会与float box叠加。
- BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之亦然。
- 计算BFC的高度时，浮动元素也参与计算。

### BFC创建条件
- 根元素或其它包含它的元素。
- 浮动 (元素的 float 不是 none)。
- 绝对定位的元素 (元素具有 position 为 absolute 或 fixed)。
- 内联块 inline-blocks (元素具有 display: inline-block)。
- 表格单元格 (元素具有 display: table-cell，HTML表格单元格默认属性)。
- 表格标题 (元素具有 display: table-caption, HTML表格标题默认属性)。
- 块元素具有overflow ，且值不是 visible。
- display:flow-root。

### BFC的作用
1. 解决margin重叠问题
- 同一个BFC下的两个相邻的盒子会出现垂直margin重叠的问题。
```
🌰：
// 在html中
<body>
    <div class="out">
        <div class="in"></div>
        <div class="in"></div>
        <div class="in"></div>
    </div>
</body>
// 在css中 
.out {
   width: 500px;
   min-height: 20px;
   border: 5px solid skyblue;
   }
.in {
   margin: 10px 0;
   width: 100%;
   height: 100px;
   background-color: pink;
   }
```
<br />
<img src="" alt="BFC的margin问题" width="100px" height="200px">


- 解决方法：可以为其中一个盒子添加一个父元素，并使其触发BFC解决这个问题。
```
🌰：
// 在html中
<body>
    <div class="out">
        <div class="bfc">
            <div class="in"></div>
        </div>
        <div class="in"></div>
        <div class="in"></div>
    </div>
</body>
// 在css中 
.out {
   width: 500px;
   min-height: 20px;
   border: 5px solid skyblue;
 }
.in {
   margin: 10px 0;
   width: 100%;
   height: 100px;
   background-color: pink;
 }
   .bfc{
   verflow: auto;
 }
```
<br />
<img src="" alt="BFC的margin解决方法" width="100px" height="200px">


2. 浮动带来的布局问题
- 在同一个BFC下，若其中一个元素有浮动，浮动的元素会与盒子的边缘接触，而BFC下元素的最左边边缘总是会与包含它的盒子左边相接触，那么就会出现浮动元素遮盖了其他元素的情况。
```
🌰：
// 在html中
<body>
    <div class="black"></div>
    <div class="skyblue"></div>
</body>
// 在css中 
body{
    position: relative;
}
.black {
    width: 100px;
    height: 100px;
    background-color: black;
    float: left;
}
.skyblue {
    width: 200px;
    height: 200px;
    background-color: skyblue;           
}
```
<br />
<img src="" alt="BFC的float布局问题" width="100px" height="100px">

- 解决方法：因为BFC的区域不会与float盒子重叠，所以可以给被覆盖的元素添加BFC，就可以避免被覆盖了。
```
🌰：
// 在html中
<body>
    <div class="black"></div>
    <div class="skyblue"></div>
</body>
// 在css中 
body{
    position: relative;
}
.black {
    width: 100px;
    height: 100px;
    background-color: black;
    float: left;
}
.skyblue {
    width: 200px;
    height: 200px;
    background-color: skyblue;
    overflow: auto;
}
```
<br />
<img src="" alt="BFC的float布局问题的解决方法" width="100px" height="100px">

3. 清除浮动
- 浮动导致的高度塌陷问题
```
🌰：
//在html中
<div class="out">
    <div class="in"></div>
</div>
//在css中
.out{
    width: 300px;
    min-height: 20px;
    border:5px solid skyblue;
}
.in{
    float: left;
    width: 200px;
    height: 200px;
    background-color: pink;
}

``` 
<br />
<img src="" alt="浮动导致的高度塌陷问题" width="100px" height="100px">

- 解决方法：因为计算BFC的高度时，浮动元素也参与计算，所以给父元素触发BFC即可避免这个问题。
```
🌰：
//在html中
<div class="out">
    <div class="in"></div>
</div>
//在css中
.out{
    width: 300px;
    min-height: 20px;
    border:5px solid skyblue;
    overflow:auto;
}
.in{
    float: left;
    width: 200px;
    height: 200px;
    background-color: pink;
}

``` 
<br />
<img src="" alt="浮动导致的高度塌陷的解决方法" width="100px" height="100px">

[参考](https://www.jianshu.com/p/0fb2f90418c3)

## IFC
- IFC又称为内联格式化上下文。

### IFC布局规则特性
- 子元素水平方向横向排列，并且垂直方向起点为元素顶部。
- 子元素只会计算横向样式空间，垂直方向样式空间不会被计算（padding、border、margin）。
- 在垂直方向上，子元素会以不同形式来对齐（vertical-align）
- 能把在一行上的框都完全包含进去的一个矩形区域，被称为该行的行框（line box）。行框的宽度是由包含块（containing box）和与其中的浮动来决定。
- IFC中的“line box”一般左右边贴紧其包含块，但float元素会优先排列。
- IFC中的“line box”高度由 CSS 行高计算规则来确定，同个IFC下的多个line box高度可能会不同。
- 当 inline-level boxes的总宽度少于包含它们的line box时，其水平渲染规则由 text-align 属性值来决定。
- 当一个“inline box”超过父元素的宽度时，它会被分割成多个boxes，这些 oxes 分布在多个“line box”中。如果子元素未设置强制换行的情况下，“inline -box”将不可被分割，将会溢出父元素。

### IFC创建条件
- 块级元素中仅包含内联级别元素

### IFC作用
1. 多个元素水平居中
```
🌰：
//css中
.warp { 
  border: 1px solid skyblue; 
  width: 300px;
  height: 100px;
  text-align: center; 
}
.content { 
  display: inline-block;
  width: 50px;
  height: 100px;
  background: pink; 
}

//html中
<div class="warp">
    <span class="content">1</span>
    <span class="content">2</span>
</div>
```

2. 上下间距不生效
```
🌰：
//css中
.warp { 
  border: 1px solid skyblue; 
  display: inline-block; 
}
.content { 
  margin: 20px; //即使写了上下间距，但不会生效
  background: pink;  
}

//html中
<div class="warp">
    <span class="content">1</span>
    <span class="content">2</span>
</div>
```


