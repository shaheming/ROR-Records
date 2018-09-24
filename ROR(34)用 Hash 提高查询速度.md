# ROR(34) | 用 Hash 提高查询速度

​设想一个场景你需要对一个人的通话记录进行处理，要判断它一段时间内的通话的对象是不是在他的通话通讯录里面，这个时候就是涉及循环比较。假设他的通讯录有 n 条，他的通讯记录有 m 条。如果按照一般的情况，使用两次循环的方式来执行这个算法，那么算法的时间复杂的为 O(n*m) , 如果当数据量大到一定的级别的时候，时间的小消耗就变得不可接受。这个我们可以用 hash 表的方式来解决这个问题，将 n 条通讯录先转换为 hash 存储，由于 hash 的时间复杂度为 O(1) 所以整个算法的时间复杂度为 O(n+m) 通过这样的方式能够大大地降低时间消耗。



用一个具体的例子来说明，设当 n = 200 ，m = 100 

```ruby
require 'benchmark'
#
Benchmark.bm(27) do |bm|
  bm.report("Generate array and hash") do
    200.times do |i|
      array << "1000#{i.to_s}"
      cnt_h["1000#{i.to_s}"] = 1
    end
  end
  bm.report('Use array') do

    for i in 1..100
      array.include?(1)
    end

  end

  bm.report('Use hash') do
    for i in 1..100
      cnt_h[1] == -1
    end
  end
end

# user     system      total        real
# Generate array and hash       0.000000   0.000000   0.000000 (  0.000309)
# Use array                     0.000000   0.000000   0.000000 (  0.003346)
# Use hash                      0.000000   0.000000   0.000000 (  0.000029)

```

结果是，后者时间消耗是前者的 1/15 如果前者使用前者算法处理数据的时间需要 15 天，而用后者，你只需要 1 天的时间就可以解决问题。