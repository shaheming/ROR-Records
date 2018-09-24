# Rails association

具体参考 rails tutorial 

注意事项，为了提高查询速度，需要将 relation ship 的表加入索引

```ruby
  create_table "relationships", force: :cascade do |t|
    t.integer  "follower_id"
    t.integer  "followed_id"
    t.datetime "created_at",  null: false
    t.datetime "updated_at",  null: false
    t.index ["followed_id"], name: "index_relationships_on_followed_id"
    t.index ["follower_id", "followed_id"], name:    "index_relationships_on_follower_id_and_followed_id", unique: true
    t.index ["follower_id"], name: "index_relationships_on_follower_id"
  end

```

关于 multi-column index 的用法可以看这个 [mysql 文档](https://dev.mysql.com/doc/refman/5.7/en/multiple-column-indexes.html)

association 的 最佳实践：

```ruby
# add
@user.active_relationships << friend
# delete
@user.active_relationships.delete(friend)
user.microposts.build(...)
# not 
Micropost.new(:user_id => @user.id,:content => content)
```
 Rails 支持使用 default_scope 指定默认排序方式;
```ruby
default_scope -> { order(created_at: :desc) }
```

通过这样的方式达到天然有序

 加入 dependent: :destroy 选项后，删除对象时也会把关联的对象删除;



