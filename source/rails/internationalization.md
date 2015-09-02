# 国际化

* 视图、模型与控制器里不应使用语言相关设置与字串。这些文字应搬到在 `config/locales` 下的语言文件里。
* 当 ActiveRecord 模型的标签需要被翻译时，使用`activerecord` 作用域:

    ```
    en:
      activerecord:
        models:
          user: Member
        attributes:
          user:
            name: "Full name"
    ```

    然后 `User.model_name.human` 会返回 "Member" ，而 `User.human_attribute_name("name")` 会返回 "Full name"。这些属性的翻译会被视图作为标签使用。

* 把在视图使用的文字与 ActiveRecord 的属性翻译分开。 把给模型使用的语言文件放在名为 `models` 的文件夹，给视图使用的文字放在名为 `views` 的文件夹。
  * 当使用额外目录的语言文件组织完成时，为了要载入这些目录，要在 `application.rb` 文件里描述这些目录。

        ```Ruby
        # config/application.rb
        config.i18n.load_path += Dir[Rails.root.join('config', 'locales', '**', '*.{rb,yml}')]
        ```

* 把共享的本土化选项，像是日期或货币格式，放在 `locales` 的根目录下。
* 使用精简形式的 I18n 方法： `I18n.t` 来取代 `I18n.translate` 以及使用 `I18n.l` 取代 `I18n.localize`。
* 使用 "懒惰" 查询视图中使用的文字。假设我们有以下结构：

    ```
    en:
      users:
        show:
          title: "User details page"
    ```

    `users.show.title` 的数值能这样被 `app/views/users/show.html.haml` 查询：

    ```Ruby
    = t '.title'
    ```

* 在控制器与模型使用点分隔的键，来取代指定 `:scope` 选项。点分隔的调用更容易阅读及追踪层级。

    ```Ruby
    # 这样子调用
    I18n.t 'activerecord.errors.messages.record_invalid'

    # 而不是这样
    I18n.t :record_invalid, :scope => [:activerecord, :errors, :messages]
    ```

* 关于 Rails i18n 更详细的信息可以在这里找到 [Rails Guides](http://guides.rubyonrails.org/i18n.html)。
