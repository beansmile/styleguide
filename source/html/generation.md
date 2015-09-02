# 通用

### 1.2.1 DOCTYPE

#### [强制] 使用 `HTML5` 的 `DOCTYPE` 来启用标准模式，建议使用大写的 `DOCTYPE`。

示例：

```html
<!DOCTYPE html>
```

#### [建议] 启用 IE Edge 模式。

示例：

```html
<meta http-equiv="X-UA-Compatible" content="IE=Edge">
```

#### [建议] 在 `html` 标签上设置正确的 lang 属性。

解释：

有助于提高页面的可访问性，如：让语音合成工具确定其所应该采用的发音，令翻译工具确定其翻译语言等。

示例：

```html
<html lang="zh-CN"></html>
```

### 1.2.2 编码

#### [强制] 页面必须明确指定字符编码。指定字符编码的 `meta` 必须是 `head` 的第一个直接子元素。

解释：

见 [HTML5 Charset能用吗](http://www.qianduan.net/html5-charset-can-it.html) 一文。

示例：

```html
<html>
  <head>
    <meta charset="UTF-8">
    ......
  </head>
  <body>
    ......
  </body>
</html>
```

### 1.2.3 CSS 和 JavaScript 引入

#### [强制] 引入 `CSS` 时必须指明 `rel="stylesheet"`。

示例：

```html
<link rel="stylesheet" src="page.css">
```

#### [强制] 表现定义放置于外部 `CSS` 中，行为定义放置于外部 `JavaScript` 中。

解释：

结构-样式-行为的代码分离，对于提高代码的可阅读性和维护性都有好处。

#### [建议] 引入 `CSS` 和 `JavaScript` 时无须指明 `type` 属性。

解释：

`text/css` 和 `text/javascript` 是 type 的默认值。

#### [建议] 在 `head` 中引入页面需要的所有 `CSS` 资源。

解释：

在页面渲染的过程中，新的CSS可能导致元素的样式重新计算和绘制，页面闪烁。

#### [建议] `JavaScript` 应当放在页面末尾，或采用异步加载。

解释：

将 script 放在页面中间将阻断页面的渲染。出于性能方面的考虑，如非必要，请遵守此条建议。

示例：

```html
<body>
  <!-- a lot of elements -->
  <script src="init-behavior.js"></script>
</body>
``
