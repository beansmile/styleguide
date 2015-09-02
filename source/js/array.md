# 数组

- 使用直接量创建数组。

  ```javascript
  // bad
  var items = new Array();

  // good
  var items = [];
  ```

- 向数组增加元素时使用 Array#push 来替代直接赋值。

  ```
  var someStack = [];

  // bad
  someStack[someStack.length] = 'abracadabra';

  // good
  someStack.push('abracadabra');
  ```

- 当你需要拷贝数组时，使用 Array#slice。[jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

  ```javascript
  var len = items.length;
  var itemsCopy = [];
  var i;

  // bad
  for (i = 0; i < len; i++) {
    itemsCopy[i] = items[i];
  }

  // good
  itemsCopy = items.slice();
  ```
