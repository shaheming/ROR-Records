There are two way to add index when create a table

### 1 when the setting the attribute

```ruby
t.string,  :index=> true
```

### 2 at the end to the claim

```ruby
 add_index :cities, :juhe_id
```

