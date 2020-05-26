# CentOS软件安装相关的命令
### rpm（了解）
- rpm -i <packagename> //安装指定的软件包
- rpm不会自动下载包相关的依赖。
- rpm只能安装离线安装包，不支持在线安装。
- rpm -e <packagename> //删除指定的包，一般不会卸载成功，因为不会自动删除依赖。
- rpm -qa //列出所有安装的软件包
### yum
- yum install <packagename> //安装指定的软件包
- yum list installed //列出所有已经安装的软件包
- yum search <packagename> //搜索软件包
- yum remove <packagename>  //用来移除软件包
- yum update <packagename> //更新软件包
- yum check-update //检查更新，列出所有可以更新的软件包
- yum info <packagename>  //列出指定软件包详情
