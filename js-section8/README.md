# 提升

- 变量声明会提升至作用域顶部，但赋值不会。

  ```javascript
  // 我们知道这样不能正常工作（假设这里没有名为 notDefined 的全局变量）
  function example() {
    console.log(notDefined); // => throws a ReferenceError
  }

  // 但由于变量声明提升的原因，在一个变量引用后再创建它的变量声明将可以正常工作。
  // 注：变量赋值为 `true` 不会提升。
  function example() {
    console.log(declaredButNotAssigned); // => undefined
    var declaredButNotAssigned = true;
  }

  // 解释器会把变量声明提升到作用域顶部，意味着我们的例子将被重写成：
  function example() {
    var declaredButNotAssigned;
    console.log(declaredButNotAssigned); // => undefined
    declaredButNotAssigned = true;
  }
  ```

- 匿名函数表达式会提升它们的变量名，但不会提升函数的赋值。

  ```javascript
  function example() {
    console.log(anonymous); // => undefined

    anonymous(); // => TypeError anonymous is not a function

    var anonymous = function() {
      console.log('anonymous function expression');
    };
  }
  ```

- 命名函数表达式会提升变量名，但不会提升函数名或函数体。

  ```javascript
  function example() {
    console.log(named); // => undefined

    named(); // => TypeError named is not a function

    superPower(); // => ReferenceError superPower is not defined

    var named = function superPower() {
      console.log('Flying');
    };
  }

  // 当函数名跟变量名一样时，表现也是如此。
  function example() {
    console.log(named); // => undefined

    named(); // => TypeError named is not a function

    var named = function named() {
      console.log('named');
    }
  }
  ```

- 函数声明提升它们的名字和函数体。

  ```javascript
  function example() {
    superPower(); // => Flying

    function superPower() {
      console.log('Flying');
    }
  }
  ```

- 了解更多信息在 [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) by [Ben Cherry](http://www.adequatelygood.com/).
