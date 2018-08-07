## Mac电脑下安装mongodb

### MongoDB是什么

MongoDB是一个基于分布式文件存储的数据库。由C++语言编写。旨在为WEB应用提供可扩展的高性能数据存储解决方案。
他的特点:高性能、易部署、易使用，存储数据非常方便。

本文主要给大家介绍了在mac下安装和配置mongodb的步骤，分享出来供大家参考学习，下面话不多说，来一起看看详细的介绍：

安装 mongodb

install 之前，iTerm2 下用 brew 查看已安装软件、搜索 mongodb：
```bash
brew list
brew search mongodb
```
安装 mongodb :
```bash
brew install mongodb
```
此处需要稍等一段时间，成功后会输出以下即说明安装成功：
```bash
$ brew install mongodb
Updating Homebrew...
==> Downloading https://homebrew.bintray.com/bottles/mongodb-3.4.0.sierra.bottle.1.tar.gz
######################################################################## 100.0%
==> Pouring mongodb-3.4.0.sierra.bottle.1.tar.gz
==> Caveats
To have launchd start mongodb now and restart at login:
 brew services start mongodb
Or, if you don't want/need a background service you can just run:
 mongod --config /usr/local/etc/mongod.conf
==> Summary
🍺 /usr/local/Cellar/mongodb/3.4.0: 17 files, 261.4M
```
启动 mongodb

新建一个 iTerm2 窗口，执行 mongod 尝试启动 mongodb 但会失败 exiting：
```bash
$ mongod
2017-06-12T15:51:49.810+0800 I CONTROL [initandlisten] MongoDB starting : pid=1765 port=27017 dbpath=/data/db 64-bit host=MacBook-Pro-2.local
2017-06-12T15:51:49.810+0800 I CONTROL [initandlisten] db version v3.4.0
2017-06-12T15:51:49.810+0800 I CONTROL [initandlisten] git version: f4240c60f005be757399042dc12f6addbc3170c1
2017-06-12T15:51:49.810+0800 I CONTROL [initandlisten] OpenSSL version: OpenSSL 1.0.2j 26 Sep 2016
2017-06-12T15:51:49.810+0800 I CONTROL [initandlisten] allocator: system
2017-06-12T15:51:49.810+0800 I CONTROL [initandlisten] modules: none
2017-06-12T15:51:49.810+0800 I CONTROL [initandlisten] build environment:
2017-06-12T15:51:49.810+0800 I CONTROL [initandlisten] distarch: x86_64
2017-06-12T15:51:49.810+0800 I CONTROL [initandlisten] target_arch: x86_64
2017-06-12T15:51:49.810+0800 I CONTROL [initandlisten] options: {}
2017-06-12T15:51:49.811+0800 I STORAGE [initandlisten] exception in initAndListen: 29 Data directory /data/db not found., terminating
2017-06-12T15:51:49.811+0800 I NETWORK [initandlisten] shutdown: going to close listening sockets...
2017-06-12T15:51:49.811+0800 I NETWORK [initandlisten] shutdown: going to flush diaglog...
2017-06-12T15:51:49.811+0800 I CONTROL [initandlisten] now exiting
2017-06-12T15:51:49.811+0800 I CONTROL [initandlisten] shutting down with code:100
```
启动 mongodb 之前，要先新建一个mongodb默认的数据写入目录：
```bash
$ mkdir -p /data/db
mkdir: /data/db: Permission denied (没有权限拒绝访问)
```
// sudo 并输入密码，重新新建目录
```bash
$ sudo mkdir -p /data/db
```
Password:
给刚才新建的数据库目录赋予权限：
```bash
$ sudo chown -R guojc /data
```
此时，执行 mongod 启动 mongodb 服务：
```
$ mongod
2017-06-12T16:00:48.036+0800 I CONTROL [initandlisten] MongoDB starting : pid=1837 port=27017 dbpath=/data/db 64-bit host=MacBook-Pro-2.local
2017-06-12T16:00:48.037+0800 I CONTROL [initandlisten] db version v3.4.0
2017-06-12T16:00:48.037+0800 I CONTROL [initandlisten] git version: f4240c60f005be757399042dc12f6addbc3170c1
2017-06-12T16:00:48.037+0800 I CONTROL [initandlisten] OpenSSL version: OpenSSL 1.0.2j 26 Sep 2016
2017-06-12T16:00:48.037+0800 I CONTROL [initandlisten] allocator: system
2017-06-12T16:00:48.037+0800 I CONTROL [initandlisten] modules: none
2017-06-12T16:00:48.037+0800 I CONTROL [initandlisten] build environment:
2017-06-12T16:00:48.037+0800 I CONTROL [initandlisten] distarch: x86_64
2017-06-12T16:00:48.037+0800 I CONTROL [initandlisten] target_arch: x86_64
2017-06-12T16:00:48.037+0800 I CONTROL [initandlisten] options: {}
2017-06-12T16:00:48.037+0800 I STORAGE [initandlisten] wiredtiger_open config: create,cache_size=3584M,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2017-06-12T16:00:48.616+0800 I CONTROL [initandlisten]
2017-06-12T16:00:48.616+0800 I CONTROL [initandlisten] ** WARNING: Access control is not enabled for the database.
2017-06-12T16:00:48.616+0800 I CONTROL [initandlisten] **  Read and write access to data and configuration is unrestricted.
2017-06-12T16:00:48.616+0800 I CONTROL [initandlisten]
2017-06-12T16:00:48.665+0800 I FTDC [initandlisten] Initializing full-time diagnostic data capture with directory '/data/db/diagnostic.data'
2017-06-12T16:00:48.741+0800 I INDEX [initandlisten] build index on: admin.system.version properties: { v: 2, key: { version: 1 }, name: "incompatible_with_version_32", ns: "admin.system.version" }
2017-06-12T16:00:48.741+0800 I INDEX [initandlisten] building index using bulk method; build may temporarily use up to 500 megabytes of RAM
2017-06-12T16:00:48.755+0800 I INDEX [initandlisten] build index done. scanned 0 total records. 0 secs
2017-06-12T16:00:48.756+0800 I COMMAND [initandlisten] setting featureCompatibilityVersion to 3.4
2017-06-12T16:00:48.756+0800 I NETWORK [thread1] waiting for connections on port 27017
```
mongodb 启动成功，正等待着被连接。

新建 iTerm2 窗口，执行 mongo，进入 mongodb 命令行模式：
```
$ mongo
MongoDB shell version v3.4.0
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 3.4.0
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
 http://docs.mongodb.org/
Questions? Try the support group
 http://groups.google.com/group/mongodb-user
Server has startup warnings:
2017-06-12T16:00:48.616+0800 I CONTROL [initandlisten]
2017-06-12T16:00:48.616+0800 I CONTROL [initandlisten] ** WARNING: Access control is not enabled for the database.
2017-06-12T16:00:48.616+0800 I CONTROL [initandlisten] **  Read and write access to data and configuration is unrestricted.
2017-06-12T16:00:48.616+0800 I CONTROL [initandlisten]
>
```
继续在上面的终端输入 show dbs,会列出系统自带的2个数据库：
```bash
> show dbs
admin 0.000GB
local 0.000GB
help
```
小结一下，往后要重新启动 mongodb 服务、进入 mongodb 命令行的操作：
在一个iTerm2窗口执行：mongod //MongoDB starting........waiting for connections
另一个iTerm2窗口执行：mongo //MongoDB shell
