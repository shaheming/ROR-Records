# ROR(19) |controller  与  model 的重构

为了简化自己的后端的代码，我们往往需要对自己的代码进行重构。今天主要讲解的是 controller 与model 的重构。

### 1 controller 的重构

#### 1.1 controller 继承 (？多用于区分不同的角色)

在我的jd store 的project 中的 admin 里面的 orders 与 product controller 里面的开头有几行代码

```ruby
before_action :authenticate_user!
before_action :require_is_admin
layout "admin"
```

为了简化代码，重新生成一个model 用来装这些东西。

```shell
rails g controller admin
```

在里面加入上述代码

并在 `admin/orders_controller.rb` `admin/product_controller.rb` 的第一行改为

```ruby
class Admin::ProductsController < AdminController
```

就可以继承上述三行代码。

#### 1.2 简化controller 里面的重复代码

最常见的是在 GRUD 里面查找资料的代码 

```ruby
@group = Group.find(params[:id])
```

可以用

```ruby
before_action :find_group, only: [:new]
```

来简化自己的代码。

在我的jstore 中在 `seats_controller.rb` 加入

```ruby
 before_action :find_product, only: [:index,:new,:create,:edit,:update]
 before_action :find_seat, only:[:edit,:update,:destroy,:select,:cancel]
```

在` products_controller.rb` 中加入

```ruby
before_action :find_product, only: [:show,:add_to_cart]
```

在 `orders_controller.rb`

```ruby
before_action :find_order, only:[:show,:pay_with_alipay,:pay_with_wechat,:apply_to_cancel]
```

在` cartItems_controller.rb`

加入

```ruby
before_action :find_caritem, only: [:destroy,:update]

###
def find_caritem
  @cart = current_cart
  @cart_item = @cart.cart_items.find_by(product_id: params[:id])
end

```

### 2 model 的重构

对于model 里面重复的代码叫做 `mixin` 将重复的代码放入一个model 中然后通过include 进行引用。

例如对于生成订单的token 可以用

```shell
rails g model concerns::tokenable.rb
```

在里面写

```ruby
module Tokenable

  extend ActiveSupport::Concern
  
  included do 
    before_create :generate_token 
  end
  
  
  def generate_token
    self.token = SecureRandom.uuid
  end
end
```

因为一般纯的 Ruby module 其实是不认识 before_create 的，所以 Rails 内部开发了一套工具 `ActiveSupport::Concern` 解决这件事。

并且在`order.rb` 中将相关代码删掉，改为

```ruby
include Tokenable
```