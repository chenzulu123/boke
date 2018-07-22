### mongodb的基本命令
1、查看有哪些数据库;
```bash
show dbs
```
2、使用数据库
```
use immooc
//如果数据库中没有immooc数据库时，MongoDB灰会自动创建一个immooc数据库。
```
3、db;
```
db
//使用的数据库是新创建的时候需要通过db来进行使用
```
```
db.immooc.find();
//不带条件的查询是查询所有数据
```
数据集合条数的统计
```
db.immooc.find().count();
//统计immooc集合的数据条数
```
```
db.user.insert({name:"chenzulu",age:26,sex:"男"});
//这条语句的操作是给immooc数据库的user表格插入一条数据内容为：
```
给数据集合创建索引
```
db.user.ensureIndex({x:1});
//这里的x:1表示的是正向排序
//X:-1的话表示是逆向排序
```
