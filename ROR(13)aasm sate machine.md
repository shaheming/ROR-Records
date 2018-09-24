# ROR(13) | aasm sate machine

[AASM - Ruby state machines](https://github.com/aasm/aasm)



### ActiveRecord

AASM comes with support for ActiveRecord and allows automatic persisting of the object's state in the database.

```ruby
# default column: aasm_state
job = Job.new
job.run   # not saved
job.run!  # saved
```

If you want to make sure that the *AASM* column for storing the state is not directly assigned, configure *AASM* to not allow direct assignment, like this:



```ruby
class Job < ActiveRecord::Base
  include AASM

  aasm :no_direct_assignment => true do
    state :sleeping, :initial => true
    state :running

    event :run do
      transitions :from => :sleeping, :to => :running
    end
  end

end
```