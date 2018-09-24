# ROR (6) |When to use self in Model? 

```ruby
class SomeData < ActiveRecord::Base
  def set_active_flag(val)
    self.active_flag = val
    self.save!
  end
end
```

When you're doing an action on the instance that's calling the method, you use self.

运用了self 是改变object的自己对的值