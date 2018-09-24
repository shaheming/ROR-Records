考虑有这种情况，你有大量的原始数据需要储，原始数据中包含一个字段`update_time` 你想根据这个字段中的月份动态分配到以 XXX2017XX 的表中 例如 example_table_201708 中。这里有几个难点需要处理。

1、如何快速大量且准确地生成所需要的model

2、注意，这里的表的名称都是单数(如果硬数字+s结尾会不好看)，本来 rails 默认的 model 的对象会去加上一个 s 去绑定其对应的名称，在这里应该怎么去让其绑定到一个为单数的表名？



首先我我们要使用 bash ，在project 的根目录下建立一个 `script/` 的目录。

建立一个`script/Makefile` 的文件。至于 Makefile 文件怎么写，请参考阮一峰的 [Make 命令教程](http://www.ruanyifeng.com/blog/2015/02/make.html)

 

在这里，你可以批量构建命令，例如

```makefile
create:
	rails g model Rong360RawRpBasics user:references loan:references \
	report_time:string \
	phone:string \
	phone_info:string \
	operator:string \
	operator_zh:string \
	phone_location:string \
	reg_time:string \
	operator_name:string \
	operator_id_card:string \
	name_check:string \
	id_card_check:string \
	cur_balance:string \
	monthly_avg_consumption:string   --no-fixture
	rails g model Rong360RawRpRiskContactsOverdue user:references loan:references \
	overdue_type:string \
	hit_cnt:string \
	seconds:string \
	cnt:string 
	rails g model Rong360RawRpRiskSingleOverdue user:references loan:references \
	no:string \
	hit_cnt:string 
	rails g model Rong360RawRpPortrait user:references loan:references \
	area:string \
	ratio:string \
	both_call_cnt:string \
	start_day:string \
	end_day:string \
	max_interval:string \
	max_detail:string \
	monthly_avg_days:string \
	night_monthly_avg_seconds:string \
	night_monthly_avg_seconds_ratio:string \
	night_monthly_avg_msg:string \
	night_monthly_avg_msg_ratio:string  --no-fixture
	rails g model Rong360RawRpPortraitSilentDetail user:references loan:references \
	date:string  --no-fixture
	rails g model Rong360RawRpPortraitSilent3Detail user:references loan:references \
	date:string  --no-fixture
```



你运用了 rails 的命令行工具来批量生成数据，具体怎么使用可以使用命令  `rails generate model -h` 查询帮助。

但是你会发现，你生成的 table 的名称是带有s的，例如会生成 example_table_201708s 为名字的table 的名字，但是这样的 table 名字非常的丑陋并不好看。为了能使生成的 table 的名称为不加 s 的模式，你需要在

`config/environment.rb` 里面设置如下的代码，

```
ActiveRecord::Base.pluralize_table_names = false
```

之后通过 `railgs g model` 产生出来的 model 的 table 的名字就不会有 s 了，但是当你欢欢喜喜地 migrate之后，想用测试一下自己的代码有没有问题。如果之前你的代码中有创建 table 例如 User 此时，你如果调用 `User.new` 就会报错。因为此时 Rails 回去寻找 user 这个table 但是事实上你创造的 table 是 users。

此时如果你将 `environment.rb` 刚才添加的代码注释掉之后，你会发现你新创建的 table 却没有办法运行了。这就造成了非常矛盾的一个结果。为了解决这个问题，你需要给 XXX2017XX  的 model 手动设置它所对应的表名字。

```ruby
 self.table_name = "XXXX"
```

你可以手动地将你每次新创建出来的 model 都手动加入这行字。或者是在调用这个 model 之前使用设置

`Model.table_name = Model.name.underscore`

而我是使用了一个脚本，将所以的生成的 model 中都加入我需要的代码。

用 `rails g task` 生成了一个 task ，通过 rake 来执行一个 task ，这样的好处是，这个里面的代码可以调用 rails 框架里面的东西。

```ruby
namespace :change_file do
  desc "TODO"
  task add_line_to_raw_msg: :environment do
    path = "#{Rails.root}/app/models/"
    add_lines(path,/^rong360_raw_msg.*$/,"RawMsg")
  end

  desc "TODO"
  task add_line_to_raw_call: :environment do
    path = "#{Rails.root}/app/models/"
    add_lines(path,/^rong360_raw_cal.*$/,"RawCall")
  end
  def add_lines(path,type,type_1)
    files = Dir.entries(path).select!{|f| f =~ type}
    files.each do |file_name|
      file_lines = File.readlines(path+file_name)
      File.open(path+file_name,'w+')  do |file|
        file.write(file_lines[0])
        file.write("  extend Rong360Util::#{type_1}\n")
        file.write("  self.table_name = \"#{file_name.gsub('.rb','')}\"\n")
        file_lines[1..-1].each{|l| file.write(l) }
      end
    end
  end
end
```

这个代码主要打开文件，插入一行代码。