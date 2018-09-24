# ROR (12) | rails helper 函数写html 如何加入 属性

### 1 link_to helper 加入 id 与 class

```html
<%= link_to event, id: "an-id", class: "some-class" do  %>
  #bunch of stuff making up the partial.
<% end %>
```

[link_to 使用方法](http://api.rubyonrails.org/classes/ActionView/Helpers/UrlHelper.html#method-i-link_to)