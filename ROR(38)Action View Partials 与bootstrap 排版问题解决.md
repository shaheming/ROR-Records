# 如何用rails 的Action View Partials 简化代码



还记得在 [Rails101 9-4](https://fullstack.xinshengdaxue.com/posts/92) 中有讲用

```ruby
 <%= render :partial => "group_item", :collection => @groups, :as => :group %>
```

并且重新新建一个 `_group_item.html.erb` 来新拆分与重构一些这部分代码。

```ruby
  <tbody>
      <% @groups.each do |group| %>
        <tr>
          <td> # </td>
          <td> <%= link_to(group.title, group_path(group)) %> </td>
          <td> <%= group.description %> </td>
          <td> <%= group.posts.count %> </td>
          <td> <%= group.updated_at %> </td>
        </tr>
      <% end %>
   </tbody>
```

同样，我们可以将这个部分的代码运用到我们job-listing中的`jobs/index.html.erb` 的代码。

新建一个`app/views/jobs/_job.html.erb` 文件，里面将每一个子模块的html语句放入，并且在

`app/views/jobs/index.html.erb` 中加入以下代码

```ruby
 <%= render :partial => "job", :collection => @jobs, :as => :job %>
```
就可以实现代码简化的功能。

同样的用这样的方式我们可以用于简化 [如何在navbar上添加搜索栏并实现搜索结果](http://forum.qzy.camp/t/navbar/486)教程中提到的 touch app/views/jobs/search.html.erb 的文件，并且可以重用`app/views/jobs/_job.html.erb`文件。



于是我结合boostrap 的Grid system 实现了如下的面板式的呈现方式。![](http://ocs14bvbg.bkt.clouddn.com/17-1-28/35986378-file_1485592700217_112f6.png)



使用Grid system 必须满足如下的html 的代码结构

```html
<div class="container">
  <div class="row">
    <div class="col-md-4">col-sm-4</div>
    <div class="col-md-4">col-sm-4</div>
    <div class="col-md-4">col-sm-4</div>
  </div>
</div>
```

对于每一列(row)来说 `col-md-4` 的最后的数字的和要为12，就以上述代码为例 4+4+4 =12 。但是，用之前的方法使用的partial 在我们无法自动满足当循环了三个job后自动加入 `<div class="row">` 标签。

于是我的代代码的最终呈现形式就变为。



```html
<div class="container">
  <div class="row">
    <div class="col-md-4">col-sm-4</div>
    <div class="col-md-4">col-sm-4</div>
    <div class="col-md-4">col-sm-4</div>
    <div class="col-md-4">col-sm-4</div>
    <div class="col-md-4">col-sm-4</div>
    <div class="col-md-4">col-sm-4</div>
    <div class="col-md-4">col-sm-4</div>
    <div class="col-md-4">col-sm-4</div>
    <div class="col-md-4">col-sm-4</div>
    ...
  </div>
</div>
```
这样是不符合 Grid system  的规范。

如果你的每一个  `<div class="col-md-4">col-sm-4</div>` 里的子模块的大小一样，这样排版还不会出错，但是当我们的  `<div class="col-md-4">col-sm-4</div>`  里包裹的内容大小不同是，排版就出现了如下的混乱状况。

![](http://ocs14bvbg.bkt.clouddn.com/17-1-28/40128069-file_1485593266735_8eee.png)



后来我想到了一个方式就是在html语句中嵌入一个if判断语句来解决自动加入  `<div class="row">`标签的方法。

```html
<%  count = 0 %>
<% @jobs.each do |job| %>
<% if count == 0 %>
<div class="row">
	<% end %>
	<% count+=1 %>
	<%= render partial: "job", locals: {job:job} %>
	<% if count == 3 %>
</div>
<% count = 0 %>
<% end %>
<% end %>
```

其中`locals: {job:job} `是用来向`app/views/jobs/_job.html.erb` 中的变量`job` 传递的参数。[大家可以参考rails 的官方说明文档。](http://api.rubyonrails.org/classes/ActionView/PartialRenderer.html)

这个语句最终实现如下的html。

```html
<div class="container">
	<div class="row">
		<div class="col-md-4">col-sm-4</div>
		<div class="col-md-4">col-sm-4</div>
		<div class="col-md-4">col-sm-4</div>
	</div>
	<div class="row">
		<div class="col-md-4">col-sm-4</div>
		<div class="col-md-4">col-sm-4</div>
		<div class="col-md-4">col-sm-4</div>
	</div>
	...
</div>
```

最终实现第一张图中的效果。

![](http://ocs14bvbg.bkt.clouddn.com/17-1-28/35986378-file_1485592700217_112f6.png)



-----

这是我的作品https://fullstack.xinshengdaxue.com/works/105，欢迎大家来参观投票。