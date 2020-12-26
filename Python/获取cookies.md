# 获取cookie
- 模拟请求后，获取对应的cookies。
- 如何获取cookie？
   - session会话对象
      - 作用：
         1. 可以进行请求发送
         2. 如果请求过程中产生了cookie，则该cookie会被自动存储/携带在session对象中。
      - session对象如何创建？
         ```
          session = request.Session()
         ```

