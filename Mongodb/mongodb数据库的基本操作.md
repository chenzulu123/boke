## MongoDB 数据库的基本使用

//创建 mongodb 数据库连接

```js
var MongoClient = require("mongodb").MongoClient;
var url = "mongodb://localhost:27017/school";

MongoClient.connect(
  url,
  function(err, db) {
    if (err) throw err;
    console.log("数据库已创建!");
    db.close();
  }
);
```

//创建集合

```js
var MongoClient = require('mongodb').MongoClient;
var url = 'mongodb://localhost:27017/';
//创建集合
MongoClient.connect(url, function (err, db) {
if (err) throw err;
console.log('数据库已创建');
var dbase = db.db("school");
dbase.createCollection('site', function (err, res) {
if (err) throw err;
console.log("创建集合!");
db.close();
});
});
```

//使用插入数据
MongoClient.connect(url, function (err, db) {
if (err) throw err;
var dbo = db.db("school");

    //insertOne插入单条数据
    /*var myobj = { name: "刘德华", sex: "男",address:"中国香港" };
    dbo.collection("class").insertOne(myobj, function(err, res) {
        if (err) throw err;
        console.log("文档插入成功");
        //关闭数据库连接
        db.close();
    });*/


    //insertMny插入多条数据
    /*var myobj = [
        { name: "张学友", sex: "男",address:"中国香港",age:54 },
        { name: "黎明", sex: "男",address:"中国香港",age:56 },
        { name: "郭富城", sex: "男",address:"中国香港",age:58 },
        { name: "刘德华", sex: "男",address:"中国香港",age:56 }
    ];
    dbo.collection("class").insertMany(myobj, function(err, res) {
        if (err) throw err;
        console.log("多条数据插入成功");
        //关闭数据库连接
        db.close();
    });*/


    //查询数据
    //查询条件
    /*var whereStr = {
        //查询name为刘德华的数据
        "name": '刘德华'
    }; // 查询条件
    dbo.collection("class")
    .find(whereStr)
    //查询的返回结果是一个数组集合
    .toArray(function (err, result) {
        if (err) throw err;
        console.log(result);
        db.close();
    });*/


    //更新一条数据
    /*var whereStr = {"name":'刘德华'};  // 查询条件
    var updateStr = {$set: { "age" : 59 }};
    //如果数据库中有多条相同的数据，updateOne更新的时候只会更新第一条数据，
    //如果需要更新多条数据的话，需要使用updateMany
    dbo.collection("class").updateOne(whereStr, updateStr, function(err, res) {
        if (err) throw err;
        console.log("更新了一条数据");
        console.log(res);
        db.close();
    });*/


    //更新多条数据
    /*var whereStr = {"name":'刘德华'};  // 查询条件
    var updateStr = {$set: { "age" : 59 }};
    //如果数据库中有多条相同的数据，updateOne更新的时候只会更新第一条数据，
    //如果需要更新多条数据的话，需要使用updateMany
    dbo.collection("class").updateMany(whereStr, updateStr, function(err, res) {
        if (err) throw err;
        console.log("更新了多条数据");
        // console.log(res);
        db.close();
    });*/


    //删除单条数据
    //删除的数据中如果有多条数据的话，如果条件都满足的话，只会删除第一条数据
    /*var whereStr = {"name":'刘德华'};  // 查询条件
    dbo.collection("class").deleteOne(whereStr, function(err, obj) {
        if (err) throw err;
        console.log("已成功删除一条数据");
        db.close();
    });*/

    //{ type: 1 }  // 按 type 字段升序
    //{ type: -1 } // 按 type 字段降序
    //删除年龄为59岁的数据
    /*var whereStr = { age: 59 };  // 查询条件
    dbo.collection("class").deleteMany(whereStr, function(err, obj) {
        if (err) throw err;
        console.log("多条数据已经删除成功！");
        db.close();
    });*/


    //排序查询
    /*
    { type: 1 }  // 按 type 字段升序
    { type: -1 } // 按 type 字段降序
    */
    /*var mysort = { type: 1 };
    dbo.collection("class").find().sort(mysort).toArray(function(err, result) {
        if (err) throw err;
        console.log(result);
        db.close();
    });*/


    //分页查询
    //如果要设置指定的返回条数可以使用 limit() 方法，该方法只接受一个参数，指定了返回的条数。
    /*dbo.collection("class").find().limit(2).toArray(function (err, result) {
        if (err) throw err;
        console.log(result);
        db.close();
    });*/

    //跳过指定查询的条数
    /*dbo.collection("class").find().skip(2).limit(5).toArray(function (err, result) {
        if (err) throw err;
        console.log(result);
        db.close();
    });*/

    //mongodb实现左连接
    /*dbo.collection('orders').aggregate([{
        $lookup: {
            from: 'products', // 右集合
            localField: 'product_id', // 左集合 join字段
            foreignField: '_id', // 右集合 join字段
            as: 'orderdetails' // 新生成字段（类型array）
        }
    }], function (err, res) {
        if (err) throw err;
        console.log(JSON.stringify(res));
        db.close();
    });*/

});
