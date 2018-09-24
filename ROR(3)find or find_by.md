# ROR (3) | find or find_by

-  `find`  is used when you need to retrieve a single record by id.

```ruby
User.find(id)
```

- Favor the use of `find_by` over `where` and `find_by_attribute` when you need to retrieve a single record by some attributes.

```ruby
# bad
User.where(first_name: 'Bruce', last_name: 'Wayne').first

# bad
User.find_by_first_name_and_last_name('Bruce', 'Wayne')

# good
User.find_by(first_name: 'Bruce', last_name: 'Wayne')
```

这里的 `find_by_first_name_and_last_name` 使用 元变程的方法实现的`define_method`.

the more info you can see [https://github.com/bbatsov/rails-style-guide#find_by](https://github.com/bbatsov/rails-style-guide#find_by)

里面提到了几条最佳实践

- Avoid string interpolation in queries, as it will make your code susceptible to SQL injection attacks.

```ruby
# bad - param will be interpolated unescaped
Client.where("orders_count = #{params[:orders]}")

# good - param will be properly escaped
Client.where('orders_count = ?', params[:orders])
```

- Consider using named placeholders instead of positional placeholders when you have more than 1 placeholder in your query.

```ruby
# okish
Client.where(
  'created_at >= ? AND created_at <= ?',
  params[:start_date], params[:end_date]
)

# good
Client.where(
  'created_at >= :start_date AND created_at <= :end_date',
  start_date: params[:start_date], end_date: params[:end_date]
)
```
-  Favor the use of `find` over `where` when you need to retrieve a single record by id.

```ruby
# bad
User.where(id: id).take

# good
User.find(id)
```
- Favor the use of `find_by` over `where` and `find_by_attribute` when you need to retrieve a single record by some attributes. 

```ruby
# bad
User.where(first_name: 'Bruce', last_name: 'Wayne').first

# bad
User.find_by_first_name_and_last_name('Bruce', 'Wayne')

# good
User.find_by(first_name: 'Bruce', last_name: 'Wayne')
```

- Favor the use of `where.not` over SQL.

```ruby
# bad
User.where("id != ?", id)

# good
User.where.not(id: id)
```