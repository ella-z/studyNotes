# 协程&asyncio&异步
## 协程
- 协程，也可以被称为微线程，是一种用户态内的上下文切换技术，简而言之，就是通过一个线程实现代码块相互切换执行。
- 协程如何实现？
   - 使用第三方：greenlet
   - 使用yield关键字
   - asyncio模块的装饰器
      ```
       import asyncio
       
       @asyncio.coroutine
       def f1():
          print(1)
          yield from asyncio.sleep(2) #遇到IO耗时操作，自动化切换到tasks中的其他任务
          print(2)
       
       @asyncio.coroutine
       def f2():
          print(3)
          yield from asyncio.sleep(2) #遇到IO耗时操作，自动化切换到tasks中的其他任务
          print(4)
       
       tasks = [
          asyncio.ensure_future(f1())
          asyncio.ensure_future(f2())
       ]
       
       loop = asyncio.get_event_loop()
       loop.run_until_complete(asyncio.wait(tasks))
       
      ```
   - async/await关键字(主要)
      - asyncio的简写
      ```
         import asyncio
         async def f1():
            print(1)
            await asyncio.sleep(2) #遇到IO耗时操作，自动化切换到tasks中的其他任务
            print(2)

         async def f2():
            print(3)
            await asyncio.sleep(2) #遇到IO耗时操作，自动化切换到tasks中的其他任务
            print(4)

         tasks = [
            asyncio.ensure_future(f1())
            asyncio.ensure_future(f2())
         ]

         loop = asyncio.get_event_loop()
         loop.run_until_complete(asyncio.wait(tasks))
      
      ```

## asycio异步编程
- 事件循环：检测并执行某些代码。
   ```
      import asyncio
      
      # 去生成或获取一个事件循环
      loop = asycio.get_event_loop();
      # 将任务放到'任务列表中'
      loop.run_until_complete(task);
   ```
- 协程函数：async def 函数名
- 协程对象：执行协程函数()得到的协程对象
   - 注意：执行协程函数创建协程对象，函数内部代码不会执行。如果想要运行协程函数内部代码，必须要将协程对象交给事件循环来处理。
   ```
    import asyncio
    
    async def func():
        print(1)
    result = func()
    
    # loop = asycio.get_event_loop()
    #loop.run_until_complete(result)
    # ||
    asyncio.run(result)
   ```
      
- await
   - await+IO等待对象(例如：协程对象、Future、Task对象)
      
- task对象
   - 在事件循环中添加多个任务。
   - 用于并发调度协程。
   - 通过asyncio.create_task()创建task对象。
      ```
        #创建task对象
        task1 = asyncio.create_task(fun())
      ```

- future对象
   - task继承于future，task对象内部await结果的处理基于future对象来的。
   
   
### 异步上下文管理
- 此种对象通过定义 aenter() 和 aexit()方法对async with 语句中的环境进行控制。
   
   
   
   
   
   
   
   
   
      
      
