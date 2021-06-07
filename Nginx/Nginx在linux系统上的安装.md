# Nginx在Linux系统上的安装.

### 安装编译工具及库文件:
`[root@iZuf6cel2bg23ixuamuwd1Z pcre-8.35]# yum -y install make zlib zlib-devel gcc-c++ libtool  openssl openssl-devel`

### 安装PCRE:
- #### PCRE 作用是让 Nginx 支持 Rewrite 功能。
`[root@iZuf6cel2bg23ixuamuwd1Z pcre-8.35]# wget http://downloads.sourceforge.net/project/pcre/pcre/8.35/pcre-8.35.tar.gz`
- #### 解压安装包:
`[root@iZuf6cel2bg23ixuamuwd1Z pcre-8.35]# tar zxvf pcre-8.35.tar.gz`
- #### 进入安装包目录:
`[root@iZuf6cel2bg23ixuamuwd1Z pcre-8.35]# cd pcre-8.35`
- #### 编译安装:
`[root@iZuf6cel2bg23ixuamuwd1Z pcre-8.35]# ./configure`
`[root@iZuf6cel2bg23ixuamuwd1Z pcre-8.35]# make && make install`
- #### 查看版本:
`[root@iZuf6cel2bg23ixuamuwd1Z pcre-8.35]# pcre-config --version`

### 安装 Nginx:
- #### 下载安装包:
`[root@iZuf6cel2bg23ixuamuwd1Z download]# wget http://nginx.org/download/nginx-1.6.2.tar.gz`
- #### 解压:
提示一下,这里解压之后把文件移动到/usr/local/src/文件夹下面.
`[root@iZuf6cel2bg23ixuamuwd1Z download]# tar zxvf nginx-1.6.2.tar.gz`
- #### 进入安装目录:
`[root@iZuf6cel2bg23ixuamuwd1Z nginx-1.6.2]# cd /usr/local/src/nginx-1.6.2/`
- #### 编译安装:
__这里 --prefix=xxx/xxxx 后面跟的路径是nginx执行之后安装的位置,而 --with-pcre=xxx/xxxx 后后面跟的路径是我们上面安装的pcre路径__
`[root@iZuf6cel2bg23ixuamuwd1Z nginx-1.6.2]# ./configure --prefix=/usr/local/webserver/nginx --with-http_stub_status_module --with-http_ssl_module --with-pcre=/usr/local/src/pcre-8.35`
`make && make install`
- #### 具体查看网址:https://www.cnblogs.com/xxoome/p/5866475.html