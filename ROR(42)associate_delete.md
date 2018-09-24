Make sure your parent Model.rb has

```ruby
has_many :model, :dependent => :delete_all 
```

this makes sure all your data gets deleted once you delete a parent Model.

Like for instance you have Post.rb and Comment.rb

Post.rb

```ruby
has_many :Comments, :dependent => :delete_all
```

```ruby
class User < ActiveRecord::Base
  has_many :activities , dependent: :destroy, foreign_key: :trackable_id
end
```