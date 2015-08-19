# beansmile CSS 编码规范

## 前言

本文档是针对CSS设计的，但是使用Sass的时候，大部分规范也同样适用。
本文档的目的是希望团队的CSS代码风格一致，方便他人理解和维护。

#### 注：规范中的遵守要求级别如下：

* 标明 [强制] 的条目必须遵守
* 标明 [建议] 的条目如无特殊情况，优先遵守

## 1 代码风格

### 1.1 缩进

#### [强制] 使用 `2` 个空格做为一个缩进层级，不允许使用 `4` 个空格 或 `tab` 字符。

示例：

```css
.selector {
  margin: 0;
  padding: 0;
}
```

### 1.2 空格

#### [强制] `选择器` 与 `{` 之间必须包含空格。

示例：

```css
.selector {
}
```

#### [强制] `属性名` 与之后的 `:` 之间不允许包含空格， `:` 与 `属性值` 之间必须包含空格。

```css
margin: 0;
```

#### [强制] `列表型属性值` 在单行书写时，`,` 后必须跟一个空格。

示例：

```css
font-family: Arial, sans-serif;
```

### 1.3 选择器

#### [建议] 当一个CSS规则块包含多个选择器时，每个选择器声明必须独占一行。

示例：

```css
/* good */
.post,
.page,
.comment {
  line-height: 1.5;
}

/* bad */
.post, .page, .comment {
  line-height: 1.5;
}
```

#### [强制] `>`、`+`、`~` 选择器的两边各保留一个空格。

示例：

```css
/* good */
main > nav {
  padding: 10px;
}

label + input {
  margin-left: 5px;
}

input:checked ~ button {
  background-color: #69C;
}

/* bad */
main>nav {
  padding: 10px;
}

label+input {
  margin-left: 5px;
}

input:checked~button {
  background-color: #69C;
}
```

#### [强制] 属性选择器中的值必须用双引号包围。

解释：

不允许使用单引号，不允许不使用引号。

示例：

```css
/* good */
a[href^="www"] {
  color: #fff;
}

/* bad */
a[href^='www'] {
  color: #fff;
}
```

### 1.4 属性

#### [强制] 属性定义必须另起一行。

示例：

```css
/* good */
.selector {
  margin: 0;
  padding: 0;
}

/* bad */
.selector { margin: 0; padding: 0; }
```

#### [强制] 属性定义后必须以分号结尾。

示例：

```css
/* good */
.selector {
  margin: 0;
}

/* bad */
.selector {
  margin: 0
}
```

### 1.5 空行

#### [强制] 文件末尾须保留一个且仅有一个空行

#### [建议] 不同组件的CSS代码间可以用一个空行分隔增加可读性

示例：

```css
.header {
  margin: 0 auto;
  .logo {
    width: 100px;
    h1 {
      font-size: 18px;
    }
  }
  
  .nav {
    margin: 0 auto;
    li {
      display: inline-block;
    }
  }
}
```

### 1.6 注释

#### [强制] 单行注释，只使用 `/* comment */` 的形式，`*` 与注释内容之间必须用一个空格隔开，注释之前添加一个空行与其他代码分开，跟在不同属性值后面的注释在垂直方向需要对齐，注释与CSS代码的间距为最长的属性后面加两个空格

#### 注：单行注释用于分隔不同页面组件的CSS代码块，或者用于注释单条存在bug的CSS属性声明

示例：

```css
/* header style */
.header {
  margin: 0 auto;          
  width: 960px;            /* width is too narrow */
  background-color: #fff;  /* this color should not be white */
  font-size: 18px;         /* font-size should use relative units */
}

/* main part style */
.main {
  float: left;
  width: 560px;
}
```

#### [强制] 多行注释风格如下，`*` 与注释之间必须用一个空格隔开，各行开头的 `*` 在垂直方向对齐，注释之前需要加一个空行与其他代码分开

#### 注：多行注释用处同单行注释，当注释内容太长时使用多行形式，对单条属性的注释应当简短，只使用单行形式

示例：

```css
* {
  margin: 0;
  padding: 0;
}

/*
 * this part will be broken when screen is resize 
 * under 560px, need responsive design and write a 
 * new rule for it
 */
.header{
  margin: 0 auto;
  width: 960px;
}
```

## 2 通用

### 2.1 选择器

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

### 2.2 属性缩写

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

### 2.3 属性书写顺序

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

### 2.4 清除浮动

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

### 2.5 !important声明

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

### 2.6 @import

#### [强制] 不要使用@import

解释：@import 规则只能置于CSS文件的顶部，而且增加了额外的HTTP请求，替代办法有以下几种：

* 使用多个 <link> 元素
* 使用Sass中的import把多个CSS文件合并
* 使用gulp，grunt，rails等合并CSS

### 2.7 z-index

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

### 2.8 class命名

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

## 3 值与单位

### 3.1 文本

#### [强制] 文本内容必须用双引号包围。

解释：

文本类型的内容可能在选择器、属性值等内容中。

示例：

```css
/* good */
html[lang|="zh"] q:before {
  font-family: "Microsoft YaHei", sans-serif;
  content: "“";
}

html[lang|="zh"] q:after {
  font-family: "Microsoft YaHei", sans-serif;
  content: "”";
}

/* bad */
html[lang|=zh] q:before {
  font-family: 'Microsoft YaHei', sans-serif;
  content: '“';
}

html[lang|=zh] q:after {
  font-family: "Microsoft YaHei", sans-serif;
  content: "”";
}
```

### 3.2 数值

#### [强制] 当数值为 0 - 1 之间的小数时，不得省略整数部分的 `0`。

示例：

```css
/* good */
panel {
  opacity: 0.8;
}

/* bad */
panel {
  opacity: .8;
}
```

### 3.3 url()

#### [强制] `url()` 函数中的路径需要加双引号，不允许使用单引号或不使用引号。

示例：

```css
body {
  background: url("bg.png");
}
```

#### [建议] `url()` 函数中的绝对路径不要省略协议名。

示例：

```css
body {
  background: url(http://baidu.com/img/bg.png) no-repeat 0 0;
}
```

### 3.4 长度

#### [强制] 长度为 `0` 时须省略单位。 (也只有长度单位可省)

示例：

```css
/* good */
body {
  padding: 0 5px;
}

/* bad */
body {
  padding: 0px 5px;
}
```

### 3.5 颜色

#### [强制] RGB颜色值必须使用十六进制记号形式 `#rrggbb`。不允许使用 `rgb()`。 

解释：

带有alpha的颜色信息可以使用 `rgba()`。使用 `rgba()` 时每个逗号后必须保留一个空格。

示例：

```css
/* good */
.success {
  box-shadow: 0 0 2px rgba(0, 128, 0, .3);
  border-color: #008000;
}

/* bad */
.success {
  box-shadow: 0 0 2px rgba(0,128,0,.3);
  border-color: rgb(0, 128, 0);
}
```

#### [强制] 颜色值可以缩写时，必须使用缩写形式。

解释：缩写可以节省少数字节

示例：

```css
/* good */
.success {
  background-color: #aca;
}

/* bad */
.success {
  background-color: #aaccaa;
}
```

#### [强制] 颜色值不允许使用命名色值。

解释：W3C标准中命名色值只有8个（RGB三个通道为255或0），不同浏览器各自实现了一套命名色值，所以不要使用

示例：

```css
/* good */
.success {
  color: #90ee90;
}

/* bad */
.success {
  color: lightgreen;
}
```

#### [强制] 颜色值中的英文字符采用小写

示例：

```css
/* good */
.success {
  background-color: #aca;
  color: #90ee90;
}

/* bad */
.success {
  background-color: #ACA;
  color: #90EE90;
}

/* bad */
.success {
  background-color: #ACA;
  color: #90ee90;
}
```

### 3.6 2D 位置

#### [强制] 必须同时给出水平和垂直方向的位置。

解释：

2D 位置初始值为 `0% 0%`，但在只有一个方向的值时，另一个方向的值会被解析为 center。为避免理解上的困扰，应同时给出两个方向的值。

示例：

```css
/* good */
body {
  background-position: center top; /* 50% 0% */
}

/* bad */
body {
  background-position: top; /* 50% 0% */
}
```

## 4 文本编排

### 4.1 字体族

#### [强制] `font-family` 属性中的字体族名称应使用字体的英文 `Family Name`，其中如有空格，须放置在引号中。

解释：

所谓英文 Family Name，为字体文件的一个元数据，常见名称如下：

字体 | 操作系统 | Family Name
-----|----------|------------
宋体 (中易宋体) | Windows | SimSun
黑体 (中易黑体) | Windows | SimHei
微软雅黑 | Windows | Microsoft YaHei
微软正黑 | Windows | Microsoft JhengHei
华文黑体 | Mac/iOS | STHeiti
冬青黑体 | Mac/iOS | Hiragino Sans GB
文泉驿正黑 | Linux | WenQuanYi Zen Hei
文泉驿微米黑 | Linux | WenQuanYi Micro Hei

示例：

```css
h1 {
  font-family: "Microsoft YaHei";
}
```

#### [强制] `font-family` 按「西文字体在前、中文字体在后」、「效果佳 (质量高/更能满足需求) 的字体在前、效果一般的字体在后」的顺序编写，最后必须指定一个通用字体族( `serif` / `sans-serif` )。

解释：

更详细说明可参考[本文](http://www.zhihu.com/question/19911793/answer/13329819)。

示例：

```css
/* Display according to platform */
.article {
  font-family: Arial, sans-serif;
}

/* Specific for most platforms */
h1 {
  font-family: "Helvetica Neue", Arial, "Hiragino Sans GB", "WenQuanYi Micro Hei", "Microsoft YaHei", sans-serif;
}
```

#### [强制] `font-family` 不区分大小写，但在同一个项目中，同样的 `Family Name` 大小写必须统一。

示例：

```css
/* good */
body {
  font-family: Arial, sans-serif;
}

h1 {
  font-family: Arial, "Microsoft YaHei", sans-serif;
}

/* bad */
body {
  font-family: arial, sans-serif;
}

h1 {
  font-family: Arial, "Microsoft YaHei", sans-serif;
}
```

### 4.2 字号

#### [强制] 需要在 Windows 平台显示的中文内容，其字号应不小于 `12px`。

解释：

由于 Windows 的字体渲染机制，小于 12px 的文字显示效果极差、难以辨认。

### 4.3 字体风格

#### [建议] 需要在 Windows 平台显示的中文内容，不要使用除 `normal` 外的 `font-style`。其他平台也应慎用。

解释：

由于中文字体没有 italic 风格的实现，所有浏览器下都会 fallback 到 obilique 实现 (自动拟合为斜体)，小字号下 (特别是 Windows 下会在小字号下使用点阵字体的情况下) 显示效果差，造成阅读困难。点击[这里](http://www.zhihu.com/question/26075109)进一步了解。

### 4.4 行高

#### [建议] `line-height` 在定义文本段落行间距时，应使用数值。

#### [强制] `line-height` 用于控制垂直居中时，应该设置成与容器高度一致。

解释：

将 line-height 设置为数值，浏览器会基于当前元素设置的 font-size 进行再次计算。在不同字号的文本段落组合中，能达到较为舒适的行间间隔效果，避免在每个设置了 font-size 都需要设置 line-height。

示例：

```css
.container {
  line-height: 1.5;
}
```
## 5 变换与动画

#### [强制] 使用 `transition` 时应指定 `transition-property`。

示例：

```css
/* good */
.box {
  transition: color 1s, border-color 1s;
}

/* bad */
.box {
  transition: all 1s;
}
```

#### [建议] 尽可能在浏览器能高效实现的属性上添加过渡和动画。

解释：

见[本文](http://www.html5rocks.com/en/tutorials/speed/high-performance-animations/)，在可能的情况下应选择这样四种变换：

* `transform: translate(npx, npx);`
* `transform: scale(n);`
* `transform: rotate(ndeg);`
* `opacity: 0..1;`

典型的，可以使用 translate 来代替 left 作为动画属性。

示例：

```css
/* good */
.box {
  transition: transform 1s;
}
.box:hover {
  transform: translate(20px); /* move right for 20px */
}

/* bad */
.box {
  left: 0;
  transition: left 1s;
}
.box:hover {
  left: 20px; /* move right for 20px */
}
```

## 6 响应式

#### [强制] `Media Query` 不得单独编排，必须与相关的规则一起定义。

示例：

```css
/* Good */
/* header styles */
@media (...) {
  /* header styles */
}

/* main styles */
@media (...) {
  /* main styles */
}

/* footer styles */
@media (...) {
  /* footer styles */
}


/* Bad */
/* header styles */
/* main styles */
/* footer styles */

@media (...) {
  /* header styles */
  /* main styles */
  /* footer styles */
}
```

#### [强制] `Media Query` 如果有多个逗号分隔的条件时，应将每个条件放在单独一行中。

示例：

```css
@media
(-webkit-min-device-pixel-ratio: 2), /* Webkit-based browsers */
(min--moz-device-pixel-ratio: 2),    /* Older Firefox browsers (prior to Firefox 16) */
(min-resolution: 2dppx),             /* The standard way */
(min-resolution: 192dpi) {           /* dppx fallback */
  /* Retina-specific stuff here */
}
```

#### [建议] 尽可能给出在高分辨率设备 (Retina) 下效果更佳的样式。

## 7 兼容性

### 7.1 厂商前缀

#### [强制] 带私有前缀的属性由长到短排列，按冒号位置对齐。

解释：

标准属性放在最后，按冒号对齐方便阅读，也便于在编辑器内进行多行编辑。

示例：

```css
.box {
  -webkit-box-sizing: border-box;
     -moz-box-sizing: border-box;
          box-sizing: border-box;
}
```

## 参考文档

本文档在编写的时候主要参考了以下的编码风格指南：

* [百度EFE团队CSS编码规范](https://github.com/ecomfe/spec/blob/master/css-style-guide.md)
* [编码规范 by @mdo](http://codeguide.bootcss.com/)
