# nginx的配置文件
- 配置文件采用"一主多子"的配置方式，即一个主配置文件，通过将不同功能或者不同模块的子配置文件进行包含引入的方式。
- nginx.conf是nginx的主配置文件，但一般不直接写在主配置文件中，而是写在在conf.d文件夹下的创建的子配置文件中。
- work进程处理用户的请求，master进程处理系统级别以及管理级别的调动。
```
# 主配置段
# user 指定运行nginx的用户和组（第一个参数为用户（默认的用户是nginx）第二个为组），master进程的user是root
# 在用户的某个请求是否能够处理，要看worker进程的用户权限是否足够。而能否监听某个端口，是由master用户权限决定的。
user  nginx; 

# 指定工作进程数（一般为CPU核数）
worker_processes  auto;   

# 指定记录错误日志为 logs/ 目录下的 error.log 文件
error_log  logs/error.log;
# 指定错误日志，并指定写入格式为 notice
error_log  logs/error.log  notice;
# 指定错误日志，并指定写入格式为 info  
error_log  logs/error.log  info;

# pid 文，存放主进程pid号，主要作用是是在给进程发送信号可以根据pin号找到主程序，如nginx -s reload命令之后，它会读取到nginx.pid文件拿到pin号，并对该进程发起信号。
pid  /run/logs/nginx.pid;

# 包含modules下的.conf配置文件
include /usr/share/nginx/modules/*.conf;

# nginx连接配置模块，用于定义事件驱动相关配置，该配置与连接的处理密切相关
events {
    use method # 定义nginx使用那种事件驱动，在RedHat/CentOS中性能最好的是epoll模型
    worker_connections  1024;# 指定每个工作进程最大连接数为 1024 (默认为1024)
    accept_mutex on|off # 处理新连接的方法，on是指由各个worker进程轮流处理，off则会同之所有worker进程，但是只有一个worker进程获得处理连接的权限。在CentOS7将使用"reuseport"会有更好的性能。
    # 注意：若启动了accept_mutex on，则会启动accpet_mutex_delay 500ms; 参数，时间可以设置，表示当一个worker进程在处理新连接时，多长时间以后才会重新接收下一个新的连接。
}

# http模块配置段
# http 配置模块，在http配置段中可以配置多个server配置，该server就是用来配置虚拟主机的，可以基于IP地址，也可以基于PORT，或者基于域名的方式配置主机。server配置段内还可以再配置多个location字段，该字段用来配置虚拟主机不同的uri的响应方式。
http {
    # 通过 include 加载 mime.types 文件，里面的 types {} 模块将文件扩展名映射到响应的 MIME 类型
    include       mime.types;
    # 定义响应的默认 MIME 类型
    default_type  application/octet-stream;

    # 写入格式 main 的内容格式如下
    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    # 指定访问日志和写入格式为 main
    #access_log  logs/access.log  main;

    # 启用或者禁用 sendfile()
    sendfile        on;
    # 启用或者禁用使用套接字选项（仅在 sendfile 使用时使用）
    #tcp_nopush     on;

    # 0 值禁用保持活动的客户端连接
    #keepalive_timeout  0;
    # 65 s 超时
    keepalive_timeout  65;

    # 启用或者禁用 gzip
    #gzip  on;

    # 虚拟主机配置模块
    server {
        # 监听80端口,默认监听的也是80端口
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  _;
        # 默认响应的地址
        root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;


        # 将特定的文件或目录重新定位，如 php 文件，image 目录等
        location / {
            # 设置请求的根目录
            root   html;
            # 定义索引，按顺序匹配
            index  index.html index.htm;
        }

        # 定义显示 404 错误的 uri
        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        # location 精准匹配 '/50x.html'
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        # 正则表达式匹配 php 文件
        #location ~ \.php$ {
            # 设置代理服务器的协议和地址，以及应该映射位置的可选URI。作为协议，可以指定“http”或“https”。该地址可以指定为一个域名或IP地址，以及一个可选端口
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
             # 设置 FastCGI 服务器的地址。地址可以指定为一个域名或 IP 地址，以及一个端口
        #    fastcgi_pass   127.0.0.1:9000;
             # 设置将在以斜杠结尾的URI之后追加的文件名，
        #    fastcgi_index  index.php;
             # 设置一个应该传递给FastCGI服务器的参数。
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
             # 加载 conf/fastcgi_params 文件
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    # ssl 配置，要启用 ssl 模块需要在编译 nginx 时加上 --with-http_ssl_module 参数
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}
```

### 虚拟主机
- 在/etc/nginx/conf.d下创建子配置文件。
- 在生产环境中，一般一个虚拟主机会单独创建一个配置文件。
```
#在conf.d中创建多个备用目录，作为不同虚拟主机的家目录
mkdir /data/nginx/a

# 创建域名为a.com|www.a.com的虚拟主机
server{
  listen        80;
  server_name   www.a.com a.com;
  root          /data/nginx/a; 
}
```











