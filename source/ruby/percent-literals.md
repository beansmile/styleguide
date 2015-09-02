# 百分号和字面值

* 需要插值与嵌入双引号的单行字符串使用 `%()` （是 `%Q` 的简写）。多行字符串，最好用 heredocs 。

    ```Ruby
    # bad (no interpolation needed)
    %(<div class="text">Some text</div>)
    # should be '<div class="text">Some text</div>'

    # bad (no double-quotes)
    %(This is #{quality} style)
    # should be "This is #{quality} style"

    # bad (multiple lines)
    %(<div>\n<span class="big">#{exclamation}</span>\n</div>)
    # should be a heredoc.

    # good (requires interpolation, has quotes, single line)
    %(<tr><td class="name">#{name}</td>)
    ```

* 没有 `'` 和 `"` 的字符串不要使用 `%q` 。除非许多字符需要转义，否则普通字符串可读性更好。

    ```Ruby
    # bad
    name = %q(Bruce Wayne)
    time = %q(8 o'clock)
    question = %q("What did you say?")

    # good
    name = 'Bruce Wayne'
    time = "8 o'clock"
    question = '"What did you say?"'
    ```

* `%r` 的方式只适合于定义包含多个 `/` 符号的正则表达式。

    ```Ruby
    # bad
    %r(\s+)

    # still bad
    %r(^/(.*)$)
    # should be /^\/(.*)$/

    # good
    %r(^/blog/2011/(.*)$)
    ```

* 除非调用的命令中用到了反引号（这种情况不常见），否则不要用 `%x`。

    ```Ruby
    # bad
    date = %x(date)

    # good
    date = `date`
    echo = %x(echo `date`)
    ```

* 不要用 `%s` 。社区倾向使用 `:"some string"` 来创建含有空白的符号。

* 用 `%` 表示字面量时使用 `()`， `%r` 除外。因为大括号经常出现在正则表达式在很多场景中在很多场景中不太通用的字符例如 `{` 作为分割符可能是一个更好的选择，取决于正则式的内容。

    ```Ruby
    # bad
    %w[one two three]
    %q{"Test's king!", John said.}

    # good
    %w(one two three)
    %q("Test's king!", John said.)
    ```

