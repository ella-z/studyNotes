# pm2
### pm2是什么
- pm2是一个基于Nodejs的进程管理工具，可以用它管理node进程，并且查看node进程的状态，它还支持性能监控，进程守护等功
能。
   - 进程守护功能的作用：当系统崩溃的时候自动重启。
- pm2具有负载均衡功能，可以保持node应用进程永远运行在后台。
- pm2具有deploy(部署)功能，可以部署项目。

### 为什么要用pm2
1. 因为node.js是单进程，若该进程被杀死，那么可能会导致整个服务器崩溃，所以需要用到进程管理器pm2解决这个问题。
2. 部署项目更方便。
   - 之前部署项目：
      1. 需要将项目从github上下载下来。
      2. 通过ftp等方法传送到服务器上。
      3. 使用npm i 安装依赖
      4. 编译css，解压js文件
      .....
      - 若要新增功能还需要重复以上功能。
      
   - 而使用pm2部署项目只需要简单的几步：
      1. 将项目push到github上。
      2. 将项目从github拉取到服务器中。
      3. 在服务器中部署并运行项目。
      - 若要新增功能，只需要将项目在服务器上重新部署并运行即可。
      
### pm2的常用命令
- pm2 start app.js                 # 启动app.js应用程序
- pm2 start app.js -i 4            # cluster mode模式启动4个app.js的应用实例，4个应用程序会自动进行负载均衡
- pm2 start app.js --name="name"   # 启动应用程序并命名为name
- pm2 start app.js --watch         # 监控应用程序，当文件发生改变时，重新应用
- pm2 start script.sh              # 启动 bash 脚本
- pm2 list                         # 列表 PM2 启动的所有的应用程序
- pm2 monit                        # 显示每个应用程序的CPU和内存占用情况
- pm2 show [app-name]              # 显示应用程序的所有信息
- pm2 logs                         # 显示所有应用程序的日志
- pm2 logs [app-name]              # 显示指定应用程序的日志
- pm2 flush                        # 清空所有日志文件
- pm2 stop all                     # 停止所有的应用程序
- pm2 stop 0                       # 停止 id为 0的指定应用程序
- pm2 restart all                  # 重启所有应用
- pm2 reload all                   # 重启 cluster mode下的所有应用
- pm2 gracefulReload all           # Graceful reload all apps in cluster mode
- pm2 delete all                   # 关闭并删除所有应用
- pm2 delete 0                     # 删除指定应用 id 0
- pm2 scale api 10                 # 把名字叫api的应用扩展到10个实例
- pm2 reset [app-name]             # 重置重启数量
- pm2 startup                      # 创建开机自启动命令
- pm2 save                         # 保存当前应用列表
- pm2 resurrect                    # 重新加载保存的应用列表


[参考：pm2](https://pm2.keymetrics.io/)

[参考：部署项目](https://www.jianshu.com/p/51bf8cf5227c?from=groupmessage)
