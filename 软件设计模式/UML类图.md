# UML类图
- UML是一种基于面向对象的可视化的通用建模语言。
- UML的特点有：
   1. 统一标准
   2. 面向对象
   3. 可视化、表示能力强
   4. 独立于过程
   5. 易掌握、易用
- UML的定义包括UML语义和UML表示法两个部分。
   - UML语义：描述基于UML的精确元模型定义，元模型为UML的所有元素在语法和语义上提供了简单、一致、通用的定义性说明，使开发者能在语义上取得
   一致，消除了因人而异的表达方法所造成的影响。此外UML还支持对元模型的扩展定义。
   - UML表示法：定义UML符号的表示法，为开发者或开发工具使用这些图形符号和文本语法为系统建模提供了标准。这些图形符号和文字所表达的是应用级
   的模型，在语义上它是UML元模型的实例。 

## UML
### 类与接口
- 符号的含义：'+':public , '-':private ，'#' protected

#### 类:
<br />
<img src="https://github.com/ella-z/studyNotes/blob/master/%E8%BD%AF%E4%BB%B6%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/images/UML/%E7%B1%BB.png" title="类" width="150px" height="70px" >

- 第一层：类的名称。若字体是斜体，则该类为抽象类。
- 第二层：类的特性 —— 字段和属性。
- 第三层：类的操作 —— 方法或行为。

#### 接口，接口有两种表示法：
   1. 矩形表示法
   <br />
   <img src="https://github.com/ella-z/studyNotes/blob/master/%E8%BD%AF%E4%BB%B6%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/images/UML/%E6%8E%A5%E5%8F%A31.png" title="矩形表示法-接口" width="120px" height="70px" >
   
   - 第一层：<<interface>> 接口名称
   - 第二层：接口方法
   
   2. 棒棒糖表示法
   <br />
   <img src="https://github.com/ella-z/studyNotes/blob/master/%E8%BD%AF%E4%BB%B6%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/images/UML/%E6%8E%A5%E5%8F%A32.png" title="棒棒糖表示法-接口" width="100px" height="100px" >
   
   - 接口的名称，在圆圈旁边。
   - 接口的方法，在实现类中出现
   
### 关系
- 继承关系：
 <br />
 <img src="https://github.com/ella-z/studyNotes/blob/master/%E8%BD%AF%E4%BB%B6%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/images/UML/%E7%BB%A7%E6%89%BF.png" title="继承关系" width="150px" height="130px" >
 
- 实现接口：
 <br />
 <img src="https://github.com/ella-z/studyNotes/blob/master/%E8%BD%AF%E4%BB%B6%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/images/UML/%E5%AE%9E%E7%8E%B0%E6%8E%A5%E5%8F%A3.png" title="实现接口" width="100px" height="130px" >
 
 - 关联关系：
 <br />
 <img src="https://github.com/ella-z/studyNotes/blob/master/%E8%BD%AF%E4%BB%B6%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/images/UML/%E5%85%B3%E8%81%94%E5%85%B3%E7%B3%BB.png" title="关联关系" width="200px" height="100px" >
 
 - 聚合关系：
 <br />
 <img src="https://github.com/ella-z/studyNotes/blob/master/%E8%BD%AF%E4%BB%B6%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/images/UML/%E8%81%9A%E5%90%88%E5%85%B3%E7%B3%BB.png" title="聚合关系" width="200px" height="100px" >
 
 - 组合/合成关系：
 <br />
 <img src="https://github.com/ella-z/studyNotes/blob/master/%E8%BD%AF%E4%BB%B6%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/images/UML/%E7%BB%84%E5%90%88%E5%85%B3%E7%B3%BB.png" title="组合/合成关系" width="200px" height="100px" >
 
 - 依赖关系：
 <br />
 <img src="https://github.com/ella-z/studyNotes/blob/master/%E8%BD%AF%E4%BB%B6%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/images/UML/%E4%BE%9D%E8%B5%96%E5%85%B3%E7%B3%BB.png" title="依赖关系" width="200px" height="100px" >
 
 
 
 
 
 
 
   
   
