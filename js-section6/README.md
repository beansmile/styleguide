# 属性

- 使用 `.` 来访问对象的属性。

  ```javascript
  var luke = {
    jedi: true,
    age: 28
  };

  // bad
  var isJedi = luke['jedi'];

  // good
  var isJedi = luke.jedi;
  ```

- 当通过变量访问属性时使用中括号 `[]`。

  ```javascript
  var luke = {
    jedi: true,
    age: 28
  };

  function getProp(prop) {
    return luke[prop];
  }

  var isJedi = getProp('jedi');
