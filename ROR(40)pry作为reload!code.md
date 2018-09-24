# pry 作为 console 无法进行 reload! code

参考 issue

在 oject 的目录下简历 `.pryrc`

```ruby
# .pryrc checked into my project root
require 'rails/console/app'
include Rails::ConsoleMethods
```
https://github.com/rweng/pry-rails/issues/9