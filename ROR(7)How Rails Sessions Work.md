# ROR (8) |What do helper and helper_method do?

Declare a controller method as a helper. For example, the following makes the `current_user` and `logged_in?` controller methods available to the view:

```ruby
class ApplicationController < ActionController::Base
  helper_method :current_user, :logged_in?

  def current_user
    @current_user ||= User.find_by(id: session[:user])
  end

  def logged_in?
    current_user != nil
  end
end
```

In a view:

```ruby
<% if logged_in? -%>Welcome, <%= current_user.name %><% end -%>](http://www.justinweiss.com/articles/how-rails-sessions-work/)
```

