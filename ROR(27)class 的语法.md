ruby 的类的定义的语法

```ruby
class Person

    def initialize(name)
        @name = name
        
    end

    def say(word)
        puts "#{word}, #{@name}"
        str = "AAAAA"
        p str
    end
end


p1 = Person.new("Heming")
p1.say("Hello") # 输出 Hello, Heming

class Person
  
  @@name = “Heming”

    def self.say
        puts @@name
    end
end

Person.say # 输出 Heming
```

- 类是大写开头，是一种常数
- `@` 开头的变量是 （intance variable）
- def 定义 instance 的方法
- 小写的字母的变量是局部变量
- `@@` 是 class variable 
- `def self.` (class method) def 开头的是 （instance method)

### 如何读取对象变量？

对于以 @ 开头的 instance variable 与 @@ 开头的 class variable  都是封装在类的内部，类的外部无法存取。都需要通过定义方法才能得到（类似于 C++ 的prive 里面的变量）

例如：

```ruby
class Person
    def initialize(name)
      @name = name
    end
end

p = Person.new('Heming')
p.name
# NoMethodError 
p.name='peny'
# NoMethodError 
```

需定义存取的方法，来直接调用，这个有点像  C++  用 private 里面的变量，不能通过外部直接访问。

```ruby
class Person

   def initialize(name)
    @name = name
   end

   def name
     @name
   end

   def name=(name)
     @name = name
   end

end

p = Person.new('Heming')
p.name
=> "ihower"
p.name = "peny"
=> "peny"

# 或者用 attr_accessor :name 来替代这两个用法
```

### 变量的作用域

在 rails 的 active record 的class 里面调用与 table 的 column 同样名字的变量就可以直接访问这个属性，例如

```
user.name
user.email
```

在 class 的定义的方法中，我们可以通过如下方法进行对这些变量的访问

```ruby
def downcase_email
  email.downcase! 
  # or self.email.downcase! 
end
```

那这里为啥会有这两种写法呢？

这是为了处理变量作用域的问题。在这个地方，我们可以定义局部变量

```ruby
#假设 user.email = "user@example.Com"
def downcase_email(email)
  email.downcase! 
  #self.email.downcase! 
end
如果我们调用
user.downcase_email("A@A.com")
#=> "a@a.com"
#而如果我们这样写
def downcase_email(email)
  self.email.downcase! 
  email.downcase! 
end
user.downcase_email("A@A.com")
#=> "a@a.com"
user.email
#=> "user@example.com"
```

这里的 self 是指带的是 email 是实例变量，而不是局部变量

### public/protected/private 方法

The difference between protected and private is subtle. If a method is protected, it may be called by any instance of the defining class or its subclasses. If a method is private, it may be called only within the context of the calling object---it is never possible to access another object instance's private methods directly, even if the object is of the same class as the caller. For protected methods, they are accessible from objects of the same class (or children).

protect 的方法子类可以继承，而private的方法子类无法继承。

