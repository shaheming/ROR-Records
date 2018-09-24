# ROR (9) | link_to helper 

#### Data attributes

- confirm: 'question?' - This will allow the unobtrusive JavaScript driver to prompt with the question specified (in this case, the resulting text would be question?. If the user accepts, the link is processed normally, otherwise no action is taken.
- :disable_with - Value of this parameter will be used as the value for a disabled version of the submit button when the form is submitted. This feature is provided by the unobtrusive JavaScript driver.

#### Examples

Because it relies on url_for, link_to supports both older-style controller/action/id arguments and newer RESTful routes. Current Rails style favors RESTful routes whenever possible, so base your application on resources and use

```ruby
link_to "Profile", profile_path(@profile)
# => <a href="/profiles/1">Profile</a>
```

or the even pithier

```ruby
link_to "Profile", @profile
# => <a href="/profiles/1">Profile</a>

```

in place of the older more verbose, non-resource-oriented

```ruby
link_to "Profile", controller: "profiles", action: "show", id: @profile
# => <a href="/profiles/show/1">Profile</a>

```

Similarly,

```ruby
link_to "Profiles", profiles_path
# => <a href="/profiles">Profiles</a>

```

**is better than**

```ruby
link_to "Profiles", controller: "profiles"
# => <a href="/profiles">Profiles</a>
```

You can use a block as well if your link target is hard to fit into the name parameter. ERB example:

```ruby
<%= link_to(@profile) do %>
  <strong><%= @profile.name %></strong> -- <span>Check it out!</span>
<% end %>
# => <a href="/profiles/1">
       <strong>David</strong> -- <span>Check it out!</span>
     </a>
```

Classes and ids for CSS are easy to produce:

```ruby
link_to "Articles", articles_path, id: "news", class: "article"
# => <a href="/articles" class="article" id="news">Articles</a>

```

**Be careful when using the older argument style, as an extra literal hash is needed:**

```ruby
link_to "Articles", { controller: "articles" }, id: "news", class: "article"
# => <a href="/articles" class="article" id="news">Articles</a>
```

Leaving the hash off gives the wrong link:

```ruby
link_to "WRONG!", controller: "articles", id: "news", class: "article"
# => <a href="/articles/index/news?class=article">WRONG!</a>

```

link_to can also produce links with anchors or query strings:

```ruby
link_to "Comment wall", profile_path(@profile, anchor: "wall")
# => <a href="/profiles/1#wall">Comment wall</a>

link_to "Ruby on Rails search", controller: "searches", query: "ruby on rails"
# => <a href="/searches?query=ruby+on+rails">Ruby on Rails search</a>

link_to "Nonsense search", searches_path(foo: "bar", baz: "quux")
# => <a href="/searches?foo=bar&amp;baz=quux">Nonsense search</a>

```

The only option specific to link_to (:method) is used as follows:

```ruby
link_to("Destroy", "http://www.example.com", method: :delete)
# => <a href='http://www.example.com' rel="nofollow" data-method="delete">Destroy</a>
```

You can also use custom data attributes using the :data option:

```ruby
link_to "Visit Other Site", "http://www.rubyonrails.org/", data: { confirm: "Are you sure?" }
# => <a href="http://www.rubyonrails.org/" data-confirm="Are you sure?">Visit Other Site</a>
```
