# BOM
- 浏览器对象模型
- BOM可以使我们通过JS来操作浏览器
- 在BOM中为我们提供了一组对象，用来完成对浏览器的操作
- BOM对象
   - Window
      - 代表的是整个浏览器的窗口，同时window也是网页中的全局对象
   - Navigator
      - 代表当前浏览器的信息，通过该对象可以来识别不同的浏览器
   - Location
      - 代表当前浏览器的地址栏信息，通过Location可以获取地址栏信息或者操作浏览器跳转页面
   - History
      - 代表浏览器的历史记录，可以通过该对象来操作浏览器的历史记录，由于隐私问题，该对象不嫩恶搞获取到具体的历史记录，只能操作浏览器向前或向后翻页。
         而且该操作只在当次访问时有效。
   - Screen
      - 代表用户的屏幕信息，通过该对象可以获取到用户的显示器的相关信息。

### Navigator
   - 由于历史原因，Navigator对象中的大部分属性都不能帮助我们识别浏览器了。
   - 一般只是用userAgent来判断浏览器的信息。
      - userAgent是一个字符串，这个字符串中包含有用来描述浏览器信息的内容，
      不同的浏览器含有不用的userAgent
   - 如果使用userAgent不能判断，还可以通过一些浏览器特有对象来判断浏览器信息。
      - 比如 IE11中的ActiveXObject
```
🌰：
//判断是什么浏览器
var ua = navigator.userAgent;

if(/firefox/i.test(ua)){
  console.log("firefox");
}else if(/chrome/i.test(ua)){
  console.log("chrome");
}else if(/msie/i.test(ua)){
  console.log(IE); //IE11不能通过userAgent识别 
}

```

### History
- 属性：
   1. length属性，可以获取当前访问的连接数量
- 方法：
   1. back(),回退到上一个页面。
   2. forward()，可以跳转到下一个页面。
   3. go(),可以跳转到指定页面，它需要一个整数作为参数，1为向前跳转一个页面，2向前跳转两个页面，-1为向后跳转一个页面。
   
### Location
- 如果直接打印location，可以获取当前地址栏的信息(完整路径)。
- 如果直接将location属性修改为一个完整的路径或相对路径，则我们页面会自动跳转到该路径，并且生成历史记录。
- 属性：
   1. href 设置或返回完整的URL。
   等等
- 方法：
   1. assign() 用来跳转到其他页面，作用与直接修改location一样。
   2. reload() 重新加载当前页面，也就是刷新。若传递true参数，则会强制清空缓存刷新页面。
   3. replace() 以新的页面替换旧的页面，调用完毕也会跳转页面，当不会生成历史记录，无法跳转回去。






