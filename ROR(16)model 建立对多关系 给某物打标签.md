如果你有一个电影，你想给这个电影打上标签，并且能够通过标签找到一类电影的方式。

1、创建电影的model 为 product

2、创建group model(:name,:string)用来放类型，创建 groupship(:product_id,group_id) 用来建立联系

3、在`product.rb `

```ruby
has_many :groupships
has_many :groups, :through =>groupship
```

4、在`group.rb `

```ruby
has_many :groupships
has_many :products, :through =>groupship
```

5、在`groupship.rb `

```ruby
belongs_to :group
belongs_to :product
```

6、用select2 实现编辑时对标签进行多选

```html
<%= simple_form_for [:admin,@product] do |f|  %>
  <div class="group">
    <%= f.association :groups  %>
</div>
 <% end %>	
```
7、在permit 的时候要写成has 的形式

```ruby
:group_ids => []
```

并且要放在permit 的最后。