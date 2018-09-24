## ROR (2) | What is the purpose of “!” and “?” at the end of method names?

- **Methods ending in ! perform some permanent or potentially dangerous change**; for example:`Enumerable#sort` returns a sorted version of the object while `Enumerable#sort!` sorts it in place.In Rails, `ActiveRecord::Base#save` returns false if saving failed, while `ActiveRecord::Base#save!` raises an exception.`Kernel::exit` causes a script to exit, while `Kernel::exit!` does so immediately, bypassing any exit handlers.



- **Methods ending in ? return a boolean**, which makes the code flow even more intuitively like a sentence — `if number.zero?` reads like "if the number is zero", but `if number.zero`just looks weird.