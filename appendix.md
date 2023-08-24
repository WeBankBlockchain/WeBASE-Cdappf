## 附录
### 1 安装nginx
#### 1.1 下载nginx依赖
在安装nginx前首先要确认系统中安装了gcc、pcre-devel、zlib-devel、openssl-devel。如果没有，请执行命令

	yum -y install gcc pcre-devel zlib-devel openssl openssl-devel
执行命令时注意权限问题，如遇到，请加上sudo
#### 1.2 下载nginx
nginx下载地址：https://nginx.org/download/（下载最新稳定版本即可）
或者使用命令：

	wget http://nginx.org/download/nginx-1.9.9.tar.gz  (版本号可换)
#### 1.3 安装nginx
##### 1.3.1 解压
	tar -zxvf nginx-1.9.9.tar.gz

##### 1.3.2 进入nginx目录

	cd nginx-1.9.9
##### 1.3.3 配置

	./configure --prefix=/usr/local/nginx

##### 1.3.4 make

	make & make install
##### 1.3.5 启动
	/usr/local/nginx/sbin/nginx
##### 1.3.6 nginx几个常见命令
```shell
/usr/local/nginx/sbin/nginx -s reload            # 重新载入配置文件
/usr/local/nginx/sbin/nginx -s reopen            # 重启 Nginx
/usr/local/nginx/sbin/nginx -s stop              # 停止 Nginx
ps -ef | grep nginx                              # 查看nginx进程
```


