# 变量

- 总是使用 `var` 来声明变量。不这么做将导致产生全局变量。我们要避免污染全局命名空间。

 ```javascript
 // bad
 superPower = new SuperPower();

 // good
 var superPower = new SuperPower();
 ```

- 使用 `var` 声明每一个变量。
 这样做的好处是增加新变量将变的更加容易，而且你永远不用再担心调换错 `;` 跟 `,`。

 ```javascript
 // bad
 var items = getItems(),
     goSportsTeam = true,
     dragonball = 'z';

 // bad
 // （跟上面的代码比较一下，看看哪里错了）
 var items = getItems(),
     goSportsTeam = true;
     dragonball = 'z';

 // good
 var items = getItems();
 var goSportsTeam = true;
 var dragonball = 'z';
 ```

- 最后再声明未赋值的变量。当你需要引用前面的变量赋值时这将变的很有用。

 ```javascript
 // bad
 var i, len, dragonball,
     items = getItems(),
     goSportsTeam = true;

 // bad
 var i;
 var items = getItems();
 var dragonball;
 var goSportsTeam = true;
 var len;

 // good
 var items = getItems();
 var goSportsTeam = true;
 var dragonball;
 var length;
 var i;
 ```

- 不要创建隐含的全局变量

  ```
  // bad
  function sum(x,y) {

    // 不推荐写法，隐式全局变量

    result = x + y;

    return result;

  }

  // bad
  function foo() {

    var x = y = 0; // x为局部变量，y为全局变量

    // ...

  }

  // good
  function sum(x,y) {

    var result = x + y;

    return result;

  }

  // good
  function foo() {

    var x, y;

    // ... x = y = 0; // 两个均为局部变量

  }

  ```
