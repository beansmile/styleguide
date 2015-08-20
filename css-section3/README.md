# 值与单位

### 文本

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

### 数值

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

### url()

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

### 长度

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

### 颜色

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

### 2D 位置

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
