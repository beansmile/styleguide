# Assets

利用这个 [assets pipeline](http://guides.rubyonrails.org/asset_pipeline.html) 来管理应用的结构。

* 保留 `app/assets` 给自定的样式表，Javascripts 或图片。
* 把自己开发，但不适合用在这个应用的函式库，放在 `lib/assets/`。
* 第三方代码如： [jQuery](http://jquery.com/) 或 [bootstrap](http://twitter.github.com/bootstrap/) 应放置在 `vendor/assets`。
* 当可能的时候，使用 gem 化的 assets 版本。(如： [jquery-rails](https://github.com/rails/jquery-rails))。

## Mailers

* 把 mails 命名为 `SomethingMailer`。 没有 Mailer 字根的话，不能立即显现哪个是一个 Mailer，以及哪个视图与它有关。
* 提供 HTML 与纯文本视图模版。
* 在你的开发环境启用信件失败发送错误。这些错误缺省是被停用的。

    ```Ruby
    # config/environments/development.rb

    config.action_mailer.raise_delivery_errors = true
    ```

* 在开发模式使用 `smtp.gmail.com` 设置 SMTP 服务器（当然了，除非你自己有本地 SMTP 服务器）。

    ```Ruby
    # config/environments/development.rb

    config.action_mailer.smtp_settings = {
      address: 'smtp.gmail.com',
      # 更多设置
    }
    ```

* 提供缺省的配置给主机名。

    ```Ruby
    # config/environments/development.rb
    config.action_mailer.default_url_options = {host: "#{local_ip}:3000"}


    # config/environments/production.rb
    config.action_mailer.default_url_options = {host: 'your_site.com'}

    # 在你的 mailer 类
    default_url_options[:host] = 'your_site.com'
    ```

* 如果你需要在你的网站使用一个 email 链结，总是使用 `_url` 方法，而不是 `_path` 方法。 `_url` 方法包含了主机名，而 `_path` 方法没有。

    ```Ruby
    # 错误
    You can always find more info about this course
    = link_to 'here', url_for(course_path(@course))

    # 正确
    You can always find more info about this course
    = link_to 'here', url_for(course_url(@course))
    ```

* 正确地显示寄与收件人地址的格式。使用下列格式：

    ```Ruby
    # 在你的 mailer 类别
    default from: 'Your Name <info@your_site.com>'
    ```

* 确定测试环境的 email 发送方法设置为 `test` ：

    ```Ruby
    # config/environments/test.rb

    config.action_mailer.delivery_method = :test
    ```

* 开发与生产环境的发送方法应为 `smtp` ：

    ```Ruby
    # config/environments/development.rb, config/environments/production.rb

    config.action_mailer.delivery_method = :smtp
    ```

* 当发送 HTML email 时，所有样式应为行内样式，由于某些用户有关于外部样式的问题。某种程度上这使得更难管理及造成代码重用。有两个相似的 gem 可以转换样式，以及将它们放在对应的 html 标签里： [premailer-rails3](https://github.com/fphilipe/premailer-rails3) 和 [roadie](https://github.com/Mange/roadie)。

* 应避免页面产生响应时寄送 email。若多个 email 寄送时，造成了页面载入延迟，以及请求可能逾时。使用 [delayed_job](https://github.com/tobi/delayed_job) gem 的帮助来克服在背景处理寄送 email 的问题。
