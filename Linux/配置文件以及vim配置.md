# 配置文件以及vim配置
### 配置文件
- 每次打开终端，都会自动执行配置文件里面的代码。
- 一些Linux里面的配置文件
   + /etc/bashrc 文件 
   + ~/bashrc 文件 //当前用户的bashrc
   ```
   🌰：
   1. vim /etc/bashrc
   2. 在/etc/bashrc文件最后面加入 alias md='mkdir'然后保存并退出
   3. 然后在控制台输入 md test 就可以创建一个名为test的文件夹
   4. 就算关掉终端，再重启无论是哪个用户都可以可以使用这个命令创建文件夹（若alias md='mkdir'写入的是~/bashrc那么md命令只能在当前用户登录
   时可以使用，别的用户无法识别这个命令）。
   ```
 
### vim的配置文件
- /etc/vimrc 文件 //修改这个文件所有的用户都能读取到这个配置
- ~/.vimrc //修改这个文件只有当前用户能读取到这个配置
```
🌰：
1. vim /etc/vimrc
2. 在/etc/vimrc文件最后面加入 set nu 然后保存并退出
3. 在所有用户vim文件时都会显示行号
```
