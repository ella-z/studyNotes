# CrawlSpider
- CrawlSpider是一个类，是Spider的一个子类。
- CrawlSpider是全站数据爬取的方式。
- CrawlSpider的使用：
   - 创建一个工程
   - 创建爬虫文件要基于CrawlSpider类：
      - scrapy genspider -t  crawl fileName url
      - 链接提取器：根据指定规则(allow="正则")进行指定链接的提取。
      - 规则解析器：将链接提取器提取到的链接进行指定规则(callback)的解析操作。
      ```
      🌰：
         # 生成文件
         from scrapy.linkextractors import LinkExtractor
         from scrapy.spiders import CrawlSpider, Rule
         
          class CrSpider(CrawlSpider):
          name = 'cr'
          allowed_domains = ['www.test.com']
          start_urls = ['http://www.test.com/']

          rules = (
              # Rule：规则解析器
              # Rule的构造方法中有三个参数：LinkExtractor：链接提取器，callback：回调函数，follow=True：可以将链接提取器，
              继续作用到链接提取器取到的链接所对应的页面中。
              Rule(LinkExtractor(allow=r'Items/'), callback='parse_item', follow=True),
          )

          def parse_item(self, response):
              item = {}
              #item['domain_id'] = response.xpath('//input[@id="sid"]/@value').get()
              #item['name'] = response.xpath('//div[@id="name"]').get()
              #item['description'] = response.xpath('//div[@id="description"]').get()
              return item
      ```
