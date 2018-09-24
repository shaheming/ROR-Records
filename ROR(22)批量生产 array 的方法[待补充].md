```ruby
 SEATS = begin
    (1..6).to_a.map do |series|
      ["A","B","C"].map do |letter|
        "#{series}#{letter}"
      end
    end
  end.flatten

# ["1A", "1B", "1C", "2A", "2B", "2C", "3A", "3B", "3C", "4A", "4B", "4C", "5A", "5B", "5C", "6A", "6B", "6C"]
```
**flatten** returns a [new](http://apidock.com/ruby/Array/new/class) array that is a one-dimensional flattening of this array (recursively). 

如果没有加上 flatten 将会产生如下的array

```ruby
# [["1A", "1B", "1C"], ["2A", "2B", "2C"], ["3A", "3B", "3C"], ["4A", "4B", "4C"], ["5A", "5B", "5C"], ["6A", "6B", "6C"]]
```
