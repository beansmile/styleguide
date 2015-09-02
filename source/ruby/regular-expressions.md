## 正则表达式

> Some people, when confronted with a problem, think
> "I know, I'll use regular expressions." Now they have two problems.<br/>
> -- Jamie Zawinski

* 如果只是需要中查找字符串的 `text`, 不要使用正则表达式：`string['text']`
* 针对简单的结构, 你可以直接使用string[/RE/]的方式来查询.

    ```Ruby
    match = string[/regexp/]             # get content of matched regexp
    first_group = string[/text(grp)/, 1] # get content of captured group
    string[/text (grp)/, 1] = 'replace'  # string => 'text replace'
    ```

* 当你不需要替结果分组时，使用非分组的群组。

    ```Ruby
    /(first|second)/   # bad
    /(?:first|second)/ # good
    ```

* 不要使用 Perl 遗风的变量来表示匹配的正则分组（如 `$1`，`$2` 等），使用 `Regexp.last_match[n]` 作为替代。

    ```Ruby
    /(regexp)/ =~ string
    ...

    # bad
    process $1

    # good
    process Regexp.last_match[1]
    ```

* 避免使用数字化命名分组很难明白他们代表的意思。命名群组来替代。

    ```Ruby
    # bad
    /(regexp)/ =~ string
    ...
    process Regexp.last_match[1]

    # good
    /(?<meaningful_var>regexp)/ =~ string
    ...
    process meaningful_var
    ```

* 字符类有以下几个特殊关键字值得注意: `^`, `-`, `\`, `]`, 所以, 不要转义 `.` 或者 `[]` 中的括号。

* 注意, `^` 和 `$` , 他们匹配行首和行尾, 而不是一个字符串的结尾, 如果你想匹配整个字符串, 用 `\A` 和 `\Z`。

    ```Ruby
    string = "some injection\nusername"
    string[/^username$/]   # matches
    string[/\Ausername\Z/] # don't match
    ```

* 针对复杂的正则表达式，使用 x 修饰符。可提高可读性并可以加入有用的注释。只是要注意空白字符会被忽略。

    ```Ruby
    regexp = %r{
      start         # some text
      \s            # white space char
      (group)       # first group
      (?:alt1|alt2) # some alternation
      end
    }x
    ```

*  `sub`/`gsub` 也支持哈希以及代码块形式语法, 可用于复杂情形下的替换操作.
