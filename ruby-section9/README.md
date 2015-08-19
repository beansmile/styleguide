# 字符串

* 优先使用 `字符串插值` 来代替 `字符串串联`。

    ```Ruby
    # bad
    email_with_name = user.name + ' <' + user.email + '>'

    # good
    email_with_name = "#{user.name} <#{user.email}>"

    # good
    email_with_name = format('%s <%s>', user.name, user.email)
    ```

* Consider padding string interpolation code with space. It more clearly sets the
  code apart from the string.考虑使用空格填充字符串插值。它更明确了除字符串的插值来源。

    ```Ruby
    "#{ user.last_name }, #{ user.first_name }"
    ```

* Consider padding string interpolation code with space. It more clearly sets the
  code apart from the string.
  考虑替字符串插值留白。這使插值在字符串里看起來更清楚。

    ```Ruby
    "#{ user.last_name }, #{ user.first_name }"
    ```

* 采用一致的字符串字面量引用风格。这里有在社区里面受欢迎的两种风格，它们都被认为非常好 -
  默认使用单引号（选项 A）以及双引号风格（选项 B）。

    * **(Option A)** 当你不需要字符串插值或者例如 `\t`， `\n`， `'` 这样的特殊符号的
      时候优先使用单引号引用。

        ```Ruby
        # bad
        name = "Bozhidar"

        # good
        name = 'Bozhidar'
        ```

    * **(Option B)** Prefer double-quotes unless your string literal
      contains `"` or escape characters you want to suppress.
      除非你的字符串字面量包含 `"` 或者你需要抑制转义字符（escape characters）
      优先使用双引号引用。

        ```Ruby
        # bad
        name = 'Bozhidar'

        # good
        name = "Bozhidar"
        ```

    第二种风格可以说在 Ruby 社区更受欢迎些。该指南的字符串字面量，无论如何，
    与第一种风格对齐。

* 不要使用 `?x` 符号字面量语法。从 Ruby 1.9 开始基本上它是多余的，`?x` 将会被解释为 `x` （只包括一个字符的字符串）。

    ```Ruby
    # bad
    char = ?c

    # good
    char = 'c'
    ```

* 别忘了使用 `{}` 来围绕被插入字符串的实例与全局变量。

    ```Ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      # bad - valid, but awkward
      def to_s
        "#@first_name #@last_name"
      end

      # good
      def to_s
        "#{@first_name} #{@last_name}"
      end
    end

    $global = 0
    # bad
    puts "$global = #$global"

    # good
    puts "$global = #{$global}"
    ```

* 在对象插值的时候不要使用 `Object#to_s`，它将会被自动调用。

    ```Ruby
    # bad
    message = "This is the #{result.to_s}."

    # good
    message = "This is the #{result}."
    ```

* 操作较大的字符串时, 避免使用 `String#+` 做为替代使用 `String#<<`。就地级联字符串块总是比 `String#+` 更快，它创建了多个字符串对象。

    ```Ruby
    # good and also fast
    html = ''
    html << '<h1>Page title</h1>'

    paragraphs.each do |paragraph|
      html << "<p>#{paragraph}</p>"
    end
    ```

* When using heredocs for multi-line strings keep in mind the fact
  that they preserve leading whitespace. It's a good practice to
  employ some margin based on which to trim the excessive whitespace.
  heredocs 中的多行文字会保留前缀空白。因此做好如何缩进的规划。这是一个很好的
  做法，采用一定的边幅在此基础上削减过多的空白。

    ```Ruby
    code = <<-END.gsub(/^\s+\|/, '')
      |def test
      |  some_method
      |  other_method
      |end
    END
    #=> "def test\n  some_method\n  other_method\nend\n"
    ```

