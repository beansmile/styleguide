# Mailers

* 把只给开发环境或测试环境的 gem 适当地分组放在 Gemfile 文件中。
* 在你的项目中只使用公认的 gem。 如果你考虑引入某些鲜为人所知的 gem ，你应该先仔细复查一下它的源代码。
* 关于多个开发者使用不同操作系统的项目，操作系统相关的 gem 缺省会产生一个经常变动的 `Gemfile.lock` 。 在 Gemfile 文件里，所有与 OS X 相关的 gem 放在 `darwin` 群组，而所有 Linux 相关的 gem 放在 `linux` 群组：

    ```Ruby
    # Gemfile
    group :darwin do
      gem 'rb-fsevent'
      gem 'growl'
    end

    group :linux do
      gem 'rb-inotify'
    end
    ```

    要在对的环境获得合适的 gem，添加以下代码至 `config/application.rb` ：

    ```Ruby
    platform = RUBY_PLATFORM.match(/(linux|darwin)/)[0].to_sym
    Bundler.require(platform)
    ```

* 不要把 `Gemfile.lock` 文件从版本控制里移除。这不是随机产生的文件 - 它确保你所有的组员执行 `bundle install` 时，获得相同版本的 gem 。
