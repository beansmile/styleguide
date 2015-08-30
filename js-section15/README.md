# 转换类型

- 在语句开始时执行类型转换。
- 字符串：

  ```javascript
  //  => this.reviewScore = 9;

  // bad
  var totalScore = this.reviewScore + '';

  // good
  var totalScore = '' + this.reviewScore;

  // bad
  var totalScore = '' + this.reviewScore + ' total score';

  // good
  var totalScore = this.reviewScore + ' total score';
  ```

- 使用 `parseInt` 转换数字时总是带上类型转换的基数。

  ```javascript
  var inputValue = '4';

  // bad
  var val = new Number(inputValue);

  // bad
  var val = +inputValue;

  // bad
  var val = inputValue >> 0;

  // bad
  var val = parseInt(inputValue);

  // good
  var val = Number(inputValue);

  // good
  var val = parseInt(inputValue, 10);
  ```

- 如果因为某些原因 `parseInt` 成为你所做的事的瓶颈而需要使用位操作解决[性能问题](http://jsperf.com/coercion-vs-casting/3)时，留个注释说清楚原因和你的目的。

  ```javascript
  // good
  /**
   * parseInt was the reason my code was slow.
   * Bitshifting the String to coerce it to a
   * Number made it a lot faster.
   */
  var val = inputValue >> 0;
  ```

- **注：** 小心使用位操作运算符。数字会被当成 [64 位值](http://es5.github.io/#x4.3.19)，但是位操作运算符总是返回 32 位的整数（[source](http://es5.github.io/#x11.7)）。位操作处理大于 32 位的整数值时还会导致意料之外的行为。[讨论](https://github.com/airbnb/javascript/issues/109)。最大的 32 位整数是 2,147,483,647：

  ```javascript
  2147483647 >> 0 //=> 2147483647
  2147483648 >> 0 //=> -2147483648
  2147483649 >> 0 //=> -2147483647
  ```

- 布尔:

  ```javascript
  var age = 0;

  // bad
  var hasAge = new Boolean(age);

  // good
  var hasAge = Boolean(age);

  // good
  var hasAge = !!age;
  ```
