# URL 编码 Base64 编码的区别

### 1 url编码

#### 1.1 什么是 url 编码

型如 /url/?%E5%B9%BF%E5%B7%9E=020 形式的 url 是编码后的 url 也叫做百分号编码。

#### 1.2 为什么要用 url 编码

因为 url RFC3986文档规定，Url中只允许包含英文字母（a-zA-Z）、数字（0-9）、-_.~4个特殊字符以及所有保留字符。url编码主要是为了解决一些url中的一些特殊字符和歧义字符或者中文字符的传输问题。

#### 1.3 编码方式

在特殊字符前加上%，例如“name1=value1”,其中value1的值是“va&lu=e1”，对其进行URL编码后：“name1=va%26lu%3D”，这样服务端会把紧跟在“%”后的字节当成普通的字节，就是不会把它当成各个参数或键值对的分隔符。因此，Url编码通常也被称为百分号编码

```ruby
#Generate URL-encoded form data from given enum.
URI.encode_www_form([["q", "ruby"], ["q", "perl"], ["lang", "电费大"]])
=>"q=ruby&q=perl&lang=%E7%94%B5%E8%B4%B9%E5%A4%A7"
URI.encode_www_form ({a:1,b:2})
=> "a=1&b=2"
#If enc is given, convert str to the encoding before percent encoding.
URI.encode_www_form_component([["q", "ruby"], ["q", "perl"], ["lang", "电费大"]])
=>"%5B%5B%22q%22%2C+%22ruby%22%5D%2C+%5B%22q%22%2C+%22perl%22%5D%2C+%5B%22lang%22%2C+%22%E7%94%B5%E8%B4%B9%E5%A4%A7%22%5D%5D"
URI.encode_www_form_component "电费大"
=> "%E7%94%B5%E8%B4%B9%E5%A4%A7" E5A4A7

 "阿斯蒂芬".unpack("U*")
=> [38463, 26031, 33922, 33452]
```

#### 1.4 什么时候需要汉字

- 网址路径中包含汉字【UTF-8】
- 网址的查询字符串包含汉字【操作系统编码】
- Get方法生成的URL包含汉字，在已打开的网页上，直接用Get或Post方法发出HTTP请求，例form方式：【meta标签的charset属性定义的编码】
- Ajax调用的URL包含汉字:【不同浏览器的编码方式不同】
  对包含中文的URL的处理问题，不同浏览器表现不同，很混乱，保证客户端只用一种编码方法向服务器发出请求的解决方案：使用Javascript先对URL编码，然后再向服务器提交，不要给浏览器插手的机会。因为Javascript的输出总是一致的，所以就保证了服务器得到的数据是格式统一的。

### 2 base64 编码

#### 2.1 什么是 base64 编码

**ase64**是一种基于64个可打印字符来表示[二进制数据](https://zh.wikipedia.org/wiki/%E4%BA%8C%E8%BF%9B%E5%88%B6)的表示方法。 base是二进制到文本字符串的转换方法，常用于在URL、Cookie、网页中传输少量二进制数据。如 

```ruby
Base64.encode64("fined method `code")
=> "ZmluZWQgbWV0aG9kIGBjb2Rl\n"
```

#### 2.2 为什么要进行base64编码

是为了满足电子邮件中不能直接使用非ASCII码字符的规定。将8位的非英语字符转化为7位的ASCII字符。

#### 2.3 什么时候进行base64编码：

- 上传图片、文件时

- 拼URL的时候，base64编码后，肉眼看不出信息

#### 2.4 其他


- Base64编码会把3字节的二进制数据编码为4字节的文本数据，长度增加33%，好处是编码后的文本数据可以在邮件正文、网页等直接显示。
- Base64是一种通过查表的编码方法，不能用于加密，即使使用自定义的编码表也不行

另有一种**用于正则表达式的改进Base64**变种，它将“+”和“/”改成了“!”和“-”，因为“+”，“*”以及前面在IRCu中用到的“[”和“]”在[正则表达式](https://zh.wikipedia.org/wiki/%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F)中都可能具有特殊含义。

```ruby
str = "This is line one\nThis is line two\nThis is line three\nAnd so on..."
Base64.encode64(str)
=>"VGhpcyBpcyBsaW5lIG9uZQpUaGlzIGlzIGxpbmUgdHdvClRoaXMgaXMgbGlu\nZSB0aHJlZQpBbmQgc28gb24uLi4=\n"
Base64.strict_encode64(str)
=>"VGhpcyBpcyBsaW5lIG9uZQpUaGlzIGlzIGxpbmUgdHdvClRoaXMgaXMgbGluZSB0aHJlZQpBbmQgc28gb24uLi4="
Base64.urlsafe_encode64(str)
=>"VGhpcyBpcyBsaW5lIG9uZQpUaGlzIGlzIGxpbmUgdHdvClRoaXMgaXMgbGluZSB0aHJlZQpBbmQgc28gb24uLi4="
```



------

参考文章

- [前端中关于编码和转码的知识小总结](http://supermaryy.com/2016/10/30/knowledge-about-encoding-and-transcoding/)

