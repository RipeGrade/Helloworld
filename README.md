# Helloworld

# 开发者: 冇旬
# 开发时间: 2022/3/18  16:30

import requests
import re

domain = "https://www.dytt89.com/"

response1 = requests.get(domain)
response1.encoding = "gb2312"
#print(response.text)

obj1 = re.compile(r'2022必看热片.*?<ul>(?P<artical>.*?)</ul>', re.S)
obj2 = re.compile(r"<a href='(?P<href>.*?)'", re.S)
obj3 = re.compile(r'◎片　　名(?P<name>.*?)<br />.*?<li><a href="(?P<download>.*?)"', re.S)

URL = []
result1 = obj1.finditer(response1.text)
for it in result1:
    #print(it.group("artical"))
    artical = it.group('artical')
    result2 = obj2.finditer(artical)
for i in result2:
    #print(i.group("href"))

    #拼接
    url = domain + i.group('href').strip("/")
    URL.append(url)
#把子页面连接地址装在URL列表内
for url in URL:
#重新拿出来
    response3 = requests.get(url)
    response3.encoding = "gb2312"
#print(response3.text)
#break
    result3 = obj3.search(response3.text)
    print(result3.group("name"))
    print(result3.group("download"))
