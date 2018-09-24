### Array liked magic



```ruby
>> %w[A B C].map { |char| char.downcase }
=> ["a", "b", "c"]
>> %w[A B C].map(&:downcase)
=> ["a", "b", "c"]
p  :string 
puts :string.inspect
('a'..'z').to_a.shuffle[0..16].map(&:upcase).join
user.following.map(&:id)
# 返回 following 的 id 的一个数组
```
```ruby
# 当 arr 非空是，返回 arr 的首字母
arr = arr && arr[0]
@current_user = @current_user || User.find(session[:id])
# 等价于
@current_user ||= User.find(session[:id])
```

