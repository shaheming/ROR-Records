

注意 mix-in 与 module 的直接用法

```ruby
module HelloModule
  Version = "1.0"

  def hello(name)
    puts "Hello, #{name}."
  end

   module_function :hello
end
```

在 另一个文件中可以通过如下的方式来调用里的函数

```ruby
require './hellomodule'
HelloModule.hello("My name")
HelloModule::Version
```

而如果使用 include

```ruby
include HelloModule
p Version
hello("ddss")
```

可以通过这样的方式来直接调用函数。而 include 可以在 class 里面使用，为class 提供拓展的函数。如果函数使用 extend 来加入模块，这样可以为 class 提供类方法。