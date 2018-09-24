```ruby
@installments = Installments.where('id < 100')
@installments.to_id
#=> [1,3,4..,5]
class Installment < ApplicationRecord
  def self.to_id
    all.map(&:id)
  end
end
```

