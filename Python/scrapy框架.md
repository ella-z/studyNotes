# scrapy框架
### scrapy框架的使用
- 创建scrapy项目
   ```
    scrapy startproject xxx
   ```
- 创建完成之后，会自动生成几个文件
   - spiders文件夹：爬虫文件夹，必须在该文件夹下放至少一个爬虫源文件。
   - setting.py：配置文件
- 在工程文件夹的目录下，创建一个爬虫文件
   ```
      scrapy genspider spiderName url
   ```
   - 生成爬虫文件
   ```
   🌰：
     import scrapy

     class TestSpider(scrapy.Spider):
      name = 'test' # 爬虫文件的名称，也就是爬虫源文件的一个唯一标识
      allowed_domains = ['www.test.com'] # 允许的域名，用来限定start_urls列表中哪些url可以进行请求发送
      start_urls = ['http://www.test.com/'] # 起始url列表，该列表中存放的url会被scrapy自动进行请求的发送
      # 用作于数据解析，reponse参数表示的就是请求成功后对应的响应对象
      def parse(self, response):
          pass
   ```
- 修改配置文件
   - 填写user-agent信息
   - 设置日志输出的类型log_level
- 执行工程
   ```
      scrapy crawl spiderName
   ```
### scrapy框架数据解析
- xpath解析返回的是列表，但是列表元素一定是Selector类型的对象。获取Selector类型对象中的data，直接.extract()即可。

### scrapy持久化存储
   - 基于终端指令：
      - 要求：只可以将parse方法的返回值存储到本地的文本文件中
      - 注意：持久化存储对应的文本文件的类型有限制(例如：'json','csv'等)
      - 指令：scrapy crawl xxx -o filePath
      - 缺点：局限性比较强
   - 基于管道：
      - 编码流程：
         1. 数据解析
         2. 在item类中定义相关的属性(items.py)
         3. 将解析的数据封装存储到item类型的对象
         4. 将item类型的对象提交给管道进行持久化存储的操作
         5. 在管道类的process_item中要将其接受到的item对象中存储的数据进行持久化存储操作
         6. 在配置文件中开启管道
      - 问题：要是想即将数据一份存储到本地同时也存储到数据库中，如何实现？
      - 解决问题的方法：在管道文件中一个管道类对应将一组数据存储到一个平台或者载体中。可以在管道文件中创建两个类，一个类对应一种存储方法。
         
         
### scrapy五大核心组件
- 调度器
   - 组成以及其作用：
      1. 过滤器：将请求进行一个去重处理。
      2. 队列：将去重后的请求放入队列中。
- 引擎
   - 作用：
      1. 根据数据流触发相应的事物或函数
- 下载器
   - 作用：
      1. 对请求的数据进行下载
- 管道
   - 作用：
      1. 对数据进行持久化存储
- spider：爬虫类
   - 作用：
      1. 产生url并对url进行请求发送。
      2. 对于请求回来的数据进行数据解析。

### scrapy请求传参
   - 使用场景：
      1. 深度爬取

### scrapy爬取图片数据ImagesPipeline
   - 基于scrapy爬取字符串类型的数据和爬取图片类型的数据区别？
      1. 字符串爬取只需要基于xpath进行解析且提交管道进行持久化存储。
      2. 图片爬取，xpath解析出图片src的属性值，还需要单独地对图片地址发起请求获取图片二进制类型的数据。
   - ImagesPipeline
      - 只要需要将img的src的属性值进行解析，提交到管道，管道就会对图片的src进行请求发送获取图片的二进制类型的
      数据，且还会帮我们进行持久化存储。
      - 使用流程：
         1. 导入ImagesPipeline类
           ```
            from scrapy.pipelines.images import ImagesPipeline
           ```
         2. 重写父类三个方法
            - get_media_requests(self，item，info) # 对item中的图片进行请求操作
            - file_path(self，request，response=None，info=None) # 指定图片存储路径
            - item_completed(self，result，item，info) # 该返回值会传递给下一个即将被执行的管道类
         3. 在setting文件中写图片存储地址IMAGES_STORE


### scrapy框架中间件
- 下载中间件：
   - 位置：处于引擎和下载器之间。
   - 作用：批量拦截到整个工程中发起的所有请求和响应。
      - 拦截请求作用：
         - UA伪装
         - 代理IP
      - 拦截响应作用：
         - 篡改响应数据，响应对象。
- middlewares.py - 中间件文件






         


