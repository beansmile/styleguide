# 常量

- 常量的形式如: NAMES_LIKE_THIS, 即使用大写字符, 并用下划线分隔.

- 对于基本类型的常量, 只需转换命名.

  ```
  /**
   * The number of seconds in a minute.
   * @type {number}
   */
  goog.example.SECONDS_IN_A_MINUTE = 60;
  ```

- 对于非基本类型, 使用 @const 标记.

  ```
  /**
   * The number of seconds in each of the given units.
   * @type {Object.<number>}
   * @const
   */
  goog.example.SECONDS_TABLE = {
    minute: 60,
    hour: 60 * 60,
    day: 60 * 60 * 24
  }
  ```
