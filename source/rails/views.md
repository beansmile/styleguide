# 视图

* 不要直接从视图调用模型层。
* 不要在视图构造复杂的格式，把它们输出到视图 helper 的一个方法或是模型。
* 使用 partial 模版与布局来减少重复的代码。
* 加入 [client side validation](https://github.com/bcardarella/client_side_validations) 至惯用的 validators。 要做的步骤有：
  * 声明一个由 `ClientSideValidations::Middleware::Base` 而来的自定 validator

      ```Ruby
      module ClientSideValidations::Middleware
        class Email < Base
          def response
            if request.params[:email] =~ /\A([^@\s]+)@((?:[-a-z0-9]+\.)+[a-z]{2,})\z/i
              self.status = 200
            else
              self.status = 404
            end
            super
          end
        end
      end
      ```

  * 建立一个新文件
    `public/javascripts/rails.validations.custom.js.coffee` 并在你的 `application.js.coffee` 文件加入一个它的参照：

      ```Ruby
      # app/assets/javascripts/application.js.coffee
      #= require rails.validations.custom
      ```

  * 添加你的用户端 validator：

      ```Ruby
      #public/javascripts/rails.validations.custom.js.coffee
      clientSideValidations.validators.remote['email'] = (element, options) ->
        if $.ajax({
          url: '/validators/email.json',
          data: { email: element.val() },
          async: false
        }).status == 404
          return options.message || 'invalid e-mail format'
      ```
