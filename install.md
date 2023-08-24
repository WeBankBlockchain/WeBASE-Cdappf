## 部署说明

### 1. 依赖环境

| 环境     | 版本              |
| ------ | --------------- |
| nginx   | nginx1.6或以上版本，安装请参考[附录](appendix.html) |
| HBuilderX | [HBuilderX官网](https://www.dcloud.io/hbuilderx.html) |

### 2. 拉取代码

执行命令：

```shell
git clone https://gitee.com/weizhong-shenda-zhengshu/certificateClient.git
```

### 3. 编译代码
现有代码已编译，在以下目录，将已编译文件打包上传服务器。如需重新编译，使用HBuilderX发行。
```shell
./certificateClient/vueapp/unpackage/dist/build/h5
```

### 4. 修改配置

在服务器已有nginx的配置文件`/usr/local/nginx/conf/nginx.conf`增加一个server：

```
    server {
        listen       80;
        server_name  127.0.0.1;
        location / {
            root   /data/fintech/h5; # 静态页面路径
            index  index.html index.htm;
            add_header Cache-control no-cache;
            try_files $uri $uri/ /index.html =404;
        }

        include /etc/nginx/default.d/*.conf;

        location /fintech-cert {
            proxy_pass    http://127.0.0.1:8081/fintech-cert/; # 后端服务链接
            proxy_set_header    Host    $host;
            proxy_set_header    X-Real-IP    $remote_addr;
            proxy_set_header    X-Forwarded-For    $proxy_add_x_forwarded_for;
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }
```

### 5. 启动nginx

	/usr/local/nginx/sbin/nginx						 # 启动 Nginx
	/usr/local/nginx/sbin/nginx -s reload            # 重新载入配置文件
	/usr/local/nginx/sbin/nginx -s reopen            # 重启 Nginx
	/usr/local/nginx/sbin/nginx -s stop              # 停止 Nginx
	ps -ef | grep nginx                              # 查看nginx进程

### 6.  查看日志

```
进程日志：tail -f /usr/local/nginx/logs/access.log
错误日志：tail -f /usr/local/nginx/logs/eror.log
```

