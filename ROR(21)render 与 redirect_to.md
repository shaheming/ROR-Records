在进行 web api 的课程的时候，我想要实现一个功能，即 form 写一个 search 的功能，之后调用 web api ，然后能够返回 search 的结。

于是我用了 simple_form 。

### step 1 建立表单

```shell
$ rails g model recipe name:string ingredient:string
```

### step 2 建立表单

在 controller 里面写

```ruby
def index
	@recipe = Recipe.new
end
```

在 view 里面写

```html
  <%= simple_form_for @recipe, :url => search_recipes_path, :method => :get do |f| %>
  <div class="form-group">
    <%= f.input :name %>
    <%= f.input :ingredients %>
  </div>
  <div class="form-action">
    <%= f.submit "Search", disable_with: 'Submiting...', class: " btn btn-primary" %>
  </div>
  <% end %>
```

改写路由

```ruby
  resources :recipes do
    collection do
      get :search
    end
  end
```

**ps: search 的功能要写在 collection 里面。** 这样产生的url 是 /recipes/search

在 controller 里面写

```ruby
def search
  #do something
end
```

这时，我以为使用了 get 动作发出 request 之后，页面会继续导向在 index 里。但是事实不是这样，页面出现了报错。

![](http://i1.piimg.com/567571/c99b919513bb976b.png)

然后我想，是不是需要用 redirect_to 将页面倒回到index 页面呢？

但是我发现当 redirect_to index 之后， 我在 search 中获得的变量全部消失了。又出现报错。

后来折腾了半天我才想起来，解决如上图所示的报错需要新建一个 view 即 search.html.erb。 最终我将问题解决了。

但是又没有办法在 /recipes 的路由下面看到搜索结果呢？

后来我想到了用 render 。 这又有一个问题 就是 render 与 redirect 的方法非常相似，到底有啥区别呢？

> **Calling render** will create a full response that is sent back to the browser.
>
> **Redirect** is concerned about telling the browser it needs to make a new request to a different location.



在 search 的方法中调用 redirect ，浏览器会发出两个请求

1. get /recipes/search status :302
2. get /recipes 

最终的url 是 /recipes

 在 search 的方法中调用 render ，浏览器会发出一个请求

1. get /recipes/search 

#### 如何选择  render 与 redirect_to 

当不需要刷新时用 render ，当需要刷新时用 redirect_to。