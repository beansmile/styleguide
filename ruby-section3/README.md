# 命名
> The only real difficulties in programming are cache invalidation and
> naming things. <br/>
> -- Phil Karlton
> 程式设计的真正难题是替事物命名及使缓存失效。<br/>
> -- Phil Karlton

* 用英语命名标识符。

    ```Ruby
    # bad - identifier using non-ascii characters
    заплата = 1_000

    # bad - identifier is a Bulgarian word, written with Latin letters (instead of Cyrillic)
    zaplata = 1_000

    # good
    salary = 1_000
    ```

* 使用`snake_case`的形式给变量和方法命名。

    ```Ruby
    # bad
    :'some symbol'
    :SomeSymbol
    :someSymbol

    someVar = 5

    def someMethod
      ...
    end

    def SomeMethod
     ...
    end

    # good
    :some_symbol

    def some_method
      ...
    end
    ```

* Snake case: punctuation is removed and spaces are replaced by single underscores. Normally the letters share the same case (either UPPER_CASE_EMBEDDED_UNDERSCORE or lower_case_embedded_underscore) but the case can be mixed

* 使用`CamelCase(駝峰式大小寫)`的形式给类和模块命名。(保持使用缩略首字母大写的方式如HTTP,
  RFC, XML)

    ```Ruby
    # bad
    class Someclass
      ...
    end

    class Some_Class
      ...
    end

    class SomeXml
      ...
    end

    # good
    class SomeClass
      ...
    end

    class SomeXML
      ...
    end
    ```

* 使用 `snake_case` 来命名文件, 例如 `hello_world.rb`。

* 以每个源文件中仅仅有单个 class/module 为目的。按照 class/module 来命名文件名，但是替换 CamelCase 为 snake_case。

* 使用`SCREAMING_SNAKE_CASE`给常量命名。

    ```Ruby
    # bad
    SomeConst = 5

    # good
    SOME_CONST = 5
    ```

* 在表示判断的方法名（方法返回真或者假）的末尾添加一个问号（如Array#empty?）。
  方法不返回一个布尔值，不应该以问号结尾。
* 可能会造成潜在“危险”的方法名（如修改 `self`或者 参数的方法，`exit!` (不是像 `exit` 执行完成项)等）应该在末尾添加一个感叹号如果这里存在一个该 *危险* 方法的安全版本。

    ```Ruby
    # bad - there is not matching 'safe' method
    class Person
      def update!
      end
    end

    # good
    class Person
      def update
      end
    end

    # good
    class Person
      def update!
      end

      def update
      end
    end
    ```

* 如果可能的话，根据危险方法（bang）来定义对应的安全方法（non-bang）。

    ```Ruby
    class Array
      def flatten_once!
        res = []

        each do |e|
          [*e].each { |f| res << f }
        end

        replace(res)
      end

      def flatten_once
        dup.flatten_once!
      end
    end
    ```

* 当在短的块中使用 `reduce` 时，命名参数 `|a, e|` (accumulator, element)。

    ```Ruby
    #Combines all elements of enum枚举 by applying a binary operation, specified by a block or a symbol that names a method or operator.
    # Sum some numbers
    (5..10).reduce(:+)                            #=> 45#reduce
    # Same using a block and inject
    (5..10).inject {|sum, n| sum + n }            #=> 45 #inject注入
    # Multiply some numbers
    (5..10).reduce(1, :*)                         #=> 151200
    # Same using a block
    (5..10).inject(1) {|product, n| product * n } #=> 151200
    ```

* 在定义二元操作符时，把参数命名为 `other` （`<<` 与 `[]` 是这条规则的例外，因为它们的语义不同）。

    ```Ruby
    def +(other)
      # body omitted
    end
    ```
* `map` 优先于 `collect`，`find` 优先于 `detect`，`select` 优先于 `find_all`，`reduce` 优先于`inject`，`size` 优先于 `length`。以上的规则并不绝定，如果使用后者能提高代码的可读性，那么尽管使用它们。有押韵的方法名（如 `collect`，`detect`，`inject`）继承于 SmallTalk 语言，它们在其它语言中并不是很通用。鼓励使用 `select` 而不是 `find_all` 是因为 `select` 与 `reject` 一同使用时很不错，并且它的名字具有很好的自解释性。

* 不要使用 `count` 作为 `size` 的替代。对于 `Enumerable` 的 `Array` 以外的对象将会迭代整个集合来
  决定它的尺寸。


    ```Ruby
    # bad
    some_hash.count

    # good
    some_hash.size
    ```

* 倾向使用 `flat_map` 而不是 `map` + `flatten` 的组合。
  这并不适用于深度大于 2 的数组，举个例子，如果 `users.first.songs == ['a', ['b', 'c']]` ，则使用 `map + flatten` 的组合，而不是使用 `flat_map` 。
  `flat_map` 将数组变平坦一个层级，而 `flatten` 会将整个数组变平坦。

    ```Ruby
    # bad
    all_songs = users.map(&:songs).flatten.uniq

    # good
    all_songs = users.flat_map(&:songs).uniq
    ```

* 使用 `reverse_each` 代替 `reverse.each`。`reverse_each` 不会分配一个新数组并且这是好事。

    ```Ruby
    # bad
    array.reverse.each { ... }

    # good
    array.reverse_each { ... }
    ```
