# 语法

* 使用 `::` 引用常量（包括类和模块）和构造器 (比如 `Array()` 或者 `Nokogiri::HTML()`)。
  永远不要使用 `::` 来调用方法。

    ```Ruby
    # bad
    SomeClass::some_method
    some_object::some_method

    # good
    SomeClass.some_method
    some_object.some_method
    SomeModule::SomeClass::SOME_CONST
    SomeModule::SomeClass()
    ```

* 使用括号将`def`的参数括起来。当方法不接收任何参数的时候忽略括号。

     ```Ruby
     # bad
     def some_method()
       # body omitted
     end

     # good
     def some_method
       # body omitted
     end

     # bad
     def some_method_with_arguments arg1, arg2
       # body omitted
     end

     # good
     def some_method_with_arguments(arg1, arg2)
       # body omitted
     end
     ```

* 从来不要使用 `for`， 除非你知道使用它的准确原因。大多数时候迭代器都可以用来替`for`。`for` 是由一组 `each` 实现的 (因此你正间接添加了一级)，但是有一个小道道 - `for`并不包含一个新的 scope (不像 `each`)并且在它的块中定义的变量在外面也是可以访问的。

    ```Ruby
    arr = [1, 2, 3]

    # bad
    for elem in arr do
      puts elem
    end

    # note that elem is accessible outside of the for loop
    elem #=> 3

    # good
    arr.each { |elem| puts elem }

    # elem is not accessible outside each's block
    elem #=> NameError: undefined local variable or method `elem'
    ```

* 在多行的 `if/unless` 中坚决不要使用 `then`。

    ```Ruby
    # bad
    if some_condition then
      # body omitted
    end

    # good
    if some_condition
      # body omitted
    end
    ```

* 在多行的 `if/unless` 总是把条件放在与 `if/unless` 的同一行。

    ```Ruby
    # bad
    if
      some_condition
      do_something
      do_something_else
    end

    # good
    if some_condition
      do_something
      do_something_else
    end
    ```

* 喜欢三元操作运算（`?:`）超过`if/then/else/end`结构。
  它更加普遍而且明显的更加简洁。

    ```Ruby
    # bad
    result = if some_condition then something else something_else end

    # good
    result = some_condition ? something : something_else
    ```

* 使用一个表达式在三元操作运算的每一个分支下面只使用一个表达式。也就是说三元操作符不要被嵌套。在这样的情形中宁可使用 `if/else`。

    ```Ruby
    # bad
    some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else

    # good
    if some_condition
      nested_condition ? nested_something : nested_something_else
    else
      something_else
    end
    ```

* 不要使用 `if x: ...` - 它在Ruby 1.9中已经移除。使用三元操作运算代替。

    ```Ruby
    # bad
    result = if some_condition then something else something_else end

    # good
    result = some_condition ? something : something_else
    ```

* 不要使用 `if x; ...`。使用三元操作运算代替。

* 利用 `if` and `case` 是表达式这样的事实它们返回一个结果。

    ```Ruby
    # bad
    if condition
      result = x
    else
      result = y
    end

    # good
    result =
      if condition
        x
      else
        y
      end
    ```

* 在 one-line cases 的时候使用 `when x then ...`。替代的语法`when x: xxx`已经在Ruby 1.9中移除。


* 不要使用`when x; ...`。查看上面的规则。

* 使用 `!` 替代 `not`.

    ```Ruby
    # 差 - 因为操作符有优先级，需要用括号。
    x = (not something)

    # good
    x = !something
    ```

* 避免使用 `!!`.

    ```Ruby
    # bad
    x = 'test'
    # obscure nil check
    if !!x
      # body omitted
    end

    x = false
    # double negation is useless on booleans
    !!x # => false

    # good
    x = 'test'
    unless x.nil?
      # body omitted
    end
    ```

* The `and` and `or` keywords are banned. It's just not worth
  it. Always use `&&` and `||` instead.
* `and` 和 `or` 这两个关键字被禁止使用了。它名不符实。总是使用 `&&` 和 `||` 来取代。

    ```Ruby
    # bad
    # boolean expression
    if some_condition and some_other_condition
      do_something
    end

    # control flow
    document.saved? or document.save!

    # good
    # boolean expression
    if some_condition && some_other_condition
      do_something
    end

    # control flow
    document.saved? || document.save!
    ```

* 避免多行的 `? : `（三元操作符）；使用 `if/unless` 来取代。

* 单行主体喜欢使用 `if/unless` 修饰符。另一个好方法是使用 `&&/||` 控制流程。

    ```Ruby
    # bad
    if some_condition
      do_something
    end

    # good
    do_something if some_condition

    # another good option
    some_condition && do_something
    ```

* 布尔表达式使用`&&/||`, `and/or`用于控制流程。（经验Rule:如果你必须使用额外的括号（表达逻辑），那么你正在使用错误的的操作符。）

    ```Ruby
    # boolean expression
    if some_condition && some_other_condition
      do_something
    end

    # control flow
    document.save? or document.save!
    ```

* 避免多行`?:`(三元操作运算)，使用 `if/unless` 替代。

* 在单行语句的时候喜爱使用 `if/unless` 修饰符。另一个好的选择就是使 `and/or` 来做流程控制。

    ```Ruby
    # bad
    if some_condition
      do_something
    end

    # good
    do_something if some_condition

    # another good option
    some_condition and do_something
    ```

* 永远不要使用 `unless` 和 `else` 组合。将它们改写成肯定条件。

    ```Ruby
    # bad
    unless success?
      puts 'failure'
    else
      puts 'success'
    end

    # good
    if success?
      puts 'success'
    else
      puts 'failure'
    end
    ```

* 不用使用括号包含 `if/unless/while` 的条件。

    ```Ruby
    # bad
    if (x > 10)
      # body omitted
    end

    # good
    if x > 10
      # body omitted
    end
    ```

* 在多行 `while/until` 中不要使用 `while/until condition do` 。

    ```Ruby
    # bad
    while x > 5 do
      # body omitted
    end

    until x > 5 do
      # body omitted
    end

    # good
    while x > 5
      # body omitted
    end

    until x > 5
      # body omitted
    end
    ```

* 当你有单行主体时，尽量使用 `while/until` 修饰符。

    ```Ruby
    # bad
    while some_condition
      do_something
    end

    # good
    do_something while some_condition
    ```

* 否定条件判断尽量使用 `until` 而不是 `while` 。

    ```Ruby
    # bad
    do_something while !some_condition

    # good
    do_something until some_condition
    ```

* 循环后条件判断使用 `Kernel#loop` 和 `break`，而不是 `begin/end/until` 或者 `begin/end/while`。

   ```Ruby
   # bad
   begin
     puts val
     val += 1
   end while val < 0

   # good
   loop do
     puts val
     val += 1
     break unless val < 0
   end
   ```

* 忽略围绕内部 DSL 方法参数的括号 (如：Rake, Rails, RSpec)，Ruby 中带有 "关键字" 状态的方法（如：`attr_reader`，`puts`）以及属性存取方法。所有其他的方法调用使用括号围绕参数。

    ```Ruby
    class Person
      attr_reader :name, :age

      # omitted
    end

    temperance = Person.new('Temperance', 30)
    temperance.name

    puts temperance.age

    x = Math.sin(y)
    array.delete(e)

    bowling.score.should == 0
    ```

* 忽略隐式选项 hash 外部的花括号。

    ```Ruby
    # bad
    user.set({ name: 'John', age: 45, permissions: { read: true } })

    # good
    user.set(name: 'John', age: 45, permissions: { read: true })
    ```

* 内部 DSL 方法的外部括号和大括号。

    ```Ruby
    class Person < ActiveRecord::Base
      # bad
      validates(:name, { presence: true, length: { within: 1..10 } })

      # good
      validates :name, presence: true, length: { within: 1..10 }
    end
    ```

* 方法调用不需要参数，那么忽略圆括号。

    ```Ruby
    # bad
    Kernel.exit!()
    2.even?()
    fork()
    'test'.upcase()

    # good
    Kernel.exit!
    2.even?
    fork
    'test'.upcase
    ```


* 在单行代码块的时候宁愿使用 `{...}` 而不是 `do...end`。避免在多行代码块使用 `{...}` (多行链式通常变得非常丑陋)。通常使用 `do...end` 来做 `流程控制` 和 `方法定义` (例如 在 Rakefiles 和某些 DSLs 中)。避免在链式调用中使用 `do...end`。

    ```Ruby
    names = ['Bozhidar', 'Steve', 'Sarah']

    # bad
    names.each do |name|
      puts name
    end

    # good
    names.each { |name| puts name }

    # bad
    names.select do |name|
      name.start_with?('S')
    end.map { |name| name.upcase }

    # good
    names.select { |name| name.start_with?('S') }.map { |name| name.upcase }
    ```

    有人会争论多行链式看起来和使用 `{...}` 一样工作，但是他们问问自己 - 这样的代码真的有可读性码并且为什么代码块中的内容不能被提取到美丽的方法中。

* Consider using explicit block argument to avoid writing block
  literal that just passes its arguments to another block. Beware of
  the performance impact, though, as the block gets converted to a
  Proc.
  考虑使用明确的块参数来避免写入的块字面量仅仅传递参数的给另一个块。小心性能的影响，即使，
  块被转换成了 Proc。

    ```Ruby
    require 'tempfile'

    # bad
    def with_tmp_dir
      Dir.mktmpdir do |tmp_dir|
        Dir.chdir(tmp_dir) { |dir| yield dir }  # block just passes arguments
      end
    end

    # good
    def with_tmp_dir(&block)
      Dir.mktmpdir do |tmp_dir|
        Dir.chdir(tmp_dir, &block)
      end
    end

    with_tmp_dir do |dir|
      puts "dir is accessible as parameter and pwd is set: #{dir}"
    end
    ```

* 避免在不需要流的控制使用 `return`。

    ```Ruby
    # bad
    def some_method(some_arr)
      return some_arr.size
    end

    # good
    def some_method(some_arr)
      some_arr.size
    end
    ```


* 避免在不需要的地方使用 `self`(它仅仅在调用一些 `self` 做写访问的时候需要)(It is only required when calling a self write accessor.)

    ```Ruby
    # bad
    def ready?
      if self.last_reviewed_at > self.last_updated_at
        self.worker.update(self.content, self.options)
        self.status = :in_progress
      end
      self.status == :verified
    end

    # good
    def ready?
      if last_reviewed_at > last_updated_at
        worker.update(content, options)
        self.status = :in_progress
      end
      status == :verified
    end
    ```

* 作为一个必然的结果，避免将方法（参数）放于局部变量阴影之下除非它们是相等的。

    ```Ruby
    class Foo
      attr_accessor :options

      # ok
      def initialize(options)
        self.options = options
        # both options and self.options are equivalent here
      end

      # bad
      def do_something(options = {})
        unless options[:when] == :later
          output(self.options[:message])
        end
      end

      # good
      def do_something(params = {})
        unless params[:when] == :later
          output(options[:message])
        end
      end
    end
    ```

* 不要在条件表达式里使用 `=` （赋值）的返回值，除非条件表达式在圆括号内被赋值。
  这是一个相当流行的 ruby 方言，有时被称为 *safe assignment in condition*。

    ```Ruby
    # bad (+ a warning)
    if v = array.grep(/foo/)
      do_something(v)
      ...
    end

    # good (MRI would still complain, but RuboCop won't)
    if (v = array.grep(/foo/))
      do_something(v)
      ...
    end

    # good
    v = array.grep(/foo/)
    if v
      do_something(v)
      ...
    end
    ```

* 在任何可以的地方使用快捷的 `self assignment` 操作符。

    ```Ruby
    # bad
    x = x + y
    x = x * y
    x = x**y
    x = x / y
    x = x || y
    x = x && y

    # good
    x += y
    x *= y
    x **= y
    x /= y
    x ||= y
    x &&= y
    ```

* 只有在变量没有被初始化的时候使用 `||=` 来初始化变量。

    ```Ruby
    # set name to Vozhidar, only if it's nil or false
    name ||= 'Bozhidar'
    ````

* 不要使用`||=`来初始化布尔变量。（想想如果当前值为`false`的时候会发生什么。）

    ```Ruby
    # bad - would set enabled to true even if it was false
    enable ||= true

    # good
    enabled = true if enabled.nil?
    ```

* 使用 `&&=` 来预处理变量不确定是否存在的变量。使用 `&&=` 仅仅在（变量）存在的时候
  才会改变值，除去了使用 `if` 来检查它的存在性。

    ```Ruby
    # bad
    if something
      something = something.downcase
    end

    # bad
    something = something ? nil : something.downcase

    # ok
    something = something.downcase if something

    # good
    something = something && something.downcase

    # better
    something &&= something.downcase
    ```

* 避免全等（case equality）`===` 操作符的使用。从名称可知，这是 `case` 表达式的隐式使用并且在 `case` 语句外的场合使用会产生难以理解的代码。

    ```Ruby
    # bad
    Array === something
    (1..100) === 7
    /something/ === some_string

    # good
    something.is_a?(Array)
    (1..100).include?(7)
    some_string =~ /something/
    ```

* 避免使用 Perl 的指定变量风格（比如，`$:`，`$;` 等等。）。它们相当神秘，不鼓励在单行代码之外使用它们。
  使用 `English` 库提供的友好别名。

    ```Ruby
    # bad
    $:.unshift File.dirname(__FILE__)

    # good
    require 'English'
    $LOAD_PATH.unshift File.dirname(__FILE__)
    ```

* 从来不要在方法名和（参数）开括号之间使用空格。

    ```Ruby
    # bad
    f (3+2) + 1

    # good
    f(3 + 2) +1
    ```

* 如果方法的第一个参数以开括号开始，通常使用括号把它们全部括起来。例如`f((3 + 2) + 1)`。

* 通常使用 `-w` 选项运行 Ruby 解释器，在你忘记上面所诉规则，ruby 将会提示你。

* 定义单行块使用新的 lambda 语法。定义多行块中使用 `lambda` 方法。

    ```Ruby
    # bad
    l = lambda { |a, b| a + b }
    l.call(1, 2)

    # correct, but looks extremely awkward
    l = ->(a, b) do
      tmp = a * 7
      tmp * b / 50
    end

    # good
    l = ->(a, b) { a + b }
    l.call(1, 2)

    l = lambda do |a, b|
      tmp = a * 7
      tmp * b / 50
    end
    ```

* 用 `proc` 而不是 `Proc.new`。

    ```Ruby
    # bad
    p = Proc.new { |n| puts n }

    # good
    p = proc { |n| puts n }
    ```

* 匿名方法 和 块 用 `proc.call()` 而不是 `proc[]` 或 `proc.()`。

    ```Ruby
    # bad - looks similar to Enumeration access
    l = ->(v) { puts v }
    l[1]

    # also bad - uncommon syntax
    l = ->(v) { puts v }
    l.(1)

    # good
    l = ->(v) { puts v }
    l.call(1)
    ```

* 未使用的块参数和局部变量使用 `_`。它也可以接受通过 `_` 来使用（即使它有少了些描述性）。
  这个惯例由 Ruby 解释器以及 RuboCop 这样的工具组织其将会抑制它们的未使用参数警告。

    ```Ruby
    # bad
    result = hash.map { |k, v| v + 1 }

    def something(x)
      unused_var, used_var = something_else(x)
      # ...
    end

    # good
    result = hash.map { |_k, v| v + 1 }

    def something(x)
      _unused_var, used_var = something_else(x)
      # ...
    end

    # good
    result = hash.map { |_, v| v + 1 }

    def something(x)
      _, used_var = something_else(x)
      # ...
    end
    ```

* 使用 `$stdout/$stderr/$stdin` 而不是 `STDOUT/STDERR/STDIN`。`STDOUT/STDERR/STDIN` 是常量，虽然在 Ruby 中是可以给常量重新赋值的（可能是重定向到某个流），但解释器会警告如果你执意这样。

* 使用 `warn` 而不是 `$stderr.puts`。除了更加清晰简洁，如果你需要的话，
  `warn` 还允许你抑制（suppress）警告（通过 `-W0` 将警告级别设为 0）。

* 倾向使用 `sprintf` 和它的别名 `format` 而不是相当隐晦的 `String#%` 方法.

    ```Ruby
    # bad
    '%d %d' % [20, 10]
    # => '20 10'

    # good
    sprintf('%d %d', 20, 10)
    # => '20 10'

    # good
    sprintf('%{first} %{second}', first: 20, second: 10)
    # => '20 10'

    format('%d %d', 20, 10)
    # => '20 10'

    # good
    format('%{first} %{second}', first: 20, second: 10)
    # => '20 10'
    ```

* 倾向使用 `Array#join` 而不是相当隐晦的使用字符串作参数的 `Array#*`。


    ```Ruby
    # bad
    %w(one two three) * ', '
    # => 'one, two, three'

    # good
    %w(one two three).join(', ')
    # => 'one, two, three'
    ```

* 当处理你希望像 Array 那样对待的变量，但是你不确定它是一个数组时，
  使用 `[*var]` or `Array()` 而不是显式的 `Array` 检查。

    ```Ruby
    # bad
    paths = [paths] unless paths.is_a? Array
    paths.each { |path| do_something(path) }

    # good
    [*paths].each { |path| do_something(path) }

    # good (and a bit more readable)
    Array(paths).each { |path| do_something(path) }
    ```

* 尽量使用范围或 `Comparable#between?` 来替换复杂的逻辑比较。

    ```Ruby
    # bad
    do_something if x >= 1000 && x <= 2000

    # good
    do_something if (1000..2000).include?(x)

    # good
    do_something if x.between?(1000, 2000)
    ```

* 尽量用谓词方法而不是使用 `==`。比较数字除外。

    ```Ruby
    # bad
    if x % 2 == 0
    end

    if x % 2 == 1
    end

    if x == nil
    end

    # good
    if x.even?
    end

    if x.odd?
    end

    if x.nil?
    end

    if x.zero?
    end

    if x == 0
    end
    ```

* 避免使用 `BEGIN` 区块。

* 使用 `Kernel#at_exit` 。永远不要用 `END` 区块。

    ```ruby
    # bad

    END { puts 'Goodbye!' }

    # good

    at_exit { puts 'Goodbye!' }
    ```

* 避免使用 flip-flops 。

* 避免使用嵌套的条件来控制流程。
  当你可能断言不合法的数据，使用一个防御语句。一个防御语句是一个在函数顶部的条件声明，这样如果数据不合法就能尽快的跳出函数。

    ```Ruby
    # bad
      def compute_thing(thing)
        if thing[:foo]
          update_with_bar(thing)
          if thing[:foo][:bar]
            partial_compute(thing)
          else
            re_compute(thing)
          end
        end
      end

    # good
      def compute_thing(thing)
        return unless thing[:foo]
        update_with_bar(thing[:foo])
        return re_compute(thing) unless thing[:foo][:bar]
        partial_compute(thing)
      end
    ```

