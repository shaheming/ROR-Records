在数据库的操作当做往往有许多对多的映射关系，例如一个学生可以参加多个社团，一个社团可以拥有多名学生。为了实现这一功能，我们需要一个中间的表来帮助我们实现这个功能。

现在有两个表 **students** 与 **clubs** ，为了实现多对多的关系我们需要一个表 **clubrelationships** 有两栏(student_id, club_id)

```shell
$ rails g model clubship, student_id:integer, club_id:integer
```

在 clubship.rb` 中写

```ruby
belongs_to :student
belongs_to :club
```

之后要修改`student.rb` `club.rb`，为了能够直接调用student就能够查看学生参加的社团。

这里有两种写法：

### 1、用 `student.clubs` 来查看

因为clubs 与 表单(clubs)的名字相同。rails 根据默认的方式来查询。

在`student.rb`

```ruby
has_many :clubships
has_many :clubs,:through=>:clubships
```

在`club.rb`

```ruby
has_many :clubships
has_many :students,:through=>:clubships
```

这样可以通过find 将某一个 student or club 捞出来之后，可以`student.clubs` `club.students` 来分别查看他们的关联信息。

### 2、用 `student.participated_clubs` （别名）来查看

因为clubs 与 表单(clubs)的名字不相同。需要加入配置的参数。

在`student.rb`

```ruby
has_many :clubships
has_many :student.participated_clubs,:through=>:clubships,:source=>:clubs
```

在`club.rb`

```ruby
has_many :clubships
has_many :members,:through=>:clubships,:source=>:students
```

这样可以通过find 将某一个 student or club 捞出来之后，可以`student.clubs` `club.students` 来分别查看他们的关联信息。