# nginx的安装与使用
### nginx的安装
- 通过yum安装：yum install nginx -y
- 在安装完成nginx过后，会自动生成nginx用户和组。(可以使用id nginx进行查询)

### nginx的相关信息
- /usr/sbin/nginx：nginx的主程序文件
- /etc/nginx：nginx的配置目录
- /usr/share/nginx/html：nginx当前的家目录

### 一些常用命令
- nginx：启动nginx，同时也可以使用systemctl start nginx启动
- nginx -h：显示帮助文件
- nginx -v：显示nginx的版本信息
- nginx -V：显示nginx的版本信息以及编译加载了哪些模块
- nginx -t：做语法检测(默认检查/etc/nginx/nginx.conf，nginx的配置文件，每次修改配置文件之后都需要利用nginx -t检查一下是否有语法错误)
- nginx -q：在检测配置文件期间屏蔽非错误信息
- nginx -s signal: 给一个nginx 主进程发送信号：stop(强制停止), quit(等待连接关闭之后再退出), reopen(用于重新打开日志文件，一般用于日志文件切割), reload(重新加载nginx配置文件，一般情况下，只有nginx的配置文件被修改了，才需要reload，reload是将之前的worker进程全部删除，在重新创建新的worker进程)
- nginx -p prefix: 设置前缀路径（默认是：/usr/local/nginx/）
- nginx -c filename: 设置配置文件（默认是：/usr/local/nginx/conf/nginx.conf）
- nginx -g directives: 用于指定指令，该指令高于配置文件中的设置。如 nginx -g 'daemon off;'，将nginx设置为前台运行
