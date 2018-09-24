git pull upstream master



application troller

```
config.time_zone = 'Beijing'
```

```
console do
  # this block is called only when running console,
  # so we can safely require pry here
  require "pry"
  config.console = Pry
end
```

debuge



ruby return 的大坑