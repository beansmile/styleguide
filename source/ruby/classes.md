# 类与模块

* 在 `class` 定义里使用一致的结构。

    ```Ruby
    class Person
      # extend and include go first
      extend SomeModule
      include AnotherModule

      # constants are next
      SOME_CONSTANT = 20

      # afterwards we have attribute macros
      attr_reader :name

      # followed by other macros (if any)
      validates :name

      # public class methods are next in line
      def self.some_method
      end

      # followed by public instance methods
      def some_method
      end

      # protected and private methods are grouped near the end
      protected

      def some_protected_method
      end

      private

      def some_private_method
      end
    end
    ```

* 倾向使用 `module`，而不是只有类方法的 `class`。类别应该只在创建实例是合理的时候使用。

    ```Ruby
    # bad
    class SomeClass
      def self.some_method
        # body omitted
      end

      def self.some_other_method
      end
    end

    # good
    module SomeClass
      module_function

      def some_method
        # body omitted
      end

      def some_other_method
      end
    end
    ```

* 当你希望将模块的实例方法变成 `class` 方法时，偏爱使用 `module_function` 胜过 `extend self `。

    ```Ruby
    # bad
    module Utilities
      extend self

      def parse_something(string)
        # do stuff here
      end

      def other_utility_method(number, string)
        # do some more stuff
      end
    end

    # good
    module Utilities
      module_function

      def parse_something(string)
        # do stuff here
      end

      def other_utility_method(number, string)
        # do some more stuff
      end
    end
    ```

* When designing class hierarchies make sure that they conform to the
  [Liskov Substitution Principle](http://en.wikipedia.org/wiki/Liskov_substitution_principle).

* 在设计类层次的时候确保他们符合 [Liskov Substitution Principle](http://en.wikipedia.org/wiki/Liskov_substitution_principle) 原则。(译者注: LSP原则大概含义为: 如果一个函数中引用了 `父类的实例`, 则一定可以使用其子类的实例替代, 并且函数的基本功能不变. (虽然功能允许被扩展))
>Liskov替换原则：子类型必须能够替换它们的基类型 <br/>
> 1. 如果每一个类型为T1的对象o1，都有类型为T2的对象o2，使得以T1定义的所有程序P在所有的对象o1都代换为o2时,程序P的行为没有变化，那么类型T2是类型T1的子类型。 <br/>
> 2. 换言之，一个软件实体如果使用的是一个基类的话，那么一定适用于其子类，而且它根本不能察觉出基类对象和子类对象的区别。只有衍生类替换基类的同时软件实体的功能没有发生变化，基类才能真正被复用。 <br/>
> 3. 里氏代换原则由Barbar Liskov(芭芭拉.里氏)提出，是继承复用的基石。 <br/>
> 4. 一个继承是否符合里氏代换原则，可以判断该继承是否合理（是否隐藏有缺陷）。

* 努力使你的类尽可能的健壮 [SOLID](http://en.wikipedia.org/wiki/SOLID_(object-oriented_design\))。
* 总是为你自己的类提供 `to_s` 方法, 用来表现这个类（实例）对象包含的对象.

    ```Ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def to_s
        "#@first_name #@last_name"
      end
    end
    ```

* 使用 `attr` 功能成员来定义各个实例变量的访问器或者修改器方法。

    ```Ruby
    # bad
    class Person
      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def first_name
        @first_name
      end

      def last_name
        @last_name
      end
    end

    # good
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end
    ```

* 避免使用 `attr`。使用 `attr_reader` 和 `attr_accessor` 作为替代。

    ```Ruby
    # bad - creates a single attribute accessor (deprecated in 1.9)
    attr :something, true
    attr :one, :two, :three # behaves as attr_reader

    # good
    attr_accessor :something
    attr_reader :one, :two, :three
    ```


* 考虑使用 `Struct.new`, 它可以定义一些琐碎的 `accessors`,
`constructor`（构造函数） 和 `comparison`（比较） 操作。

    ```Ruby
    # good
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end

    # better
    class Person < Struct.new(:first_name, :last_name)
    end
    ````

* 考虑使用 `Struct.new`，它替你定义了那些琐碎的存取器（accessors），构造器（constructor）以及比较操作符（comparison operators）。

    ```Ruby
    # good
    class Person
      attr_accessor :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end

    # better
    Person = Struct.new(:first_name, :last_name) do
    end
    ````

* 不要去 `extend` 一个 `Struct.new` - 它已经是一个新的 `class`。扩展它会产生一个多余的 `class` 层级
  并且可能会产生怪异的错误如果文件被加载多次。

* 考虑添加工厂方法来提供灵活的方法来创建特定类实例。

    ```Ruby
    class Person
      def self.create(potions_hash)
        # body omitted
      end
    end
    ```

* 鸭子类型（[duck-typing](http://en.wikipedia.org/wiki/Duck_typing)）优于继承。

    ```Ruby
    # bad
    class Animal
      # abstract method
      def speak
      end
    end

    # extend superclass
    class Duck < Animal
      def speak
        puts 'Quack! Quack'
      end
    end

    # extend superclass
    class Dog < Animal
      def speak
        puts 'Bau! Bau!'
      end
    end

    # good
    class Duck
      def speak
        puts 'Quack! Quack'
      end
    end

    class Dog
      def speak
        puts 'Bau! Bau!'
      end
    end
    ```

* Avoid the usage of class (`@@`) variables due to their "nasty" behavior
in inheritance.
* 避免使用类变量（`@@`）因为他们讨厌的继承习惯（在子类中也可以修改父类的类变量）。

    ```Ruby
    class Parent
      @@class_var = 'parent'

      def self.print_class_var
        puts @@class_var
      end
    end

    class Child < Parent
      @@class_var = 'child'
    end

    Parent.print_class_var # => will print "child"
    ```

    正如上例看到的, 所有的子类共享类变量, 并且可以直接修改类变量,此时使用类实例变量是更好的主意.

* 根据方法的用途为他们分配合适的可见度( `private`, `protected` )，不要让所有的方法都是 `public` (这是默认设定)。这是 *Ruby* 不是 *Python*。
* `public`, `protected`, 和 `private` 等可见性关键字应该和其（指定）的方法具有相同的缩进。并且不同的可见性关键字之间留一个空格。

    ```Ruby
    class SomeClass
      def public_method
        # ...
      end

      private

      def private_method
        # ...
      end

      def another_private_method
        # ...
      end
    end
    ```

* 使用 `def self.method` 来定义单例方法. 当代码重构时, 这将使得代码更加容易因为类名是不重复的.

    ```Ruby
    class TestClass
      # bad
      def TestClass.some_method
        # body omitted
      end

      # good
      def self.some_other_method
        # body omitted
      end

      # Also possible and convenient when you
      # have to define many singleton methods.
      class << self
        def first_method
          # body omitted
        end

        def second_method_etc
          # body omitted
        end
      end
    end
    ```

    ```Ruby
    class SingletonTest
      def size
        25
      end
    end

    test1 = SingletonTest.new
    test2 = SingletonTest.new
    def test2.size
      10
    end
    test1.size # => 25
    test2.size # => 10
    ```
    本例中，test1 與 test2 屬於同一類別，但 test2 具有重新定義的 size 方法，因此兩者的行為會不一樣。只給予單一物件的方法稱為单例方法 (singleton method)。

