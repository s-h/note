## requests
参考文档：http://docs.python-requests.org/zh_CN/latest/user/quickstart.html
### 发送请求

    >>> import requests
    >>> r = requests.get('https://www.jianghao.tech')

    HTTP POST、PUT、DELETE、HEAD、OPTIONS请求
    >>> r = requests.post('http://httpbin.org/post', data = {'key':'value'})
    >>> r = requests.put('http://httpbin.org/put', data = {'key':'value'})
    >>> r = requests.delete('http://httpbin.org/delete')
    >>> r = requests.head('http://httpbin.org/get')
    >>> r = requests.options('http://httpbin.org/get')

### 传递URL参数

    >>> payload = {'key1': 'value1', 'key2': 'value2'}
    >>> r = requests.get('http://httpbin.org/get', params=payload)
    通过打印输出该URL，你能看到URL已被正确的编码:
    >>> print(r.url)
    http://httpbin.org/get?key2=value2&key1=value1

### 读取相应内容

    >>> r = requests.get('http://api.github.com/events')
    >>> r.text
    Requests会自动解码来自服务器的内容，大多数unicode字符集都能被无缝地解码.
    >>>r.encoding
    'utf-8'
    >>>r.encoding = 'ISO-8859-1'
    如果改变了编码，每当你访问r.text，Requests会使用r.encoding新值

### 二进制响应内容
Requests会自动解码gzip和deflate传输编码的相应数据

    >>> r.content

### JSON响应内容

    >>>r.json()

    如果JSON解码失败，r.json()会抛出一个异常
    使用r.raise_for_status()或者检查r.status_code来验证返回值

### 定制请求头部
需要传递一个dict给headers参数
    >>> url = 'https://api.github.com/some/enpoint'
    >>> headers = {'user-agent': 'my-app/0.0.1'}
    >>> r = requests.get(url, headers=headers)

### 响应状态码

    >>> r.status_code
    200

    通过Response.raise_for_status()抛出异常
    >>> bad_r = requests.get('http://httpbin.org/status/404')
    >>> bad_r.status_code
    404

    >>> bad_r.raise_for_status()

### 响应头
查看一个字典像是展示的服务器响应头，这个字典比较特殊，它仅为HTTP头而生。根据RFC2616，HTTP头部是不区分大小写的:

    >>> r.headers
    >>> r.headers['Content-Type']
    >>> r.headers.get['content-type']

## Beautiful Soup
Beautiful Soup 是一个可以从HTML或XML文件中提取数据的Python库。
参考文档：https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/

### 打印标准缩进格式

    >>> from bs4 import Beautifulsoup
    >>> soup = BeautifulSoup(html)
    >>> print(soup.prettify())

### 几个简单的浏览结构化数据的方法

    >>> soup.title
    >>> soup.title.name
    >>> soup.title.string
    >>> soup.title.parent.name
    >>> soup.p['class']
    >>> soup.a
    >>> soup.find_all('a')
    >>> soup.find(id="link3")

### 在文档中找到所有<a>标签的链接

    >>> for link in soup.find_all('a'):
    >>>     print(link.get('href'))
