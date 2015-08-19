# 注解

* 注解应该写在紧接相关代码的上方。
* 注解关键字后跟一个冒号和空格，然后是描述问题的记录。
* 如果需要多行来描述问题，随后的行需要在 `#` 后面缩进两个空格。

    ```Ruby
    def bar
      # FIXME: This has crashed occasionally since v3.2.1. It may
      #  be related to the BarBazUtil upgrade.
      baz(:quux)
    end

* 如果问题相当明显，那么任何文档就多余了，注解也可以（违规的）在行尾而没有任何备注。这种用法不应当在一般情况下使用，也不应该是一个 rule。

    ```Ruby
    def bar
      sleep 100 # OPTIMIZE
    end
    ```

* 使用 `TODO` 来备注缺失的特性或者在以后添加的功能。
* 使用 `FIXME` 来备注有问题需要修复的代码。
* 使用 `OPTIMIZE` 来备注慢的或者低效的可能引起性能问题的代码。
* 使用 `HACK` 来备注那些使用问题代码的地方可能需要重构。
* 使用 `REVIEW` 来备注那些需要反复查看确认工作正常的代码。例如： `REVIEW: 你确定客户端是怎样正确的完成 X 的吗？`
* 使用其他自定义的关键字如果认为它是合适的，但是确保在你的项目的 `README` 或者类似的地方注明。

