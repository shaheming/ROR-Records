这几天在做项目的代码的时候发现想要建立一个数据库，执行了 

```shell
$ rake db:create
$ rake db:migrate

=>
Mysql2::Error: Cannot add foreign key constraint: ALTER TABLE `loans` ADD CONSTRAINT `fk_rails_c15c911198` FOREIGN KEY (`user_id`)
```

发现总是报错，开始的时候一直不明就里，搞不懂错在什么地方，但是 如果是执行 `rake db:reset` 却可以创建数据库，因此我特地查了查到底是什么原因。

Note that running the `db:migrate` task also invokes the `db:schema:dump` task, which will update your `db/schema.rb` file to match the structure of your database.

分析 rails [的源码](https://github.com/rails/rails/blob/15ef55efb591e5379486ccf53dd3e13f416564f6/activerecord/lib/active_record/migration.rb) 你们发现 migrate 会将 migrate 里面的程序转化为 SQL 语句进行执行。在执行了 migrate 之后，还会修改 db/schema.rb 里面的 schema. 如下面的代码所示

```ruby
  desc "Migrate the database (options: VERSION=x, VERBOSE=false, SCOPE=blog)."
  task :migrate => [:environment, :load_config] do
    ActiveRecord::Tasks::DatabaseTasks.migrate
    db_namespace['_dump'].invoke #修改 db/schema.rb 
  end
```

而 `db:reset` 这个指令将执行下面两个命令

```shell
$ db:drop, db:setup
```

而 `db:setup` 又会执行下面的命令

```shell
$ db:create, db:schema:load, db:seed
```

`db:create` 会按照 `database.yml` 里面的配置进行创建数据库。`db:schema:load` 命令会将 `schema.rb`

里面的定义的 schema 写进数据库。`db:seed` 会执行  `seed.rb` 里面定义的schema 写入数据库。这样就会造成一个问题，如果你有新的 migrate 但是没有执行，修改你的数据库，他会提示你

```shell
You have 1 pending migration:
  20170818021228 CreateView
Run `rake db:migrate` to update your database then try again.
```

这时候需要使用 `rake db:migrate`



参考链接
------

1. [Difference between rake db:migrate db:reset and db:schema:load](https://stackoverflow.com/questions/10301794/difference-between-rake-dbmigrate-dbreset-and-dbschemaload) stack overflow
2. [database.rake](https://github.com/rails/rails/blob/15ef55efb591e5379486ccf53dd3e13f416564f6/activerecord/lib/active_record/railties/databases.rake) rails 源码

