# ror 深入探究购物车的写法



```
class ApplicationController < ActionController::Base
  protect_from_forgery with: :exception
  def admin_required
    if !current_user.admin?
      redirect_to "/", alert: "You are not admin."
    end
  end 
  
  helper_method :current_cart
  def current_cart
    @current_cart ||= find_cart
  end
  
  private

  def find_cart
    cart = Cart.find_by(id: session[:cart_id])
    if cart.blank?
      cart = Cart.create
    end
    session[:cart_id] = cart.id
    return cart
  end
end
```



rails helper 的写法可以在view 与 controller 里面进行调用。

```
  class ApplicationController < ActionController::Base
    helper_method :current_user, :logged_in?

    def current_user
      @current_user ||= User.find_by_id(session[:user])
    end

     def logged_in?
       current_user != nil
     end
  end
```

rails 的子类的 private 可以继承父亲类的provate



 cart_items.destroy_all 删除所有的

method 常常写错，就会导致出现 notfind show



product_list 与 cart item 的性质其实是一样的，但是只是 product_list 里面的数据是写死的。product_list 与 cart_item 的使之上是一种分栏的设计思路。



