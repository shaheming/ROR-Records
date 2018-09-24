### 1 让 nil 变成 false

```ruby
!!nil = false
```

### 2 YAML 文件导入

```ruby
YAML.load_file "config/database.yml"
opts = YAML.load(ERB.new(IO.read(cfile)).result) || opts #通过这种方式可以将 ERB 模板里面的变量替换掉。
```

### 3 更简洁的创建数组的方法

```ruby
 (opts[:queues] ||= []) << q
```

### 4 查看是否定义了某个变量

```ruby
defined? SS
```

### 5 :: 开头的变量的使用方法

```ruby
class MyClass #1
end

module MyNameSpace
  class MyClass #2
  end

  def foo # Creates an instance of MyClass #1
    ::MyClass.new # If I left out the ::, it would refer to
                  # MyNameSpace::MyClass instead.
  end
end
#双冒号代指来自顶级命名空间
```

### 6 如何不断传进函数

```ruby
#被传入的函数
def start_heartbeat
  while true
    heartbeat
    sleep 5
  end
  Sidekiq.logger.info("Heartbeat stopping...")
end

@thread = safe_thread("heartbeat", &method(:start_heartbeat)) #调用入口

#入口1 如何创建多线程
def safe_thread(name, &block)
  Thread.new do
    Thread.current['sidekiq_label'.freeze] = name
    watchdog(name, &block)
  end
end

#入口2
def watchdog(last_words)
  yield
rescue Exception => ex
  handle_exception(ex, { context: last_words })
  raise ex
end
```

### 7 对复杂变量的赋值方法

```ruby
@data ||=begin
  {
      'hostname' => hostname,
      'started_at' => Time.now.to_f,
      'pid' => $$,
      'tag' => @options[:tag] || '',
      'concurrency' => @options[:concurrency],
      'queues' => @options[:queues].uniq,
      'labels' => @options[:labels],
      'identity' => identity,
  }
rescue
  23
end #一种写法
```

### 8 让类的实例在运行时绑定上一些方法

```ruby
 results = Sidekiq::CLI::PROCTITLES.map {|x| x.(self, to_data) } #将一些 block 塞入 data
 
 PROCTITLES = [
    proc { 'sidekiq'.freeze },
    proc { Sidekiq::VERSION },
    proc { |me, data| data['tag'] },
    proc { |me, data| "[#{Processor::WORKER_STATE.size} of #{data['concurrency']} busy]" },
    proc { |me, data| "stopping" if me.stopping? },
]
#？ 心跳代码有啥用？
```

