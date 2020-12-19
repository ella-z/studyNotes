# Requests模块
### Requests模块是什么
   - 是python中原生的一款基于网络请求的模块。
### Requests模块的作用的什么
   - 模拟浏览器发请求
### 如何使用：
   1. 指定url
   2. 发起请求(GET/POST)
   3. 获取响应数据
   4. 持久化存储数据
### UA伪装
- User-Agent
   - 作用：告诉HTTP服务器，客户端使用的操作系统和浏览器的名称和版本。
- 为什么需要UA伪装：
   - 门户网站的服务器会检测对应请求的载体身份标识(UA检测)，如果检测到请求的载体身份标识为某一款浏览器，说明该请求是一个正常的请求。但是，如果检测到请求的载体身体标识不是基于某一款浏览器的，则表示该请求为不正常的请求(爬虫)，那么服务器就很有可能拒绝该次请求。
   - UA伪装就是让对应的请求载体身份标识伪装成某一款浏览器。
- 如何实现UA伪装
   - 将对应的User-Agent封装到一个headers字典中

### 小栗子
- 实现百度查询
```
🌰：
import requests

url = 'https://www.baidu.com/s'

headers={
    'User-Agent':'xxxx'
}

keyword = input('请输入你要查询的内容：')

params = {
    'wd':keyword
}

response = requests.get(url=url,params=params,headers=headers)

with open('./search.html','w',encoding='utf-8') as file:
    file.write(response.text)

print('success!')
```

- 获取百度翻译数据
```
🌰：
import requests
import json

url = 'https://fanyi.baidu.com/sug'

headers = {
    'User-Agent':'xxxxx'
}

kw = input('input a word:')

data = {
    'kw':kw
}

response = requests.post(url=url, data=data, headers=headers)
file_name = kw + '.json'
fp = open(file_name, 'w', encoding='utf-8')
json.dump(response.json(), fp=fp, ensure_ascii=False)
print(response.json())
```




