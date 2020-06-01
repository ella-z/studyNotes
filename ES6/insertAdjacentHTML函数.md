# insertAdjacentHTML
- 概述：将指定的文本解析为HTML或XML，并将结果节点插入到DOM树中的指定位置。
- 语法：element.insertAdjacentHTML(position,text);
   - position:
      1. beforebegin:元素自身前面。
      2. afterbegin:插入元素内部的第一个子节点之前。
      3. beforeend:插入元素内部的最后一个子节点之后。
      4. afterend：元素自身后面。
   - text：是要被解析为HTML或XML，并插入到DOM树中的字符串。

### insertAdjacentHTML与appendChild区别
- appendChild不支持追加字符串的子元素，必须通过createElement动态创建元素再添加进去，
而insertAdjacentHTML支持追加字符串的元素。
