# 深入 Rails 的 joins 与 includes

Joins 与 includes 是两种关联查询的方法，但是这两种用法常常别人混淆，我在这里试图说明一下 Joins 与 includes 的区别

### 1 Includes

在学习 rails 的性能算法优化的时候我们有提到需为了提高速度，我们需要解决 n + 1 query 的问题。而什么是 n + 1 query 呢？

n + 1 query 是指两个互相关联的tables，从一个table 中读出一批数据，然后通过循环去读取在另一个table中关联的数据。例如一个表是books， 另个表示author，现在读出

```ruby
@books = Book.all
@books.each do |book|
  puts book.name + book.author.name
end
```

因为在因为每次读取book.author.name 都会产生一条querry 这样会降低效率。因为`@books = Book.all` 会querry一次， 而每次循环也会querry 一次，假设会有N条循环，所以会叫叫做 N + 1 query。

为了解决这个问题将代码改为

```ruby
@books = Book.includes(:authors).all
@books.each do |book|
  puts book.name + book.author.name
end
```

可以将query 的次数减少为两次。 产生的 Sql 是两条 第一条是普通查询，第二条是命中`主键索引`的 `IN` 查询。但是这个有个缺点，当需要将数据库中的所有的数据都读出来的时候，就会出现特别大的内存占用，所以尽管通过这种方式比较快，但是不能够使用这种方式进行。这需要使用 find_in_batches 的方法，通过限制每次查询的大小。

```ruby
  User.find_in_batches(batch_size: 500) do |users|
    users.each do |user|
      do something
    end
  end
```

或者是更进一步，使用数据库中的 Transactions 来保证每次执行的完整性。

```ruby
User.find_in_batches(batch_size: 500) do |users|
    User.transaction do
      users.each do |user|
       # do something
      end
    end
  end
```

#### 

### 2 joins

使用 joins 的会执行数据库里面的 joins 操作，目的是精确的查找出的大批量的数据。 Include 是将所用的数据都捞出来，然后用 ruby 处理，这样会很慢。例如要找出去年第三季度的所有的商品的销售额，并根据分类计算总额。这样如果用 ruby 进行统计会效率非常低下。所以必须使用 SQL 精准的捞出想要的数据。