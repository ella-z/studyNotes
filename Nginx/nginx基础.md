# nginx基础
### nginx
- nginx是异步框架的web服务器，也可以作用反向代理、负载均衡以及作为缓存服务器。

### nginx的基本功能
1. web服务器。
2. 反向代理服务器：nginx可以作为http协议的反向代理服务器。
3. FastCGI(php)、uWSGI(python)代理服务器：生产环境上经常使用nginx作为客户端请求后端应用服务。
4. TCP/UDP代理服务器。

### nginx的基础架构
- nginx为master/workers结构，一个master主进程，负责管理和维护多个worker进程，真正接收并处理用户请求的其实是worker进程，master不对用户请求
进行处理。即master主进程负责分析并加载配置文件，管理worker进程，接收用户信号传递以及平滑升级等功能。另外，nginx具有强大的缓存功能，
其中Cache Loader负责载入缓存对象，Cache Manager负责管理缓存对象。
<br />
<img src="https://github.com/ella-z/studyNotes/blob/master/Nginx/images/nginx%E5%9F%BA%E7%A1%80%E6%9E%B6%E6%9E%84.PNG" alt="nginx的基础架构" width="600px" height="400px">

### nginx的优点 
- 高并发，高性能：一台普通的服务器可以轻松处理上万并发连接。
- 模块化设计：基于模块化设计，具有非常好的扩展性，可以通过加载、卸载某个模块以实现相应的功能。
- 热部署、热更新：Nginx支持配置文件的热更新，版本热升级、动态加载模块、日志热更换。
- 内存低消耗
- 配置、维护简单

### nginx的组成
- Nginx二进制可执行文件，作用：由各模块源码编译出的一个文件。
- Nginx.conf配置文件，作用：控制nginx的行为。
- access.log访问日志，作用：记录每一条http请求信息。
- error.log错误日志，作用：定位问题。

### nginx配置语法
- 配置文件由指令与指令块构成。
- 每条指令以;分号结尾，指令与参数间以空格符号分隔。
- 指令块以{}大括号将多条指令组织在一起
- include语句允许组合多个配置文件以提升可维护性
- 使用#符号添加注释，提高可读性
- 使用$符号使用变量
- 部分指令的参数支持正则表达式













