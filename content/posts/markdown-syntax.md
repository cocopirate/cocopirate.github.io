---
author: "Lee"
title: "Hugo编写文档语法教程"
date: "2021-03-26"
description: "这是一篇适用Hugo编写文档语法的教程"
tags: ["Markdown","语法","Hugo"]
categories: ["语法"]
series: ["教程"]
aliases: ["migrate-from-jekyl"]
---

本文提供了在Hugo的内容文件中使用的基本的Markdown语法示例，方便编写文档时查询使用。

## 标题

下面是HTML中`<h1>`—`<h6>`元素代表六级节标题。`<h1>`是最大的标题，而`<h6>`是最小的标题。

```
# H1标题
## H2标题
### H3标题
#### H4标题
##### H5标题
###### H6标题
```

## 段落

我翻开历史一查，这历史没有年代。歪歪斜斜的每页上都写着“仁义道德”几个字，我横竖睡不着，仔细看了半夜，才从字缝里看出来，满本上都写着两个字“吃人"！

——鲁迅 《狂人日记》

## 引用

引用标签表示从其他来源引用的内容，可选地带有必须在`footer`或`cite`元素内的引用，并且可选地带有内联更改，例如注释和缩写。

#### 不注明出处的引用

> Tiam, ad mint andaepu dandae nostion secatur sequo quae.
> **Note** that you can use *Markdown syntax* within a blockquote.

#### 注明出处的引用

> Don't communicate by sharing memory, share memory by communicating.</p>
> — <cite>Rob Pike[^1]</cite>


[^1]: The above quote is excerpted from Rob Pike's [talk](https://www.youtube.com/watch?v=PAAkCSZUG1c) during Gopherfest, November 18, 2015.

## 表格

Tables aren't part of the core Markdown spec, but Hugo supports supports them out-of-the-box.

   Name | Age
--------|------
    Bob | 27
  Alice | 23

#### 表格内部使用Markdown

| Inline&nbsp;&nbsp;&nbsp;     | Markdown&nbsp;&nbsp;&nbsp;  | In&nbsp;&nbsp;&nbsp;                | Table      |
| ---------- | --------- | ----------------- | ---------- |
| *斜体*  | **加粗**  | ~~划线~~&nbsp;&nbsp;&nbsp; | `code`     |

## 代码块

#### 带反引号的代码块

``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Example HTML5 Document</title>
</head>
<body>
  <p>Test</p>
</body>
</html>
```

#### 缩进四个空格的代码块

``` html
    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <title>Example HTML5 Document</title>
    </head>
    <body>
      <p>Test</p>
    </body>
    </html>
```
#### 带有 Hugo 内部高亮简码的代码块

{{< highlight html >}}
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Example HTML5 Document</title>
</head>
<body>
  <p>Test</p>
</body>
</html>
{{< /highlight >}}

## 列表

#### 有序列表

1. 第一个列表项
2. 第二个列表项
3. 第三个列表项

#### 无序列表

* 列表项
* 另一个列表项
* 还有一个列表项

#### 嵌套列表

* 项目
  1. 第一个子列表项
  2. 第二个子列表项

## 其他元素 — abbr, sub, sup, kbd, mark

<abbr title="Graphics Interchange Format">GIF</abbr> is a bitmap image format.

H<sub>2</sub>O

X<sup>n</sup> + Y<sup>n</sup> = Z<sup>n</sup>

Press <kbd><kbd>CTRL</kbd>+<kbd>ALT</kbd>+<kbd>Delete</kbd></kbd> to end the session.

Most <mark>salamanders</mark> are nocturnal, and hunt for insects, worms, and other small creatures.

## 图片

图片放置在项目的`/static/images`目录下，使用Markdown语法`![](/images/ydmt256.png)`配置图片的相对路径展示。图片将按原始尺寸显示，如下所示：

![](/images/ydmt256.png)

图片若需要调整大小，可使用Hugo的`figure`短代码引用图片，如下所示：

```
# 需要将下面代码中的{和<之间的空格删除，否则无法正常显示
{{ <figure src="/images/ydmt256.png" title="图片示例" width="10%" >}}
```
缩小后的图片大小如下所示：

{{<figure src="/images/ydmt256.png" title="图片示例" width="10%" >}}
