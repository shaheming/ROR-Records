通过将数据转换为json 的形式将参数传给js



```html
var a=  <%=  raw @data.to_json %>；
```
！！@data 需要是一个array

问题？如果是其他的数据形式应该怎么用呢？

