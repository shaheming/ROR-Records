### HTTP part 的坑



```ruby
res = HTTParty.post("#{SEVER_URL}/retraining",
  :body => data.to_json,
  :headers =>
    {
      "Content-Type" => "application/json"
      })
```

在写 header 的时候错误将 

```ruby
 :headers => {"Content-Type" => "application/json"}
```

写成了

```ruby
 :headers => {"Content-Type" : "application/json"}
```

导致了 post 出去的 body 不在了。



```ruby
{ 'Content-Type' => 'application/json' }
=>{"Content-Type"=>"application/json"}

 { 'Content-Type' => 'application/json' }.keys.first.class
=> String

{ 'Content-Type': 'application/json' }
=> {:"Content-Type"=>"application/json"}
 { 'Content-Type': 'application/json' }.keys[0].class
=> Symbol
```

### 