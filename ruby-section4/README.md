# 注释

> Good code is its own best documentation. As you're about to add a
> comment, ask yourself, "How can I improve the code so that this
> comment isn't needed?" Improve the code and then document it to make
> it even clearer. <br/>
> -- Steve McConnell
> 好的代码在于它有好的文档。当你打算添加一个注释，问问自己，“我该做的是怎样提高代码质量，那么这个注释是不是不需要了？”提高代码并且给他们添加文档使得它更加简洁。<br/>
> -- Steve McConnell

* 写出自解释文档代码，然后让这部分歇息吧。这不是说着玩。
* 使用英文编写注释。
* 使用一个空格将注释与符号隔开。
* 注释超过一个单词了，应句首大写并使用标点符号。句号后使用 [一个空格](http://en.wikipedia.org/wiki/Sentence_spacing)
* 避免多余的注释。

    ```Ruby
    # bad
    counter += 1 # increments counter by one
    ```

* 随时更新注释，没有注释比过期的注释更好。

> Good code is like a good joke - it needs no explanation. <br/>
> -- Russ Olsen

* Avoid writing comments to explain bad code. Refactor the code to
  make it self-explanatory. (Do or do not - there is no try. --Yoda)

* 不要为糟糕的代码写注释。重构它们，使它们能够“自解释”。(Do or do not - there is no try.)
