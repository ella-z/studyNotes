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
      
      
      
      
      
      
      
      
      
