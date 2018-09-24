# ROR(11) | @ 变量的用法



在rails 里面我们常见如下写法的变量

```ruby
def index
	@products = Product.all
end
```

其区别是

`@variable`s are called instance variables in ruby. Which means you can access these variables in ANY METHOD inside the class. [Across all methods in the class]



ariables without the `@` symbol are called local variables, which means you can access these local variables within THAT DECLARED METHOD only. Limited to the local scope.'