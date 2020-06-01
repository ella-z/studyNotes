# fs模块
- fs模块的命令
   1. fs.stat 检测是文件还是目录
   2. fs.mkdir 创建目录
   3. fs.writeFile 创建写入文件 
   4. fs.appendFile 追加文件
   5. fs.readFile 读取文件,使用的是绝对路径，而不是相对路径
   6. fs.readdir 读取目录
   7. fs.rename 重命名
   8. fs.rmdir 删除目录
   9. fs.unlink 删除文件
   
   ```
   🌰：
   var fs = require('fs');

    //1. fs.stat 检测是文件还是目录
    fs.stat('../hello.js',(err,data)=>{
        if(err){
            //判断是否报错（例如：路径错误问题）
            console.log(err);
            return;
        }else{
            console.log(`是文件:${data.isFile()}`);
            console.log(`是目录:${data.isDirectory()}`);
            //根据判断输出false或者true
        }
    });
    
    //2. fs.mkdir(path,options,callback) 创建目录
    /** 
     * path 将创建的路径
     * options 可选值，option数组对象，包括：
     *   · encoding 默认'utf8'
     *   · mode 目录权限（读写权限） 777
     *   · flag 默认值'w'
     * callback 回调函数，可选*/

    fs.mkdir('./test',(err)=>{
        if(err){
            //创建失败
            console.log(err);
            return;
        }else{
            console.log('创建成功');
        }
    });

    //3. fs.writeFile 创建写入文件
    //写入文件后，若再次写入，会将之前的内容所替换掉
    /**
     * filename 文件名称
     * data 将要写入的内容，可以是字符串或者buffer数据
     * options 可选值，option数组对象，包括：
     *   · encoding 默认'utf8'
     *   · mode 文件读写权限，默认值 438
     *   · flag 默认值'w'
     * callback {Function} 回调，传递一个异常参数err
     */
    fs.writeFile('./test/index.html','hello nodeJs',(err)=>{
        if(err){
            console.log(err);
            return;
        }else{
            console.log('写入成功');
        }
    });
    
    //4. fs.appendFile 追加文件
    //写入文件后，若再次写入，内容会添加在原来文件的末端
    fs.appendFile('./test/index.html','data',(err)=>{
        if(err){
            console.log(err);
            return;
        }else{
            console.log('写入成功');
        }
    });
    
    //5. fs.readFile 读取文件
      fs.readFile('./test/index.html',(err,data)=>{
          if(err){
              console.log(err);
              return;
          }else{
              console.log(data);//默认数据类型是buffer
              console.log(data.toString());
          }
      });
      
      //6. fs.readdir 读取目录
      fs.readdir('./test',(err,data)=>{
          if(err){
              console.log(err);
              return;
          }else{
              console.log(data);//读取到当前目录下的所有文件以及文件夹
          }
      });

      //7. fs.rename 重命名
      /**功能：
       * 1. 重命名
       * 2. 移动文件
       * 参数:
       * 1.oldPath //旧路径
       * 2.newPath //新路径
       * 3.callblack //回调函数
       */
       //重命名
      fs.rename('./test/test.css','./test/index.css',(err)=>{
          if(err){
              console.log(err);
              return;
          }else{
              console.log('重命名成功');
          }
      });
      //移动文件
      fs.rename('./test/index.css','./test1/index.css',(err)=>{
          if(err){
              console.log(err);
              return;
          }else{
              console.log('移动文件成功');
          }
      });
      
      //8.fs.rmdir 删除目录
      //只能删除空的目录
      fs.rmdir('./test1',(err)=>{
          if(err){
              console.log(err);
              return;
          }else{
              console.log('删除目录成功');
          }
      });
      
      //9.fs.unlink 删除文件
       fs.unlink('./test1/index.css',(err)=>{
          if(err){
              console.log(err);
              return;
          }else{
              console.log('删除文件成功');
          }
      });
   ```
   
   ### mkdirp模块
   - 第三方模块，需要npm安装。
   - 作用：判断某个目录是否存在，若不存在就创建该目录，若存在就不执行任何操作。支持形成多级目录创建。
   
   ### 小案例
   ```
   //题目：wwwroot文件夹下面有 image css js 以及 index.html，若需要找出wwroot目录像下面的所有的目录，并将这些目录放在一个数组中。
   
   //错误写法
   const fs = require('fs');
   var path = './wwwroot';
   
   var dirArr=[];
   fs.readdir(path,(err,data)=>{
      if(err){
         console.log(err);
         return;
      }
      console.log(data);
      for(let i = 0 ;i <data.length;i++){
         fs.stat(path+'/'+data[i],(error,stats)=>{     //fs.stat是异步操作
            if(stats.isDirectory()){                 
               dirArr.push(data[i]);
            }
         });
      }
      console.log(dirArr);//输出的是空数组
   })
   
   //正确写法：
   //1. 改造for循环，使用递归实现
   //2. 使用 async await
   
   //方法1：
   const fs = require('fs');
   var path = './wwwroot';
   
   var dirArr=[];
   fs.readdir(path,(err,data)=>{
      if(err){
         console.log(err);
         return;
      }
      console.log(data);
      (function getDir(i){
      if(i == data.length){
          console.log(dirArr);//image css js
          return;
      }
      fs.stat(path+'/'+data[i],(error,stats)=>{
            if(stats.isDirectory()){                 
               dirArr.push(data[i]);
            }
            getDir(i+1);
         });})(0);
   })
   
   //方法2：
   //首先，定义一个isDir方法判断一个资源到底是目录还是文件
   async function isDir(path){
      return new Promise((res,rej)=>{
          fs.stat(path,(error,stats)=>{
            if(error){
               console.log(error);
               return;
            }
            if(stats.isDirectory()){
               resolve(true);
            }else{
               resolve(false);
            }
         }
      });
   }
   //然后，获取wwwroot里面的所有资源循环遍历
   function main(){
      let path="./wwwroot";
      let dirArr = [];
      fs.readdir(path,async (err,data)=>{
      if(err){
         console.log(err);
         return;
      }
      console.log(data);
      for(let i = 0 ;i <data.length;i++){
          if(await isDir(path+'/'+data[i])){
            dirArr.push(data[i]);
          }
      }
      console.log(dirArr);
   }
   ```
   
   ### fs模块中的流以及管道流
   - fs.createReadStream 从文件流中读取数据
   - fs.createWriteStream 写入文件
   
   #### createReadStream以及createWriteStream
   ```
   🌰：
   const fs = require('fs');

   //创建读取流
   const readStream = fs.createReadStream('./data/data.txt');

   var count = 0; //计算读取了多少次
   //获取读取到的数据
   readStream.on('data',(data)=>{
       console.log(data.toString());
       count++;
   });

   //监控是否读取完成
   readStream.on('end',()=>{
       console.log('end',count);
   });

   //监控读取过程中是否报错
   readStream.on('error',(err)=>{
       console.log(err);
   });
   
   ---------------------------------------------------------------------------------------------------------
   
   var data = "新加入数据";
   //创建一个写入流
   var writeStream = fs.createWriteStream('./data/data.txt'); //会覆盖掉之前存在的数据
   //使用utf-8编码写入数据
   writeStream.write(data,'utf-8');
   //标记文件末尾end() 若没有end(),就无法触发finish
   writeStream.end();
   //处理流事件->finish事件
   writeStream.on('finish',()=>{
       console.log('end!');
   });
   writeStream.on('error',(err)=>{
       console.log(err.stack);
   });
   ```
   
   #### 管道流
   - 主要用于大文件的复制
   
   ```
   🌰：
   const fs = require('fs');
   
   //创建一个可读流
   var readStream = fs.createReadStream('./data/data.txt');
   //创建一个可写流
   var writeStream = fs.createWriteStream('./data/outdata.txt');
   //管道读写操作
   //将data.txt文件内容，写入outdata.txt文件中
   readStream.pipe(writeStream);
   console.log('finish!');
   ```
   
   
