# cookie 与 session 

要知道 cookie 与 session 的是什么我们先要简要的了解他们为什么被发明出来。

我们知道我们的网络架构是基于 HTTP 协议。通过 HTTP 协议对网站进行访问的过程是由 request → response 的模式进行。由于 HTTP 协议本身所以一个无状态协议，所以服务器端根本不知道你是谁，从哪里来。

所以要想实现用户的登录就需要在每次请求时加入用户的账号与密码。但是如果让用户每次都输入账号密码显然是不可能的。于是工程师就想，有没有让浏览器在每次发出请求时自动的向服务器发送一些客户端的信息呢，例如用户的用户名和密码。

于是就发明了 cookie。

### 什么是 cookie

Cookies 是服务器在客户端上存储的一小段文本(小于 3K )并随每一个请求发送至同一个服务器。cookie的内容主要包括：名字，值，过期时间，路径和域。路径与域一起构成cookie的作用范围。

设置cookie 的主要步骤如下

```
1. 首先，我们假设当前域名下还是没有 cookie 的
2. 接下来，浏览器发送了一个请求给服务器（这个请求是还没带上 cookie 的）
3. 服务器设置 cookie 并发送给浏览器（当然也可以不设置）
4. 浏览器将 cookie 保存下来
5. 接下来，以后的每一次请求，都会带上这些 cookie，发送给服务器
```

这样我们就可以实现开不同的页面登录网站我们都能够知道是你在登录。用个比喻来说 cookie 就是一张门禁卡。

### 什么是 session

尽管使用 cookie 会比较方便，但是其有两个弊端：

- 因为 cookie 是明文存储，所以其中的所有数据都可被伪造
- cookie 的数据量太大的话会影响传输速度

为了解决这些问题，有人想我们是不是可以在服务器端也制作一个类似于 cookie 的东西用来储存用户的数据呢？如果每次用户发送请求只需要发送一个 session_id (类似于输入门禁号码) 服务器端就可以知道你是谁了，并且可以读出用户的数据。这就是 session。而这个 session_id 是通过 cookie 储存在用户端的，并在浏览器发起请求时发送给服务器。

session 可以储存在不同的地方

- 数据库
- cache
- 内存
- cookie

在 rails 中是通过如下的方法可以快速设置 session 并且，rails 默认是将 session 加密存放在 cookie中。

```ruby
module SessionsHelper
  # 登入指定的用户
  def log_in(user)
    session[:user_id] = user.id
  end
end
```



当然你也可以在`config/initializers/session_store.rb` 中修改session 的储存位置。

----



参考：

1. [cookie 和 session](https://github.com/alsotang/node-lessons/tree/master/lesson16)
2. [Cookie 和 Session 的使用简记](http://mertensming.github.io/2016/10/19/cookie-session/)
3. [Rails Sessions 是如何工作的？](http://grantcss.com/blog/2015/03/23/how-rails-sessions-work/)

