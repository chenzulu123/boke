## mac下安装nginx
```
网上一直没有好的nginx安装的教程，今天我就把我自己在Mac电脑上安装nginx的过程分享一下。
```
### 1、安装工具
```
安装工具使用的是homebrew,homebrew不知道是什么？那你需要去这个网站了解一下了。
```
homebrew官网 https://brew.sh/index_zh-cn.html
### 2、安装步骤
#### 1）打开终端，升级homebrew
```bash
brew update
```
#### 2)查询安装的软件是否存在
```bash
brew search nginx 
```
#### 3）查询安装软件的相关信息，方便后期的操作
```bash
brew info nginx
```
如果你的Mac没有安装过nginx的话，那么会出现下面的信息
```
我们可以看到，nginx在本地还未安装（Not installed），nginx的来源（From），Docroot默认为/usr/local/var/www，在/usr/local/etc/nginx/nginx.conf配置文件中默认端口被配置为8080从而使nginx运行时不需要加sudo，nginx将在/usr/local/etc/nginx/servers/目录中加载所有文件，以及我们可以通过最简单的命令 ‘nginx’ 来启动nginx。
```
#### 4）正式安装
```bash
brew install nginx
```
#### 5）安装好以后打开安装的目录
```bash
open /usr/local/etc/nginx/
```
继续执行命令：
```bash
open /usr/local/Cellar/nginx
```
#### 6）启动nginx
```bash
nginx
```
查看nginx的配置文件信息
```bash
cat /usr/local/etc/nginx/nginx.conf
```
#### 7)修改nginx主页面的页面内容
打开nginx的目录文件夹
```bash
open /usr/local/var/www
//修改目录下的index.html文件即可
```