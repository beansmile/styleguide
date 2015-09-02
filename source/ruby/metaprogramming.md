# 元编程

* 避免无限循环的元编程。

* 写一个函数库时不要使核心类混乱（不要使用 monkey patch）。

* 代码块形式最好用于字符串插值形式。
  - 当你使用字符串插值形式，总是提供 `__FILE__` 和 `__LINE__`，使得你的回溯有意义。

    ```ruby
    class_eval 'def use_relative_model_naming?; true; end', __FILE__, __LINE__
    ```

  - `define_method` 最好用 `class_eval{ def ... }`

* 当使用 `class_eval` (或者其他的 `eval`)以及字符串插值，添加一个注释块使之在插入的时候显示(这是我从 rails 代码学来的实践)：

    ```ruby
    # from activesupport/lib/active_support/core_ext/string/output_safety.rb
    UNSAFE_STRING_METHODS.each do |unsafe_method|
      if 'String'.respond_to?(unsafe_method)
        class_eval <<-EOT, __FILE__, __LINE__ + 1
          def #{unsafe_method}(*args, &block)       # def capitalize(*args, &block)
            to_str.#{unsafe_method}(*args, &block)  #   to_str.capitalize(*args, &block)
          end                                       # end

          def #{unsafe_method}!(*args)              # def capitalize!(*args)
            @dirty = true                           #   @dirty = true
            super                                   #   super
          end                                       # end
        EOT
      end
    end
    ```

* 避免在元编程中使用 `method_missing`，它使得回溯变得很麻烦，这个习惯不被列在 `#methods`，拼写错误的方法可能也在默默的工作，例如 `nukes.launch_state = false`。考虑使用委托，代理或者是 `define_method` ，如果必须这样，使用 `method_missing` ，
  - 确保 [也定义了 `respond_to_missing?`](http://blog.marc-andre.ca/2010/11/methodmissing-politely.html)
  - 仅捕捉字首定义良好的方法，像是 find_by_* ― 让你的代码越肯定（assertive）越好。
  - 在语句的最后调用 super
  - delegate 到确定的、非魔法方法中:

    ```ruby
    # bad
    def method_missing?(meth, *args, &block)
      if /^find_by_(?<prop>.*)/ =~ meth
        # ... lots of code to do a find_by
      else
        super
      end
    end

    # good
    def method_missing?(meth, *args, &block)
      if /^find_by_(?<prop>.*)/ =~ meth
        find_by(prop, *args, &block)
      else
        super
      end
    end

    # best of all, though, would to define_method as each findable attribute is declared
    ```
