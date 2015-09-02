# Beansmile代码规范指南

## 说明：

本文档用于Beansmile内部代码规范，包括HTML，CSS，JavasSript，Ruby，Rails编码规范。

本文档的目的是：

- 提高代码可读性
- 保持代码一致性

发布于 https://www.gitbook.com/book/beansmile/styleguide/details

源码：https://github.com/beansmile/styleguide

------

## 规范的收录原则：

1. 应该有实际指导意义规范
2. 尽量少
3. 编制人员必须清楚每一条规则

------

## 目录结构和文件命名

### 根目录

``` 
source/    # 源文件目录
README.MD  # 总说明
SUMMARY.MD # 总文档索引
```

### source中的目录结果

``` 
- source/html/
- source/css/
   |_ README.md # 章节导言
   |_ codestyle.md
   |_ common.md
   |_ more...
```

### SUMMRAY.md 的内容

``` 
* [CSS styleguide](source/css/README.md)
  * [代码风格](source/css/codestyle.md)
  * [通用](source/css/common.md)
```

### 各章节的导言

#### 文件：

各章节目录中的README.md文件，如 `source/css/README.md`

#### 内容：

- 说明：本章节说明
- 参考文档：用到的参考文档列表
- 编制人员：编制人员的名字，审订人名字
- 修订历史：定稿时间、修订时间

