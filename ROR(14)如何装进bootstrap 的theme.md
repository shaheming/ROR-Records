### 如何装进bootstrap 的theme

1、下载相关的boostrap 模板

2、构造自己的layout（为了让不同的layout加载不同的js 与 css）`layout.html.erb`

3、将theme 里面的js 与 css 放在 vender 里面

4、将在asset 里面建立自己layout 的 js 与css 并且 要记住 require 与 import 相关的css 与 require 

5、将 config/asset.rb 中 将中反注释

```ruby
Rails.application.config.assets.precompile += %w( blog.js blog.scss )

```

6、在 layout 里面加入

```html
<%= stylesheet_link_tag    'blog', media: 'all', 'data-turbolinks-track': 'reload' %>
<%= javascript_include_tag 'blog', 'data-turbolinks-track': 'reload' %>
```

中的blog 换为自己的js 与css 的路径。



套用主题失败的原因 是没有在layouyt css 里面加入 jqury

基础 config css