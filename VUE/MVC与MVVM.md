# MVC与MVVM
### MVC
<br />
<img src="http://zhangzqcloud.cn/file-images/MVC.png" title="MVC" width="600px" height="500px">

- MVC有三个部分：
   - 视图(View)：视图是依据模型数据创建的。
   - 控制器(Controller)：负责从视图读取数据，控制用户输入，并向模型发送数据。
   - 模型(Model)：通常负责在数据库中存取数据。
   
   
### MVVM
<br />
<img src="http://zhangzqcloud.cn/file-images/MVVM.png" title="MVVM" width="600px" height="500px">

- MVVM由三个部分组成：
   - 模型(Model)：包含了业务和验证逻辑的数据模型
   - 视图(View)：视图是用户在屏幕上看到的结构、布局。
   - 视图模型(ViewModel)：是一个同步View 和 Model的对象，View 和 Model 之间并没有直接的联系，而是通过ViewModel进行交互。ViewModel 通过双向数据绑定把 View 层和 Model 层连接了起来，相当于是把MVC里的controller的数据加载，加工功能分离出来。
   
### MVC与MVVM的区别
- MVVM采用了双向绑定，即当Model变化时，View-Model会自动更新，View也会自动变化，而用户操作View的时候，ViewModel 感知到变化，然后通知 Model 发生相应改变。


   
