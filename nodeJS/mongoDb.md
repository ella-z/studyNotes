# mongoDb
### mongoDb commands
   1. 创建数据库、表
      - show dbs //展示所有的数据库
      - use databaseName //使用名为databaseName的数据库，若该数据库不存在，则会创建名字为databaseName的数据库
   2. 插入数据
      - db.collectionName.insert({"name1":"value1","name2":"value2"}) // 在当前的数据库中的名为collectionName表中插入数据
   3. 查询
      - show collections //查看数据库中的表(集合)
      - db.tableName.find() //查看名为tableName的表中有什么数据
         + db.tableName.find({"name":value}) //查找指定数据，相当于 select * from tableName where name = value;
         + db.tableName.find({"name1":value1,"name2":value2}) //查找指定数据，相当于 select * from tableName where name1 = value1 and name2 = value2;
         + db.tableName.find({$or:["name":value1,"name":value2]}) //查询 name = value1 或者 name = value2的值,相当于 select * from tableName where name1 = value1 or name2 = value2;
         + db.tableName.find({"name":{$gt:value}}) //查询在name中值大于value的数据
         + db.tableName.find({"name":{$lt:value}}) //查询在name中值小于value的数据
         + db.tableName.find({name:{$gte:value1,$lte:value2}}) //查询在name中值大于等于value1并且小于等于value2的数据
         + db.tableName.find({name:/xxx/}}) //查询name中包含xxx的数据，相当于select * from tableName name like '%xxx%';
         + db.tableName.find({name:/^xxx/}}) //查询name中以xxx开头的数据，相当于select * from tableName name like 'xxx%';
         + db.tableName.find({name:/xxx$/}}) //查询name中以xxx结尾的数据，相当于select * from tableName name like '%xxx';
         + db.tableName.find({name:/xxx/},{name1:1,name2:1}) //查询指定列，只显示满足条件的数据中的name1，name2列
         + db.tableName.find().sort({name:1}) //查询后，按照name进行升序排序
         + db.tableName.find().sort({name:-1}) //查询后，按照name进行降序排序
         + db.tableName.find().limit(n) //查询前n条数据
         + db.tableName.find().skip(n) //查询n条以后的数据
         + db.tableName.find().limit(n).skip(n) //查询n条以后的数据的前n条数据
         + db.tableName.findOne() //查询第一条数据
         + db.tableName.find().count() //查询某个结果集记录的条数
      - db.tableName.distinct("name") //查询去掉当前表中某列的重复数据，相当于select distict name from tableName;
   4. 删除
      - db.dropDatabase() //删除当前所在的数据库
      - db.collectionName.drop() //删除名为collectionName的表(集合)
      - db.collectionName.remove({name:value}) // 移除name中值为value的数据
      - db.tableName.remove({"name":{$gt:value}}) //删除在name中值大于value的数据
      - db.tableName.remove({"name":{$gt:value}},{justOne:true}) //删除一条在name中值大于value的数据
   5. 修改数据
      - db.tableName.update({name:value1},{$set:{name:value2}});//将name的值修改为value2
      - db.tableName.update({name:value},{name1:value1,name2:value2}); //完整替换
      - db.tableName.update({name:value},{$inc:{age:50}},false,true); //相当于 update tableName set age = age + 50 where name = value;
### mongoDb demo
```
//在student的表中添加100条数据
for(let i = 0;i<100;i++){
   db.student.insert({"num":i});
}

```

### MongoDb索引
- 索引是对数据库表中一列或多列的值进行排序的一种结构,设置索引可以使得我们的查询执行时间变短。
- 创建索引:db.tableName.ensureIndex({"username":1}) //1表示name键的索引按升序存储，-1表示为降序
   + 在创建索引的时候可以为其指定索引的名字//db.tableName.ensureIndex({"username":1},{"name":"nameIndex"})
- 获取当前集合的索引:db.tableName.getIndex()
- 删除索引:db.tableName.dropIndex({"username":1})
#### 复合索引
- 复合索引的创建:db.tableName.ensureIndex({"username1":1,"username2":-1})
   + 复合索引创建后，基于username1和username2的查询将会用到该索引，或者是基于username1的查询也会用到该索引，但是基于username2的查询将不会用到该复合索引。
   + 若想用到复合索引，必须在查询条件中包含符合索引中的前N个索引列。
#### 唯一索引
- 创建唯一索引:db.tableName.ensureIndex({"username":1},"unique":true)
   + 若再次插入username重复的文档，MongoDB将会报错，以提示插入重复键 //例如：db.tableName.ensureIndex({"username":1}) //error，类似于mysql中的键的unique功能。
   
### MongoDb的explain工具
- explain返回的是一个文档，而不是游标本身。
- executionStats查询具体的执行时间//db.tableName.find().explain("executionStats")
 
### MongoDb的权限设置
- 步骤：
   1. 创建超级管理员
   ```
   use admin
   
   db.createUser({
      user:'admin',
      pwd:'123456',
      roles:[{role:'root',db:'admin'}] //root表示的就是超级管理员
   })
   ```
   2. 修改MongoDb数据库的配置文件
   - 找到 mongod.cfg 文件，配置security：authorization: "enabled"//开启权限
   3. 重启mongoDb服务
   4. 使用超级管理员账户连接数据库
   ```
   way1：
   mongo admin -u 用户名 -p 密码
   way2：
   mongo admin
   db.auth("admin","123456")
   ```
   5. 给xxx数据库创建一个用户，该用户只能访问xxx数据库，而不能访问其他数据库
   ```
   use xxx
   
   db.createUser({
      user:"username",
      pwd:"123456",
      roles:[{role:"dbOwner",db:"xxx"}] //dbOwner只能访问指定数据库
   })
   ```
- 在admin表中，可以使用show users查询管理员，db.dropUser('xxx')表示删除xxx管理员
- [数据库中的内置角色](https://www.jianshu.com/p/d60c09e45816)

### MongoDb 关系型数据库表
- 关系型数据库中表与表的关系
   1. 一对一的关系
   2. 一对多的关系
   3. 多对多的关系
 
### MongoDb 聚合管道
- 在mongoDb中使用聚合管道可以对集合中的文档进行变换和组合,主要用于表关联查询、数据的统计。
- 用法：db.tableName.aggregate([{<stage>},....])
- 管道操作符：
   1. $project 增加、删除、重命名字段
   2. $match 条件匹配，只满足条件的文档才能进入下一阶段
   3. $lookup 引入其他集合的数据
      ```
      🌰：
        //表的关联查询
        db.order.aggregate([
            {
               $lookup:{
                  from:"order_item", //与order_item关联
                  localField:"order_id", //关联的键
                  foreginFieled:"order_id", //关联的外键
                  as:"items" //将关联的结果放入items中
               }
            }
        ])
   //返回的结果为json类型的数据
      ```
   4. $limit 限制结果数据
   5. $skip 跳过文档的数量
   6. $sort 条件排序
   7. $group 条件组合结果
- 管道表达式
   
```
🌰：
db.tableName.aggregate([
   {$match:{status:"A"} },//在这个表中找到符合status键的值为A的数据
   {$project:{trade_no:1,all_price:1}}, //查询指定列，这里只显示trade_no与all_price列
   {$group:{_id:"$cust_id",total:{$sum:"$amount"}}} //以_id的值不同进行分组，然后再将每个组的amount值相加
])
```   
   
   
   



