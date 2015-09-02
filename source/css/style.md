# 代码风格

### 缩进

#### [强制] 使用 `2` 个空格做为一个缩进层级，不允许使用 `4` 个空格 或 `tab` 字符。

示例：

```css
.selector {
  margin: 0;
  padding: 0;
}
```

### 空格

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

### 选择器

#### [建议] 当一个CSS规则块包含多个选择器时，每个选择器声明必须独占一行。

示例：

```css
/* good */
.post,
.page,
.comment {
  line-height:;
}

/* bad */
.post, .page, .comment {
  line-height:;
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

### 属性

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

### 空行

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

### 注释

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
