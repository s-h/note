## python核心编程 ##
### 第20章 web编程 ###
#### 20.1 介绍 ####
本章是有关**web编程**的介绍 
**HTTP**协议是TCP/IP协议的上层协议,这意味着HTTP协议依靠TCP/IP协议来进行低层的交流工作. 
**HTTP**协议属于无状态协议. 
#### 20.2 使用python进行web应用:创建一个简单的web客户端 ####
_浏览器只是Web客户端的一种_
#####urlparse##### 模块提供了操作URL字符串的基本功能. 
urlparse.urlparse() 将URL字符串拆分成一些主要部件,语法如下:

    urlparse.urlparse(urlstr, defProtSch=None, allowFrag=None)
	>>urlparse.urlparse('http://www.python.org/doc/FAQ.html')
	('http‘， ‘www.python.org', '/doc/FAQ.html', '', '', '')

urlparse()将urlstr解析成一个6元组(port_sch(网络协议或者下载规则)、net_loc(服务器位置)、path(文件位置)、params(可选参数)、query(连接符&链接键值对)、frag(拆分文档中的特殊锚)) 
 
urlparse.urlunparse() 的功能与urlpase()完全相反:它拼合一个6元组. 
 
urlpase.urljoin() 在需要多个相关URL的时候就需要urljoin()的功能了,语法如下: 

    urlpase.urljoin(baseurl, newurl, allowFrag=None)
	>>urlpase.urljoin('http://www.python.org/doc/FAQ.html', 'current/lib/lib.htm')
	'http://www.python.org/doc/current/lib/lib.html'


#####urllib#####模块 提供了在给定的URL地址下载数据的功能,同时也可以通过字符串的编码、解码来确保他们是有效URL字符串的一部分. 

1.**urllib.urlopen()** 打开了给定URL字符串与Web链接,并返回了文件的类型,一旦连接成功,将支持f.read()、f.readline()、f.readlines()、f.close()、f.fileno(). 

	urlopen(urlstr, postQueryData=None)

_如果基于数字的权限验证、重定位、cookie等问题,我们建议使用urllib2模块_

2.**urlib.urlretrieve()** 可以方便得将urlstr定位到的整个HTML文件下载到你的本地硬盘上. 
3.**urllib.quote() 和 urllib.quote-plus()** 获取URL数据,并将其编码,从而试用于URL字符串中.尤其一些不能被打印的或不被WEB服务器作为有效URL接收的特殊字符串必须被转换. 

    urllib.quote(urldata, safe='/')

_逗号、下划线、句号、斜线和字母数字这类符号是不需要转化的.其他的则均需要转换.另外，那些不被允许的字符前边会被加上%同时转换成16进制._

5.**urllib.unquote() 和 urllib.unquote_plus()** 和上边的功能完全相反 
6.**urllib.urlencode()** 接收字典的 键-值对,编译为'键=值' 

