![](http://ocs14bvbg.bkt.clouddn.com/17-8-18/78615517.jpg)

现在有这样的状况我们有一个表 OnlineOrder 与 Order 总表，我们希望能找出全部实例 Order 表中的是在线下单的并在查看 Name 属性。那么问题来了，我们应该怎么做呢？

这里的办法是使用 Join 这个方法.

首先在 model 调用方法

```ruby
class OnlineOrder < ApplicationRecord
 
end
class Order < ApplicationRecord
   has_one :onlie_order
end
```

之后使用

```ruby
Order.joins(:onlie_order)
```

就可可以将其全部找出来，其应的 SQL 语句是：

```shell
SELECT "orders".* FROM "orders" INNER JOIN "onlineorder" ON "onlineorder"."order_id" = "orders"."id"
```

这里遇见一个疑问，has_one 与 belongs_to 的区别？

The `has_one` association creates a one-to-one match with another model. In database terms, this association says that the other class contains the foreign key. If this class **contains** the foreign key, then you should use `belongs_to` instead.