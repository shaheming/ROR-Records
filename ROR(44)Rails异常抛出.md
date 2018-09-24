# ruby 异常抛出

### SyntaxError

使用语法错误

### NoMethodError

调用 object 没有的对象，例如:

```ruby
"123".a
#=> undefined method `a' for "123":String
```

### NameError

使用没有定义的变量，例如，调用没有赋值的变量 `a` 

```ruby
a
#=> NameError: undefined local variable or method `a' for main:Object
```

### ActiveRecord::RecordNotFound

当我们需要找到的记录不存在时，会抛出这个错误。

```ruby
Product.find(100000)
#ActiveRecord::RecordNotFound: Couldn't find Product with 'id'=100000
```

### ActiveRecord::ForbiddenAttributesError

params 没有经过验证，就会产生这个 error

```ruby
params = ActionController::Parameters.new(name: 'Bob')
Person.new(params)
# => ActiveModel::ForbiddenAttributesError

params.permit!
Person.new(params)
# => #<Person id: nil, name: "Bob">
```

### ActiveRecord::RecordInvalid

当想向数据库中存储数据是，所输入的属性不满足model 里面有 validates 条件，就会抛出这个错误。

```ruby
Order.create!(:total => 1)
#ActiveRecord::RecordInvalid: 验证失败: First name不能为空字符, Last name不能为空字符, Phonenumber不能为空字符
```

### ActionController::Routing Error

当所访问的网址在 routing 里面没有 match 的时候，会抛出这个错误。在 develop 环境与 production 环境下，两者不太一样。

### ActionController::UrlGenerationError

再用 helper 生成 url时出现错误 如果在 routes 里面设置的 url 对于的状态是 put 而你在 link_to 这个函数里面使用了 method::delete 这个就会报错。



### AbstractController::DoubleRenderError

在一个函数里面即使用了 render 也使用了 redirect_to 这两个方法。例如

```ruby
redirect_to new_user_session_path
render :new
#AbstractController::DoubleRenderError in WelcomeController#index

#Render and/or redirect were called multiple times in this action. Please note that you may only call render OR redirect, and at most once per action. Also note that neither redirect nor render terminate execution of the action, so if you want to exit an action after redirecting, you need to do something like "redirect_to(...) and return".
```