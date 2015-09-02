# 代码格式化

- 大括号

  务必在同一行上插入大括号. 例如:

  ```
  if (something) {
    // ...
  } else {
    // ...
  }
  ```

- 数组和对象的初始化

  如果初始值不是很长, 就保持写在单行上:

  ```
  var arr = [1, 2, 3];
  var obj = {a: 1, b: 2, c: 3};
  ```

- 初始值占用多行时, 缩进2个空格.

  ```
  // Object initializer.
  var inset = {
    top: 10,
    right: 20,
    bottom: 15,
    left: 12
  };

  // Array initializer.
  this.rows_ = [
    '"Slartibartfast" <fjordmaster@magrathea.com>',
    '"Zaphod Beeblebrox" <theprez@universe.gov>',
    '"Ford Prefect" <ford@theguide.com>',
    '"Arthur Dent" <has.no.tea@gmail.com>',
    '"Marvin the Paranoid Android" <marv@googlemail.com>',
    'the.mice@magrathea.com'
  ];
  ```

- 比较长的标识符或者数值, 不要为了让代码好看些而手工对齐. 如:

  ```
  CORRECT_Object.prototype = {
    a: 0,
    b: 1,
    lengthyName: 2
  };
  ```

  不要这样做:

  ```
  WRONG_Object.prototype = {
    a          : 0,
    b          : 1,
    lengthyName: 2
  };
  ```
