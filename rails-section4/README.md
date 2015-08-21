# 模型

* Model Skeleton example
    ```Ruby
    class ModelName < ActiveRecord::Base
      # constants

      # concerns

      # attr related macros

      # association macros

      # validation macros

      # callbacks

      # other macros

      # scopes

      # class methods

      # instance methods

    end
    ```
* 自由地引入不是 ActiveRecord 的类别吧。
* 替模型命名有意义（但简短）且不带缩写的名字。
* 如果你需要模型有著 ActiveRecord 行为的对象，比方说验证这一块，使用 [ActiveAttr](https://github.com/cgriego/active_attr) gem。

    ```Ruby
    class Message
      include ActiveAttr::Model

      attribute :name
      attribute :email
      attribute :content
      attribute :priority

      attr_accessible :name, :email, :content

      validates_presence_of :name
      validates_format_of :email, :with => /\A[-a-z0-9_+\.]+\@([-a-z0-9]+\.)+[a-z0-9]{2,4}\z/i
      validates_length_of :content, :maximum => 500
    end
    ```

    更完整的示例，参考 [RailsCast on the subject](http://railscasts.com/episodes/326-activeattr)。

### ActiveRecord

* 避免改动缺省的 ActiveRecord（表的名字、主键，等等），除非你有一个非常好的理由（像是不受你控制的数据库）。
* 把宏风格的方法（`has_many`, `validates`, 等等）放在类别定义的前面。

    ```Ruby
    class User < ActiveRecord::Base
      # 默认的scope放在最前面(如果有)
      default_scope { where(active: true) }

      # 接下来是常量
      GENDERS = %w(male female)

      # 然后放一些attr相关的宏
      attr_accessor :formatted_date_of_birth

      attr_accessible :login, :first_name, :last_name, :email, :password

      # 仅接着是关联的宏
      belongs_to :country

      has_many :authentications, dependent: :destroy

      # 以及宏的验证
      validates :email, presence: true
      validates :username, presence: true
      validates :username, uniqueness: { case_sensitive: false }
      validates :username, format: { with: /\A[A-Za-z][A-Za-z0-9._-]{2,19}\z/ }
      validates :password, format: { with: /\A\S{8,128}\z/, allow_nil: true}

      # 接着是回调
      before_save :cook
      before_save :update_username_lower

      # 其它的宏 (像devise的) 应该放在回调的后面

      ...
    end
    ```

* 偏好 `has_many :through` 胜于 `has_and_belongs_to_many`。 使用 `has_many :through` 允许在 join 模型有附加的属性及验证

    ```Ruby
    # 使用 has_and_belongs_to_many
    class User < ActiveRecord::Base
      has_and_belongs_to_many :groups
    end

    class Group < ActiveRecord::Base
      has_and_belongs_to_many :users
    end

    # 偏好方式 - using has_many :through
    class User < ActiveRecord::Base
      has_many :memberships
      has_many :groups, through: :memberships
    end

    class Membership < ActiveRecord::Base
      belongs_to :user
      belongs_to :group
    end

    class Group < ActiveRecord::Base
      has_many :memberships
      has_many :users, through: :memberships
    end
    ```

* 使用新的 ["sexy" validation](http://thelucid.com/2010/01/08/sexy-validation-in-edge-rails-rails-3/)。
* 当一个惯用的验证使用超过一次或验证是某个正则表达映射时，创建一个惯用的 validator 文件。

    ```Ruby
    # 差
    class Person
      validates :email, format: { with: /\A([^@\s]+)@((?:[-a-z0-9]+\.)+[a-z]{2,})\z/i }
    end

    # 好
    class EmailValidator < ActiveModel::EachValidator
      def validate_each(record, attribute, value)
        record.errors[attribute] << (options[:message] || 'is not a valid email') unless value =~ /\A([^@\s]+)@((?:[-a-z0-9]+\.)+[a-z]{2,})\z/i
      end
    end

    class Person
      validates :email, email: true
    end
    ```

* 所有惯用的验证器应放在一个共享的 gem 。
* 自由地使用命名的作用域(scope)。

    ```Ruby
    class User < ActiveRecord::Base
      scope :active, -> { where(active: true) }
      scope :inactive, -> { where(active: false) }

      scope :with_orders, -> { joins(:orders).select('distinct(users.id)') }
    end
    ```

* 将命名的作用域包在 `lambda` 里来惰性地初始化。

    ```Ruby
    # 差劲
    class User < ActiveRecord::Base
      scope :active, where(active: true)
      scope :inactive, where(active: false)

      scope :with_orders, joins(:orders).select('distinct(users.id)')
    end

    # 好
    class User < ActiveRecord::Base
      scope :active, -> { where(active: true) }
      scope :inactive, -> { where(active: false) }

      scope :with_orders, -> { joins(:orders).select('distinct(users.id)') }
    end
    ```

* 当一个由 lambda 及参数定义的作用域变得过于复杂时，更好的方式是建一个作为同样用途的类别方法，并返回一个 `ActiveRecord::Relation` 对象。你也可以这么定义出更精简的作用域。

    ```Ruby
    class User < ActiveRecord::Base
      def self.with_orders
        joins(:orders).select('distinct(users.id)')
      end
    end
    ```

* 注意 `update_attribute` 方法的行为。它不运行模型验证（不同于 `update_attributes` ）并且可能把模型状态给搞砸。
* 使用用户友好的网址。在网址显示具描述性的模型属性，而不只是 `id` 。
有不止一种方法可以达成：
  * 覆写模型的 `to_param` 方法。这是 Rails 用来给对象建构网址的方法。缺省的实作会以字串形式返回该 `id` 的记录。它可被另一个具人类可读的属性覆写。

        ```Ruby
        class Person
          def to_param
            "#{id} #{name}".parameterize
          end
        end
        ```

    为了要转换成对网址友好 (URL-friendly)的数值，字串应当调用 `parameterize` 。 对象的 `id` 要放在开头，以便给 ActiveRecord 的 `find` 方法查找。

  * 使用此 `friendly_id` gem。它允许藉由某些具描述性的模型属性，而不是用 `id` 来创建人类可读的网址。

        ```Ruby
        class Person
          extend FriendlyId
          friendly_id :name, use: :slugged
        end
        ```

        查看 [gem 文档](https://github.com/norman/friendly_id)获得更多关于使用的信息。

### ActiveResource

* 当 HTTP 响应是一个与存在的格式不同的格式时（XML 和 JSON），需要某些额外的格式解析，创一个你惯用的格式，并在类别中使用它。惯用的格式应当实作下列方法：`extension`, `mime_type`,
`encode` 以及 `decode`。

    ```Ruby
    module ActiveResource
      module Formats
        module Extend
          module CSVFormat
            extend self

            def extension
              'csv'
            end

            def mime_type
              'text/csv'
            end

            def encode(hash, options = nil)
              # 数据以新格式编码并返回
            end

            def decode(csv)
              # 数据以新格式解码并返回
            end
          end
        end
      end
    end

    class User < ActiveResource::Base
      self.format = ActiveResource::Formats::Extend::CSVFormat

      ...
    end
    ```

* 若 HTTP 请求应当不扩展发送时，覆写 `ActiveResource::Base` 的 `element_path` 及 `collection_path` 方法，并移除扩展的部份。

    ```Ruby
    class User < ActiveResource::Base
      ...

      def self.collection_path(prefix_options = {}, query_options = nil)
        prefix_options, query_options = split_options(prefix_options) if query_options.nil?
        "#{prefix(prefix_options)}#{collection_name}#{query_string(query_options)}"
      end

      def self.element_path(id, prefix_options = {}, query_options = nil)
        prefix_options, query_options = split_options(prefix_options) if query_options.nil?
        "#{prefix(prefix_options)}#{collection_name}/#{URI.parser.escape id.to_s}#{query_string(query_options)}"
      end
    end
    ```

    如有任何改动网址的需求时，这些方法也可以被覆写。
