Note offset should after limit

```ruby
offset = 0
limit = 1000

while(results)
  results = Model.find_by_sql("<your SQL here>  LIMIT #{limit} OFFSET #{offset}")
  offset += limit  
  # Do stuff here
end
```

