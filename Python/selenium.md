# selenium
- 作用：
   1. 该模块可以便捷地获取网站中动态加载的数据
   2. 可以便捷实现模拟登录
- selenium：是一个基于浏览器自动化的模块。 

### selenium使用流程
   - 环境安装：pip install selenium
   - 下载浏览器的驱动程序(驱动程序的版本要和浏览器的版本相匹配)
   - 使用selenium模块实例化浏览器对象
   ```
    from selenium import webdriver
    web = webdriver.Chrome(executable_path='./驱动路径')
    web.get('/xxx')
    # 获取页面源码
    page_text = web.page_source
   ```
