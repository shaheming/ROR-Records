# ROR (5) |Binding a Form to an Object



```ruby
<%= form_for @article, url: {action: "create"}, html: {class: "nifty_form"} do |f| %>
  <%= f.text_field :title %>
  <%= f.text_area :body, size: "60x12" %>
  <%= f.submit "Create" %>
<% end %>
```

- `@article` is the actual object being edited.
- There is a single hash of options. Routing options are passed in the `:url` hash, HTML options are passed in the `:html` hash. Also you can provide a `:namespace` option for your form to ensure uniqueness of id attributes on form elements. The namespace attribute will be prefixed with underscore on the generated HTML id.
- The `form_for` method yields a **form builder** object (the `f` variable).
- Methods to create form controls are called **on** the form builder object `f`.



####  Dealing with Namespaces

```ruby
form_for [:admin, @article]
```
#### Difference between `:url`, `:action` and `:method`

**:url**

If you want to submit your form for any particular controller, any particular action and want to pass some extra parameter (use action that define in controller that you pass on controller )

*for example*

```
<%= form_for @post, :url => {:controller => "your-controller-name", :action => "your-action-name"} do |f| %>
```

In the above code the form is submitted to that controller(that you pass on url) and goto that (you pass on action) action. it will take defaults to the current action.

now suppose you want to pass extra parameter then **for example**

```
form_for @post, :url => { :action => :update, :type => @type, :this => @currently_editing } do |f| ...
```

you can pass extra parameter like `:type => @type`

so `:url` is The URL the form is submitted to. It takes the same fields you pass to url_for or link_to. In particular you may pass here a named route directly as well.

------

**:action**

```
 form_for @post, :url => { :action => :update, :type => @type, :this => @currently_editing } do |f| ...
```

In the above example we pass `:action` if we want to submit the form in different action then we pass `:action` and `your-action-name` the form is post to that action

------

**:method**

method is used for which method you want to pass for that action. There are several methods like `put`,`post`,`get` ...

*for example*

```
form_for @post, :url => post_path(@post), :method => :put, ....
```

In the above `form_for` we pass `:method => :put` when this form is submit it will use `put`method



**by default when submitted, will POST to the current page.** 