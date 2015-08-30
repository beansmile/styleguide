# 字符串

- 使用单引号 `''` 包裹字符串。

  ```javascript
  // bad
  var name = "Bob Parr";

  // good
  var name = 'Bob Parr';

  // bad
  var fullName = "Bob " + this.lastName;

  // good
  var fullName = 'Bob ' + this.lastName;
  ```

- 超过 80 个字符的字符串应该使用连接符写成多行。
- 注：若过渡使用，通过连接符连接的长字符串可能会影响性能。[jsPerf](http://jsperf.com/ya-string-concat) & [讨论](https://github.com/airbnb/javascript/issues/40).

  ```javascript
  // bad
  var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

  // bad
  var errorMessage = 'This is a super long error that was thrown because \
  of Batman. When you stop to think about how Batman had anything to do \
  with this, you would get nowhere \
  fast.';

  // good
  var errorMessage = 'This is a super long error that was thrown because ' +
    'of Batman. When you stop to think about how Batman had anything to do ' +
    'with this, you would get nowhere fast.';
  ```

- 程序化生成的字符串使用 Array#join 连接而不是使用连接符。尤其是 IE 下：[jsPerf](http://jsperf.com/string-vs-array-concat/2).

  ```javascript
  var items;
  var messages;
  var length;
  var i;

  messages = [{
    state: 'success',
    message: 'This one worked.'
  }, {
    state: 'success',
    message: 'This one worked as well.'
  }, {
    state: 'error',
    message: 'This one did not work.'
  }];

  length = messages.length;

  // bad
  function inbox(messages) {
    items = '<ul>';

    for (i = 0; i < length; i++) {
      items += '<li>' + messages[i].message + '</li>';
    }

    return items + '</ul>';
  }

  // good
  function inbox(messages) {
    items = [];

    for (i = 0; i < length; i++) {
      // use direct assignment in this case because we're micro-optimizing.
      items[i] = '<li>' + messages[i].message + '</li>';
    }

    return '<ul>' + items.join('') + '</ul>';
  }
  ```
