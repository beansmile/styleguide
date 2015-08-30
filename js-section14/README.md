# 分号

- **使用分号。**

  ```javascript
  // bad
  (function() {
    var name = 'Skywalker'
    return name
  })()

  // good
  (function() {
    var name = 'Skywalker';
    return name;
  })();

  // good (防止函数在两个 IIFE 合并时被当成一个参数
  ;(function() {
    var name = 'Skywalker';
    return name;
  })();
  ```

  [了解更多](http://stackoverflow.com/a/7365214/1712802).
