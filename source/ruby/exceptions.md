# 异常

* 单个异常使用 `fail` 关键字仅仅当捕获一个异常并且反复抛出这个异常(因为这里你不是失败，而是准确的并且故意抛出一个异常)。

    ```Ruby
    begin
      fail 'Oops'
    rescue => error
      raise if error.message != 'Oops'
    end
    ```

* 不要为 `fail/raise` 指定准确的 `RuntimeError`。

    ```Ruby
    # bad
    fail RuntimeError, 'message'

    # good - signals a RuntimeError by default
    fail 'message'
    ```

* 宁愿提供一个异常类和一条消息作为 `fail/raise` 的两个参数，而不是一个异常实例。

    ```Ruby
    # bad
    fail SomeException.new('message')
    # Note that there is no way to do `fail SomeException.new('message'), backtrace`.

    # good
    fail SomeException, 'message'
    # Consistent with `fail SomeException, 'message', backtrace`.
    ```

* 不要在 `ensure` 块中返回。如果你明确的从 `ensure` 块中的某个方法中返回，返回将会优于任何抛出的异常，并且尽管没有异常抛出也会返回。实际上异常将会静静的溜走。

    ```Ruby
    def foo
      begin
        fail
      ensure
        return 'very bad idea'
      end
    end
    ```

* Use **implicit begin blocks** when possible.如果可能使用**隐式 `begin` 代码块**。

    ```Ruby
    # bad
    def foo
      begin
        # main logic goes here
      rescue
        # failure handling goes here
      end
    end

    # good
    def foo
      # main logic goes here
    rescue
      # failure handling goes here
    end
    ```

* 通过 *contingency methods* 偶然性方法。 (一个由 Avdi Grimm 创造的词) 来减少 `begin` 区块的使用。

    ```Ruby
    # bad
    begin
      something_that_might_fail
    rescue IOError
      # handle IOError
    end

    begin
      something_else_that_might_fail
    rescue IOError
      # handle IOError
    end

    # good
    def with_io_error_handling
       yield
    rescue IOError
      # handle IOError
    end

    with_io_error_handling { something_that_might_fail }

    with_io_error_handling { something_else_that_might_fail }
    ```

* 不要抑制异常输出。

    ```Ruby
    # bad
    begin
      # an exception occurs here
    rescue SomeError
      # the rescue clause does absolutely nothing
    end

    # bad
    do_something rescue nil
    ```

* 避免使用 `rescue` 的修饰符形式。

    ```Ruby
    # bad - this catches exceptions of StandardError class and its descendant classes
    read_file rescue handle_error($!)

    # good - this catches only the exceptions of Errno::ENOENT class and its descendant classes
    def foo
      read_file
    rescue Errno::ENOENT => ex
      handle_error(ex)
    end
    ```

* 不要用异常来控制流。

    ```Ruby
    # bad
    begin
      n / d
    rescue ZeroDivisionError
      puts "Cannot divide by 0!"
    end

    # good
    if d.zero?
      puts "Cannot divide by 0!"
    else
      n / d
    end
    ```

* 应该总是避免拦截(最顶级的) `Exception` 异常类。这里(ruby自身)将会捕获信号并且调用 `exit`，需要你使用 `kill -9` 杀掉进程。

    ```Ruby
    # bad
    begin
      # calls to exit and kill signals will be caught (except kill -9)
      exit
    rescue Exception
      puts "you didn't really want to exit, right?"
      # exception handling
    end

    # good
    begin
      # a blind rescue rescues from StandardError, not Exception as many
      # programmers assume.
    rescue => e
      # exception handling
    end

    # also good
    begin
      # an exception occurs here

    rescue StandardError => e
      # exception handling
    end

    ```

* 将更具体的异常放在救援（rescue）链的上方，否则他们将不会被救援。

    ```Ruby
    # bad
    begin
      # some code
    rescue Exception => e
      # some handling
    rescue StandardError => e
      # some handling
    end

    # good
    begin
      # some code
    rescue StandardError => e
      # some handling
    rescue Exception => e
      # some handling
    end
    ```

* 在 `ensure` 区块中释放你程式获得的外部资源。

    ```Ruby
    f = File.open('testfile')
    begin
      # .. process
    rescue
      # .. handle error
    ensure
      f.close unless f.nil?
    end
    ```


* 除非必要, 尽可能使用 Ruby 标准库中异常类，而不是引入一个新的异常类。(而不是派生自己的异常类)

