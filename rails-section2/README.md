# 路由

* 当你需要加入一个或多个动作至一个 RESTful 资源时（你真的需要吗？），使用 `member` and `collection` 路由。

    ```Ruby
    # 差
    get 'subscriptions/:id/unsubscribe'
    resources :subscriptions

    # 好
    resources :subscriptions do
      get 'unsubscribe', on: :member
    end

    # 差
    get 'photos/search'
    resources :photos

    # 好
    resources :photos do
      get 'search', on: :collection
    end
    ```

* 若你需要定义多个 `member/collection` 路由时，使用替代的区块语法(block syntax)。

    ```Ruby
    resources :subscriptions do
      member do
        get 'unsubscribe'
        # 更多路由
      end
    end

    resources :photos do
      collection do
        get 'search'
        # 更多路由
      end
    end
    ```

* 使用嵌套路由(nested routes)来更佳地表达与 ActiveRecord 模型的关系。

    ```Ruby
    class Post < ActiveRecord::Base
      has_many :comments
    end

    class Comments < ActiveRecord::Base
      belongs_to :post
    end

    # routes.rb
    resources :posts do
      resources :comments
    end
    ```

* 使用命名空间路由来群组相关的行为。

    ```Ruby
    namespace :admin do
      # Directs /admin/products/* to Admin::ProductsController
      # (app/controllers/admin/products_controller.rb)
      resources :products
    end
    ```

* 不要在控制器里使用留给后人般的疯狂路由(legacy wild controller route)。这种路由会让每个控制器的动作透过 GET 请求存取。

    ```Ruby
    # 非常差
    match ':controller(/:action(/:id(.:format)))'
    ```
