### 1、防止出现 nil 的 array

在处理对方的来的 json 数据时，对方来的数据有可能不完成，为了避免对方的数据出错，我们需要考虑特殊情况。

#### 1.1 数据的类型不对

本来期待对方来的数据的类型是 int 结果可能对方来的数据却为 string. 为了避免这样的问题，我们应该将从json 中解析出来的数据都处理一下，处理成为自己需要的数据类型，例如 `to_s `, `to_i` 等等

### 1.2 出现数据缺失

本来希望对方 dic 的类型中能够有解析出 数组类型的数据，结果由于数据缺失返回的数据为 nil 以至于后面的迭代导致出错，nil 的类没有array 的方法。

为了解决这样的问题我们可以用如下的写法来防止出现 nil 

```ruby
array  = dic['key'] || []
```

在这样的情况下，就算 dic 中没有我们所需要的数据，我们也能够得到一个 array 的数据。

### 2 不能将 each 作为函数的最后一行

我在完成项目的时候有一个函数需要用 each 来迭代寻找一个数字中第一个满足要求的数据

```ruby
def find_first_large_than_zero(array)
  array.each do |item|
    if item['v'] > 0
        return item
    end
  end
end
```

这里会有一个小 bug 即 如果没有找到大于零的值，这个函数将返回 array 的值，而不是返回 nil。

我在这里栽了一个大坑。

修改过的写法应该为

```ruby
def find_first_large_than_zero(array)
  array.each do |item|
    if item['v'] > 0
        return item
    end
  end
  nil
end
```
这样函数就默认返回一个 nil 值。