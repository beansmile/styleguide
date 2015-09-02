# 集合

* 倾向数组及哈希的字面表示法(除非你需要传递参数到它们的构造函数中)。

    ```Ruby
    # bad
    arr = Array.new
    hash = Hash.new

    # good
    arr = []
    hash = {}
    ```

* 当你需要元素为单词（没有空格和特殊符号）的数组的时候总是使用 `%w` 的方式来定义字符串数组。应用这条规则仅仅在两个或多个数组。

    ```Ruby
    # bad
    STATES = ['draft', 'open', 'closed']

    # good
    STATES = %w(draft open closed)
    ```

* 当你需要一个符号的数组（并且不需要保持 Ruby 1.9 兼容性）时，使用 `%i`。仅当数组只有两个及以上元素时才应用这个规则。

    ```Ruby
    # bad
    STATES = [:draft, :open, :closed]

    # good
    STATES = %i(draft open closed)
    ```

* 避免在 `Array` 或者 `Hash` 的最后一项后面出现逗号，特别是当这些条目不在一行。

    ```Ruby
    # bad - easier to move/add/remove items, but still not preferred
    VALUES = [
               1001,
               2020,
               3333,
             ]

    # bad
    VALUES = [1001, 2020, 3333, ]

    # good
    VALUES = [1001, 2020, 3333]
    ```

* 避免在数组中创造巨大的间隔。

    ```Ruby
    arr = []
    arr[100] = 1 # now you have an array with lots of nils
    ```

* 当访问一个数组的第一个或者最后一个元素，倾向使用 `first` 或 `last` 而不是 `[0]` 或 `[-1]`。

* 如果要确保元素唯一, 则使用 `Set` 代替 `Array` .`Set` 更适合于无顺序的, 并且元素唯一的集合, 集合具有类似于数组一致性操作以及哈希的快速查找.

* 尽可能使用符号代替字符串作为哈希键.

    ```Ruby
    # bad
    hash = { 'one' => 1, 'two' => 2, 'three' => 3 }

    # good
    hash = { one: 1, two: 2, three: 3 }
    ```

* 避免使用易变对象作为哈希键。
* 优先使用 1.9 的新哈希语法当你的哈希键是符号。

    ```Ruby
    # bad
    hash = { :one => 1, :two => 2, :three => 3 }

    # good
    hash = { one: 1, two: 2, three: 3 }
    ```

* 在相同的 hash 字面量中不要混合 Ruby 1.9 hash 语法和箭头形式的 hash。当你
  得到的 keys 不是符号的时候转换为箭头形式的语法。

    ```Ruby
    # bad
    { a: 1, 'b' => 2 }

    # good
    { :a => 1, 'b' => 2 }

* 用 `Hash#key?` 不用 `Hash#has_key?` 以及用 `Hash#value?`, 不用 `Hash#has_value?` Matz [提到过](http://blade.nagaokaut.ac.jp/cgi-bin/scat.rb/ruby/ruby-core/43765) 长的形式在考虑被弃用。

    ```Ruby
    # bad
    hash.has_key?(:test)
    hash.has_value?(value)

    # good
    hash.key?(:test)
    hash.value?(value)
    ```

* 在处理应该存在的哈希键时，使用 `fetch`。

    ```Ruby
    heroes = { batman: 'Bruce Wayne', superman: 'Clark Kent' }
    # bad - if we make a mistake we might not spot it right away
    heroes[:batman] # => "Bruce Wayne"
    heroes[:supermann] # => nil

    # good - fetch raises a KeyError making the problem obvious
    heroes.fetch(:supermann)
    ```

* 在使用 `fetch` 时，使用第二个参数设置默认值而不是使用自定义的逻辑。

   ```Ruby
   batman = { name: 'Bruce Wayne', is_evil: false }

   # bad - if we just use || operator with falsy value we won't get the expected result
   batman[:is_evil] || true # => true

   # good - fetch work correctly with falsy values
   batman.fetch(:is_evil, true) # => false
   ```

* 尽量用 `fetch` 加区块而不是直接设定默认值。

   ```Ruby
   batman = { name: 'Bruce Wayne' }

   # bad - if we use the default value, we eager evaluate it
   # so it can slow the program down if done multiple times
   batman.fetch(:powers, get_batman_powers) # get_batman_powers is an expensive call

   # good - blocks are lazy evaluated, so only triggered in case of KeyError exception
   batman.fetch(:powers) { get_batman_powers }
   ```

* 当你需要从一个 hash 连续的取回一系列的值的时候使用 `Hash#values_at`。

    ```Ruby
    # bad
    email = data['email']
    nickname = data['nickname']

    # good
    email, username = data.values_at('email', 'nickname')
    ```

* 记住, 在 Ruby1.9 中, 哈希的表现不再是无序的. (译者注: Ruby1.9 将会记住元素插入的序列)
* 当遍历一个集合的同时, 不要修改这个集合。

