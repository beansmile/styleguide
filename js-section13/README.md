# 逗号

- 行首逗号: **不需要**。

  ```javascript
  // bad
  var story = [
      once
    , upon
    , aTime
  ];

  // good
  var story = [
    once,
    upon,
    aTime
  ];

  // bad
  var hero = {
      firstName: 'Bob'
    , lastName: 'Parr'
    , heroName: 'Mr. Incredible'
    , superPower: 'strength'
  };

  // good
  var hero = {
    firstName: 'Bob',
    lastName: 'Parr',
    heroName: 'Mr. Incredible',
    superPower: 'strength'
  };
  ```

- 额外的行末逗号：**不需要**。这样做会在 IE6/7 和 IE9 怪异模式下引起问题。同样，多余的逗号在某些 ES3 的实现里会增加数组的长度。在 ES5 中已经澄清了 ([source](http://es5.github.io/#D))：

> Edition 5 clarifies the fact that a trailing comma at the end of an ArrayInitialiser does not add to the length of the array. This is not a semantic change from Edition 3 but some implementations may have previously misinterpreted this.

  ```javascript
  // bad
  var hero = {
    firstName: 'Kevin',
    lastName: 'Flynn',
  };

  var heroes = [
    'Batman',
    'Superman',
  ];

  // good
  var hero = {
    firstName: 'Kevin',
    lastName: 'Flynn'
  };

  var heroes = [
    'Batman',
    'Superman'
  ];
  ```
