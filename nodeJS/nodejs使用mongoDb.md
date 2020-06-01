# nodejs使用mongoDb
- 步骤：
   1. 安装mongoDb模块：npm install mongodb --sava
   2. 引入mongoDb模块
   3. 定义数据库的连接地址以及需要操作的数据库
      - 直连数据库：mongodb://localhost
      - 连接权限数据库：mongodb://adminName:adminPassword@localhost
   4. 实例化MongoClient
   5. 通过实例对象与数据库进行连接
   6. 关闭连接数据库
   ```
   🌰1：
   //引入mongodb
   const { MongoClient } = require('mongodb');
   //定义数据库连接的地址
   const url = 'mongodb://admin:123456@127.0.0.1:27017';
   //定义要操作的数据库
   const dbName = 'student';
   //实例化MongoClient 传入数据库地址
   const client = new MongoClient(url, { useUnifiedTopology: true });
   //连接数据库
   client.connect((err) => {
       if (err) {
           console.log(err);
           return;
       }

       let db = client.db(dbName);

       //1.查找数据
       /*db.collection('students').find({"age":15}).toArray((err,data)=>{
           if(err){
               console.log(err);
               return;
           }else{
               console.log(data);
           }
           //操作完数据库后一定要关闭连接的数据库
           client.close();    
       });*/

       //2.增加数据
       //增加一条数据
       /* db.collection('students').insertOne({ 'name': 'nodejstest', 'age': 50 }, (err, data) => {
            if (err) {
                console.log(err);
                return;
            } else {
                console.log(data); //data中会返回与插入语句相关的信息
            }
            //操作完数据库后一定要关闭连接的数据库
            client.close();
        });

        //增加多条数据
        db.collection('students').insertMany([{'name': 'nodejstest2', 'age': 10},{'name': 'nodejstest3', 'age': 10},{'name': 'nodejstest4', 'age': 10}],(err,data)=>{
            if(err){
                console.log(err);
                return;
            }else{
                console.log(data);
            }
            //操作完数据库后一定要关闭连接的数据库
            client.close();
        })*/

       //3.修改数据
       //只修改一条，即使有多条数据符合条件，也只修改第一条符合条件的数据
       /*db.collection('students').updateOne({'name':'nodejstest'},{$set:{'age':100}},(err,result)=>{
           if(err){
               console.log(err);
               return;
           }else{
               console.log(result);
           }
           //操作完数据库后一定要关闭连接的数据库
           client.close();
       })

       //可修改多条数据
       db.collection('students').updateMany({'name':'nodejstest'},{$set:{'age':150}},(err,result)=>{
           if(err){
               console.log(err);
               return;
           }else{
               console.log(result);
           }
           //操作完数据库后一定要关闭连接的数据库
           client.close();
       })*/

       //4.删除数据
       //只删除一条
       /*db.collection('students').deleteOne({ 'name': 'nodejstest2' }, (err) => {
           if (err) {
               console.log(err);
               return;
           }
           //操作完数据库后一定要关闭连接的数据库
           client.close();
       })*/

       //删除多条数据
       db.collection('students').deleteMany({'name':'nodejstest'},(err)=>{
           if(err){
               console.log(err);
               return;
           }
           //操作完数据库后一定要关闭连接的数据库
           client.close();
       });

   });
   ```
   ```
   🌰2：
      //引入mongodb
      const { MongoClient } = require('mongodb');
      //定义数据库连接的地址
      const url = 'mongodb://admin:123456@127.0.0.1:27017';
      //定义要操作的数据库
      const dbName = 'student';
      
      //连接数据库，这种方式连接数据库，每次连接都会创一个新的client，这样就可以避免在get或者post请求中，多次请求时，抛出异常。
      MongoClient.connect(url,(err,client)=>{
      })
   ```









