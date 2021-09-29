# 使用BeautifulSoup进行HTML解析
- #### 详细使用方法可以看:https://www.cnblogs.com/zhaopanpan/p/9319822.html

- #### 使用:
```python
import requests
from bs4 import BeautifulSoup

# 获取html页面内容
def getResponse():
  # url
  url = 'https://movie.douban.com/top250'
  # 构建请求头
  headers = {
    # 代理
    'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/57.0.2987.133 Safari/537.36',
    # 主机
    'Host':'movie.douban.com'
  }
  # 发送请求,返回响应体
  response = requests.get(url=url, headers=headers)
  return response



# 主函数
if '__main__'==__name__:
  # https://www.cnblogs.com/zhaopanpan/p/9319822.html
  response = getResponse()
  soup = BeautifulSoup(response.text, 'html.parser')
  # 按照标准的缩进格式的结构输出
  # print(soup.prettify)
  # 输出匹配到的第一个title标签
  # print(soup.title)
  # 输出匹配到的第一个title标签中的文本
  # print(soup.title.text)
  # 查找所有a标签
  # print(soup.find_all('span'))
  # 匹配第一个class属性为item的标签
  # print(soup.find(class_='item'))

  # 用tag.attrs就可以获取该标签以字典模式返回的所有属性与属性值
  # print(soup.find('a').attrs)
  # 用tag.name来获取标签的名字
  # print(soup.find('a').name)
  # 用tag['attsName']来获取该属性值
  # print(soup.find('a')['href'])

  # 用tag.contents是以列表方式返回该标签下面的子类
  # print(soup.find(class_='item').contents)
  # 用tag.children返回的是一个 list 生成器对象,我们可以用它来对该标签下面的子节点进行遍历
  # for childrenTag in soup.find(class_='item').children:
  #   print(childrenTag)
```
