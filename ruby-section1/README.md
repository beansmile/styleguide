# 源代码布局

> 所有风格都又丑又难读，自己的除外。几乎人人都这样想。
> 把 “自己的除外”拿掉，他们或许是对的...
> -- Jerry Coffin (on indentation缩进)

* 使用 `UTF-8` 作为源文件编码。
* 每个缩进级别使用两个 **spaces** （又名软 tabs）. 不要硬 tabs

    ```Ruby
    # bad - four spaces
    def some_method
        do_something
    end

    # good
    def some_method
      do_something
    end
    ```

* 使用 Unix-风格 换行符。(*BSD/Solaris/Linux/OSX 用户被为默认涵盖，Windows 用户必须特别小心.)
> \n是换行，英文是LineFeed，ASCII码是0xA。
> \r是回车，英文是Carriage Return ,ASCII码是0xD。
> windows下enter是 \n\r,unix下是\n,mac下是\r
    * 如果你正在使用 Git 你可能会想要添加下面的配置设置来保护你的项目（避免）Windows 蔓延过来的换行符:

    ```bash
    $ git config --global core.autocrlf true
    ```

* 不用使用 `;` 来分割语句和表达式。以此推论 - 一行使用一个表达式

    ```Ruby
    # bad
    puts 'foobar'; # superfluous semicolon

    puts 'foo'; puts 'bar' # two expression on the same line

    # good
    puts 'foobar'

    puts 'foo'
    puts 'bar'

    puts 'foo', 'bar' # this applies to puts in particular
    ```

* 对于没有内容的类定义，尽可能使用单行类定义形式.

    ```Ruby
    # bad
    class FooError < StandardError
    end

    # okish
    class FooError < StandardError; end

    # good
    FooError = Class.new(StandardError)
    ```

* 避免单行方法。即便还是会受到一些人的欢迎，这里还是会有一些古怪的语法用起来很容易犯错.
  无论如何 - 应该一行不超过一个单行方法.

    ```Ruby
    # bad
    def too_much; something; something_else; end

    # okish - notice that the first ; is required
    def no_braces_method; body end

    # okish - notice that the second ; is optional
    def no_braces_method; body; end

    # okish - valid syntax, but no ; make it kind of hard to read
    def some_method() body end

    # good
    def some_method
      body
    end
    ```

    空方法是这个规则的例外。

    ```Ruby
    # good
    def no_op; end
    ```

* 操作符旁的空格，在逗号，冒号和分号后；在 `{` 旁和在 `}` 之前，大多数空格可能对 Ruby 解释（代码）无关，但是它的恰当使用是让代码变得易读的关键。

    ```Ruby
    sum = 1 + 2
    a, b = 1, 2
    1 > 2 ? true : false; puts 'Hi'
    [1, 2, 3].each { |e| puts e }
    ```

    唯一的例外是当使用指数操作时：

    ```Ruby
    # bad
    e = M * c ** 2

    # good
    e = M * c**2
    ```

    `{` 和 `}` 值得额外的澄清，自从它们被用于 块 和 hash 字面量，以及以表达式的形式嵌入字符串。
    对于 hash 字面量两种风格是可以接受的。

    ```Ruby
    # good - space after { and before }
    { one: 1, two: 2 }

    # good - no space after { and before }
    {one: 1, two: 2}
    ```

    第一种稍微更具可读性（并且争议的是一般在 Ruby 社区里面更受欢迎）。
    第二种可以增加了 块 和 hash 可视化的差异。
    无论你选哪一种都行 - 但是最好保持一致。

    目前对于嵌入表达式，也有两个选择：

    ```Ruby
    # good - no spaces
    "string#{expr}"

    # ok - arguably more readable
    "string#{ expr }"
    ```

    第一种风格极为流行并且通常建议你与之靠拢。第二种，在另一方面，（有争议）更具可读性。
    如同 hash - 选取一个风格并且保持一致。

* 没有空格 `(`, `[`之后或者 `]`, `)`之前。

    ```Ruby
    some(arg).other
    [1, 2, 3].length
    ```

* `!` 之后没有空格 .

    ```Ruby
    # bad
    ! something

    # good
    !something
    ```

* `when`和`case` 缩进深度一致。我知道很多人会不同意这点，但是它是"The Ruby Programming Language" 和 "Programming Ruby"中公认的风格。

    ```Ruby
    # bad
    case
      when song.name == 'Misty'
        puts 'Not again!'
      when song.duration > 120
        puts 'Too long!'
      when Time.now.hour > 21
        puts "It's too late"
      else
        song.play
    end

    # good
    case
    when song.name == 'Misty'
      puts 'Not again!'
    when song.duration > 120
      puts 'Too long!'
    when Time.now.hour > 21
      puts "It's too late"
    else
      song.play
    end
    ```
    ```Ruby
    case
    when song.name == 'Misty'
      puts 'Not again!'
    when song.duraton > 120
      puts 'Too long!'
    when Time.now > 21
      puts "It's too late"
    else
      song.play
    end
    ```

* 当赋值一个条件表达式的结果给一个变量时，保持分支的缩排在同一层。

    ```Ruby
    # bad - pretty convoluted
    kind = case year
    when 1850..1889 then 'Blues'
    when 1890..1909 then 'Ragtime'
    when 1910..1929 then 'New Orleans Jazz'
    when 1930..1939 then 'Swing'
    when 1940..1950 then 'Bebop'
    else 'Jazz'
    end

    result = if some_cond
      calc_something
    else
      calc_something_else
    end

    # good - it's apparent what's going on
    kind = case year
           when 1850..1889 then 'Blues'
           when 1890..1909 then 'Ragtime'
           when 1910..1929 then 'New Orleans Jazz'
           when 1930..1939 then 'Swing'
           when 1940..1950 then 'Bebop'
           else 'Jazz'
           end

    result = if some_cond
               calc_something
             else
               calc_something_else
             end

    # good (and a bit more width efficient)
    kind =
      case year
      when 1850..1889 then 'Blues'
      when 1890..1909 then 'Ragtime'
      when 1910..1929 then 'New Orleans Jazz'
      when 1930..1939 then 'Swing'
      when 1940..1950 then 'Bebop'
      else 'Jazz'
      end

    result =
      if some_cond
        calc_something
      else
        calc_something_else
      end
    ```

* 在方法定义之间使用空行并且一个方法根据逻辑段来隔开。

    ```Ruby
    def some_method
      data = initialize(options)

      data.manipulate!

      data.result
    end

    def some_methods
      result
    end
    ```

* 避免在一个方法调用的最后一个参数有逗号，特别是当参数不在另外一行。

    ```Ruby
    # bad - easier to move/add/remove parameters, but still not preferred
    some_method(
                 size,
                 count,
                 color,
               )

    # bad
    some_method(size, count, color, )

    # good
    some_method(size, count, color)
    ```

* 当给方法的参数赋默认值时，在 `=` 两边使用空格：

    ```Ruby
    # bad
    def some_method(arg1=:default, arg2=nil, arg3=[])
      # do something...
    end

    # good
    def some_method(arg1 = :default, arg2 = nil, arg3 = [])
      # do something...
    end
    ```

    虽然几本 Ruby 书建议用第一个风格，不过第二个风格在实践中更为常见（并可争议地可读性更高一点）。

* 避免在不需要的时候使用行继续符 `\` 。实践中，
  除非用于连接字符串, 否则避免在任何情况下使用行继续符。

    ```Ruby
    # bad
    result = 1 - \
             2

    # good (but still ugly as hell)
    result = 1 \
             - 2

    long_string = 'First part of the long string' \
                  ' and second part of the long string'
    ```

* 采用连贯的多行方法链式风格。在 Ruby 社区有两种受欢迎的风格，它们都被认为很好
  \- `.` 开头(选项 A) 和 尾随 `.` (选项 B) 。

    * **(选项 A)** 当一个链式方法调用需要在另一行继续时，将 `.` 放在第二行。

        ```Ruby
        # bad - need to consult first line to understand second line
        one.two.three.
          four

        # good - it's immediately clear what's going on the second line
        one.two.three
          .four
        ```
    * **(选项 B)** 当在另一行继续一个链式方法调用，将 `.` 放在第一行来识别要继续的表达式。

        ```Ruby
        # bad - need to read ahead to the second line to know that the chain continues
        one.two.three
          .four

        # good - it's immediately clear that the expression continues beyond the first line
        one.two.three.
          four
        ```
    在[这里](https://github.com/bbatsov/ruby-style-guide/pull/176)可以发现有关这两个另类风格的优点的讨论。

* 如果一个方法调用的跨度超过了一行，对齐它们的参数。当参数对齐因为行宽限制而不合适，
  在第一行之后单缩进也是可以接受的。

    ```Ruby
    # starting point (line is too long)
    def send_mail(source)
      Mailer.deliver(to: 'bob@example.com', from: 'us@example.com', subject: 'Important message', body: source.text)
    end

    # bad (double indent)
    def send_mail(source)
      Mailer.deliver(
          to: 'bob@example.com',
          from: 'us@example.com',
          subject: 'Important message',
          body: source.text)
    end

    # good
    def send_mail(source)
      Mailer.deliver(to: 'bob@example.com',
                     from: 'us@example.com',
                     subject: 'Important message',
                     body: source.text)
    end

    # good (normal indent)
    def send_mail(source)
      Mailer.deliver(
        to: 'bob@example.com',
        from: 'us@example.com',
        subject: 'Important message',
        body: source.text
      )
    end
    ```

* 对齐多行跨度的 array literals 的元素。

    ```Ruby
    # bad - single indent
    menu_item = ['Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam',
      'Baked beans', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam']

    # good
    menu_item = [
      'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam',
      'Baked beans', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam'
    ]

    # good
    menu_item =
      ['Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam',
       'Baked beans', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam']
    ```

* 大数值添加下划线来提高它们的可读性。

    ```Ruby
    # bad - how many 0s are there?
    num = 1000000

    # good - much easier to parse for the human brain
    num = 1_000_000
    ```

* 使用 RDoc 以及它的惯例来撰写 API 文档。注解区块及 `def` 不要用空行隔开。
* 每一行限制在 80 个字符内。
* 避免行尾空格。
* 不要使用区块注释。它们不能由空白引导（=begin 必须顶头开始），并且不如普通注释容易辨认。

    ```Ruby
    # bad
    == begin
    comment line
    another comment line
    == end

    # good
    # comment line
    # another comment line
    ```

* 在 API 文档中使用 RDoc和它的公约。不要在注释代码块和`def`之间加入空行。
* 保持每一行少于80字符。
* 避免尾随空格。
