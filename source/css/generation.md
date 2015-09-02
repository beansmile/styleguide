# 通用

### 选择器

#### [强制] 如无必要，不得为 `id`、`class` 选择器添加 `类型选择器` 进行限定。

解释：

在性能和维护性上，都有一定的影响。

示例：

```css
/* good */
#error,
.danger-message {
  font-color: #c00;
}

/* bad */
dialog#error,
p.danger-message {
  font-color: #c00;
}
```

#### [建议] 选择器的嵌套层级应不大于 3 级，位置靠后的限定条件应尽可能精确。

示例：

```css
/* good */
#username input {
}
.comment .avatar {
}

/* bad */
.page .header .login #username input {
}
.comment div * {
}
```

### 属性缩写

#### [建议] 在可以使用缩写的情况下，尽量使用属性缩写。

示例：

```css
/* good */
.post {
  font: 12px/1.5 arial, sans-serif;
}

/* bad */
.post {
  font-family: arial, sans-serif;
  font-size: 12px;
  line-height: 1.5;
}
```

#### [建议] 使用 `border` / `margin` / `padding` 等缩写时，应注意隐含值对实际数值的影响，确实需要设置多个方向的值时才使用缩写。

解释：

border / margin / padding 等缩写会同时设置多个属性的值，容易覆盖不需要覆盖的设定。如某些方向需要继承其他声明的值，则应该分开设置。

示例：

```css
/* centering <article class="page"> horizontally
 * and highlight featured ones 
 */
article {
  margin: 5px;
  border: 1px solid #999;
}

/* good */
.page {
  margin-right: auto;
  margin-left: auto;
}

.featured {
  border-color: #69c;
}

/* bad */
.page {
  margin: 5px auto;  /* introducing redundancy */
}

.featured {
  border: 1px solid #69c;  /* introducing redundancy */
}
```

### 属性书写顺序

#### [建议] 同一CSS代码块中的属性在书写时，应按功能进行分组，并以 **Formatting Model（布局方式、位置） > Box Model（尺寸） > Typographic（文本相关） > Visual（视觉效果** 的顺序书写，以提高代码的可读性。

解释：

- Formatting Model 相关属性包括：`position` / `top` / `right` / `bottom` / `left` / `float` / `display` / `overflow` 等
- Box Model 相关属性包括：`border` / `margin` / `padding` / `width` / `height` 等
- Typographic 相关属性包括：`font` / `line-height` / `text-align` / `word-wrap` 等
- Visual 相关属性包括：`background` / `color` / `transition` / `list-style` 等

另外，如果包含 `content` 属性，应放在最前面。

示例：

```css
.sidebar {
  /* formatting model: positioning schemes / offsets / z-indexes / display / ...  */
  position: absolute;
  top: 50px;
  left: 0;
  overflow-x: hidden;

  /* box model: sizes / margins / paddings / borders / ...  */
  width: 200px;
  padding: 5px;
  border: 1px solid #ddd;

  /* typographic: font / aligns / text styles / ... */
  font-size: 14px;
  line-height: 20px;

  /* visual: colors / shadows / gradients / ... */
  background: #f5f5f5;
  color: #333;
  -webkit-transition: color 1s;
     -moz-transition: color 1s;
          transition: color 1s;
}
```

### 清除浮动

#### [建议] 当元素需要撑起高度以包含内部的浮动元素时，通过对伪类设置 `clearfix` 或触发 `BFC` 的方式进行。尽量不使用增加空标签的方式。

#### 清除浮动的方法，具体可以参考[这篇文章](http://www.iyunlu.com/view/css-xhtml/55.html)：

示例：

```css
/* 触发父元素的BFC,以下每个规则是一种方法，不需要同时使用 */
.parent {
  overflow: hidden;  /* 缺点是内容增多时会被隐藏 */
}

.parent {
  display: table;  /* 缺点是改变了原有的盒模型 */
}

.parent {
  float: left;  /* float设置为非none,缺点是影响了父元素所在层次的布局 */
}
```

```css
/* 通过伪元素设置clear: both; */
.clearfix:after {
  content: ".";
  display: block;
  height: 0;
  visibility: hidden;
  clear: both;
}
.clearfix {
  *zoom: 1;
}
```

### !important声明

#### [建议] 尽量不使用 `!important` 声明。

#### [强制] 当需要强制指定样式且不允许任何场景覆盖时，通过标签内联和 `!important` 定义样式，此时必须在注释中说明要使用!important的原因.

解释：

必须注意的是，仅在设计上 `确实不允许任何其它场景覆盖样式` 时，才使用内联的 `!important` 样式。通常在第三方环境的应用中使用这种方案，第三方环境对于开发者完全不可控，比如需要覆盖邮件客户端的默认样式。

示例：

```css
i {
  font-style: italic;!important  /* 强制邮件客户端将i标签显示为斜体 */
}
```

### @import

#### [强制] 不要使用@import

解释：@import 规则只能置于CSS文件的顶部，而且增加了额外的HTTP请求，替代办法有以下几种：

* 使用多个 <link> 元素
* 使用Sass中的import把多个CSS文件合并
* 使用gulp，grunt，rails等合并CSS

### z-index

#### [建议] 将 `z-index` 进行分层，对文档流外绝对定位元素的视觉层级关系进行管理

* `position: relative;` 的元素，`z-index`范围为 `0 ~ 1000`
* `position: absolute;` 的元素，`z-index`范围为 `1000 ~ 9999`
* `position: fixed;` 的元素， `z-index`范围为 `9999+`
* 层次相同的元素，可以使用相同的 `z-index`
* 同一父元素下的多个定位元素，`z-index` 间隔为

```scss
.movie {
  position: relative;
  z-index: 0;
}
.gallery {
  position: relative;
  z-index: 100;
  .avatar {
    position: absolute;
    z-index: 1000;
  }
  .thumbnail {
    position: absolute;
    z-index: 1000;
  }
  .popup-photo {
    position: absolute;
    z-index: 1100;
  }
}
.sharelink {
  position: fixed;
  z-index: 9999;
}
```

### class命名

#### [强制] class名称中只能出现小写字符和破折号（中划线），不要使用下划线和驼峰命名法。根据页面组件的功能来命名class，不要按照组件的样式命名

示例：

```css
/* good */
.alert-box {
  background-color: #f00;
}

/* bad */
.redbox {
  background-color: #f00;
}
```

#### [强制] 不允许采用没有意义的简写，`.btn` 能清晰明了地代表button，`.s`、`.p` 不能表达任何意思

#### [建议] 使用带 `js-`前缀 的class作为JS行为的钩子，并且不要在CSS中使用这些class
