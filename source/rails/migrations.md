# 迁移

* 把 `schema.rb` 保存在版本管控之下。
* 使用 `rake db:scheme:load` 取代 `rake db:migrate` 来初始化空的数据库。
* 使用 `rake db:test:prepare` 来更新测试数据库的 schema。
* 在迁移中使用`say_with_time`输出一些有用的log信息
* 避免在表里设置缺省数据。使用模型层来取代。

    ```Ruby
    def amount
      self[:amount] or 0
    end
    ```

    然而 `self[:attr_name]` 的使用被视为相当常见的，你也可以考虑使用更罗嗦的（争议地可读性更高的） `read_attribute` 来取代：

    ```Ruby
    def amount
      read_attribute(:amount) or 0
    end
    ```

* 当编写建设性的迁移时（加入表或栏位），使用 Rails 3.1 的新方式来迁移 - 使用 `change` 方法取代 `up` 与 `down` 方法。

    ```Ruby
    # 过去的方式
    class AddNameToPerson < ActiveRecord::Migration
      def up
        add_column :persons, :name, :string
      end

      def down
        remove_column :person, :name
      end
    end

    # 新的偏好方式
    class AddNameToPerson < ActiveRecord::Migration
      def change
        add_column :persons, :name, :string
      end
    end
    ```
