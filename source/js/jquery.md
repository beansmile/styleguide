# jQuery

- 使用 `$` 作为存储 jQuery 对象的变量名前缀。

  ```javascript
  // bad
  var sidebar = $('.sidebar');

  // good
  var $sidebar = $('.sidebar');
  ```

- 缓存 jQuery 查询。

  ```javascript
  // bad
  function setSidebar() {
    $('.sidebar').hide();

    // ...stuff...

    $('.sidebar').css({
      'background-color': 'pink'
    });
  }

  // good
  function setSidebar() {
    var $sidebar = $('.sidebar');
    $sidebar.hide();

    // ...stuff...

    $sidebar.css({
      'background-color': 'pink'
    });
  }
  ```

- 更多jQuery最佳实践参考

  [http://www.ruanyifeng.com/blog/2011/08/jquery_best_practices.html](http://www.ruanyifeng.com/blog/2011/08/jquery_best_practices.html)
