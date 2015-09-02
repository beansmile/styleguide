# 配置

* 把惯用的初始化代码放在 `config/initializers`。 在 initializers 内的代码于应用启动时执行。
* 每一个 gem 相关的初始化代码应当使用同样的名称，放在不同的文件里，如： `carrierwave.rb`, `active_admin.rb`, 等等。
* 相应调整配置开发、测试及生产环境（在 `config/environments/` 下对应的文件）
  * 添加需要预编译的额外静态资源文件（如果有的话）：

    ```Ruby
    # config/environments/production.rb
    # 预编译额外的静态资源文件(application.js, application.css, 以及所有已经被加入的非 JS 或 CSS 的文件)
    config.assets.precompile += %w( rails_admin/rails_admin.css rails_admin/rails_admin.js )
    ```

* 将所有环境皆通用的配置档放在 `config/application.rb` 文件。
* 构建一个与生产环境(production enviroment)相似的，一个额外的 `staging` 环境。
