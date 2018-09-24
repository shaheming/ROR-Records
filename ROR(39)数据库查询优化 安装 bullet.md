在这篇文章中 [提取练习(3) | 数据库优化](http://shaheming-blog.logdown.com/posts/1550609)  在数据库查询中有三种方式会拖慢程序

1、N + 1 query

2、count cache

3、table scane 是指没有 将 key 建立索引。

rials 有一个gem 可以帮助我们检查自己的程序里面是不是用出现这些情况。

[bullet]( https://github.com/flyerhzm/bullet)

在安装这个gem的时候我遇见了一些坑，主要是notify的一些东西我搞不懂。

后面往下面翻了翻才解决了问题。

#### 1 、 [Install](https://github.com/flyerhzm/bullet#install)

#### 2、 按照拼图理论直接看[demo](https://github.com/flyerhzm/bullet#demo)(不要看config)

按照demo的教程执行到第8 步之后

![](http://ocs14bvbg.bkt.clouddn.com/17-3-12/79530728-file_1489325259078_720b.png)



⚠️ 这里出现了一个问题，按照readme里面应该是出现

```shell
 The request has N+1 queries as follows: 
```

但是我的shell 里面只是说道

```shell
 USE eager loading detected
```

之后进行到11 步他说的是 

```
user: mac
AVOID eager loading detected
  Post => [:comments]
  Remove from your finder: :includes => [:comments]
Call stack
```

与readme里面有所不同。



### 我掉入的坑

一开始的时候，我在装了这个gem之后，直接按照configuration里面的内容

```ruby
config.after_initialize do
  Bullet.enable = true
  Bullet.alert = true
  Bullet.bullet_logger = true
  Bullet.console = true
  Bullet.growl = true
  Bullet.xmpp = { :account  => 'bullets_account@jabber.org',
                  :password => 'bullets_password_for_jabber',
                  :receiver => 'your_account@jabber.org',
                  :show_online_status => true }
  Bullet.rails_logger = true
  Bullet.honeybadger = true
  Bullet.bugsnag = true
  Bullet.airbrake = true
  Bullet.rollbar = true
  Bullet.add_footer = true
  Bullet.stacktrace_includes = [ 'your_gem', 'your_middleware' ]
  Bullet.stacktrace_excludes = [ 'their_gem', 'their_middleware' ]
  Bullet.slack = { webhook_url: 'http://some.slack.url', channel: '#default', username: 'notifier' }
end
```

直接写config 结果报了一大堆的错误

```ruby
You must install the ruby-growl or the ruby_gntp gem to use Growl notification:

uninitialized constant UniformNotifier::AirbrakeNotifier::Airbrake

```

是因为这个些notifier  都没有装。

后面我按照demo里面的方法进行配置。

```ruby
  Bullet.enable = true
  Bullet.alert = true
  Bullet.bullet_logger = true
  Bullet.console = true
  Bullet.rails_logger = true
  Bullet.add_footer = true
```