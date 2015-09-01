# 命名规则

- 避免单字母命名。命名应具备描述性。

  ```javascript
  // bad
  function q() {
    // ...stuff...
  }

  // good
  function query() {
    // ..stuff..
  }
  ```

- 使用驼峰式命名对象、函数和实例。

  ```javascript
  // bad
  var OBJEcttsssss = {};
  var this_is_my_object = {};
  var o = {};
  function c() {}

  // good
  var thisIsMyObject = {};
  function thisIsMyFunction() {}
  ```

- 使用帕斯卡式命名构造函数或类。

  ```javascript
  // bad
  function user(options) {
    this.name = options.name;
  }

  var bad = new user({
    name: 'nope'
  });

  // good
  function User(options) {
    this.name = options.name;
  }

  var good = new User({
    name: 'yup'
  });
  ```

- 使用下划线 `_` 开头命名私有属性。

  ```javascript
  // bad
  this.__firstName__ = 'Panda';
  this.firstName_ = 'Panda';

  // good
  this._firstName = 'Panda';
  ```

- 使用 `_this` 保存 `this` 的引用。

  ```javascript
  // bad
  function() {
    var self = this;
    return function() {
      console.log(self);
    };
  }

  // bad
  function() {
    var that = this;
    return function() {
      console.log(that);
    };
  }

  // good
  function() {
    var _this = this;
    return function() {
      console.log(_this);
    };
  }
  ```

- 给函数命名。

  ```javascript
  // bad
  var log = function(msg) {
    console.log(msg);
  };

  // good
  var log = function log(msg) {
    console.log(msg);
  };
  ```

- **注：** IE8 及以下版本对命名函数表达式的处理有些怪异。了解更多信息到 [http://kangax.github.io/nfe/](http://kangax.github.io/nfe/)。
