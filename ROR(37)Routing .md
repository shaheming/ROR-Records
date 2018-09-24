# 阅读 Rails Routing 

Rails Routing 主要有两部分的功能

1、将 URL 链接到 controller 里面的方法

2、通过 Helper 生成 url

具体又可以分为两个部分

- resourceful 的 route
- non resourceful 的 route

resouceful 的 route 可以自己定制

### 实用功能

#### 1 shallow nest 

```ruby
resources :articles do
  resources :comments, only: [:index, :new, :create]
end
resources :comments, only: [:show, :edit, :update, :destroy]
```

One way to avoid deep nesting (as recommended above) is to generate the collection actions scoped under the parent, so as to get a sense of the hierarchy, but to not nest the member actions. In other words, to only build routes with the minimal amount of information to uniquely identify the resource

| HTTP Verb | Path                                     | Controller#Action | Named Helper             |
| --------- | ---------------------------------------- | ----------------- | ------------------------ |
| GET       | /articles/:article_id/comments(.:format) | comments#index    | article_comments_path    |
| POST      | /articles/:article_id/comments(.:format) | comments#create   | article_comments_path    |
| GET       | /articles/:article_id/comments/new(.:format) | comments#new      | new_article_comment_path |
| GET       | /comments/:id/edit(.:format)             | comments#edit     | edit_comment_path        |
| GET       | /comments/:id(.:format)                  | comments#show     | comment_path             |
| PATCH/PUT | /comments/:id(.:format)                  | comments#update   | comment_path             |
| DELETE    | /comments/:id(.:format)                  | comments#destroy  | comment_path             |

#### 2 对 url 进行限制

- 限制 id ，限制 http 的 veb ，限制 format
- 增加命名空间
- 用 match 来重新定向路由
- ​

