---
title: 撰写新文章
date: 2019-08-08 14:10:00 +0800
categories:
  - 博客
  - 教程
tags:
  - 写作
---

本教程将指导您如何在 _Chirpy_ 模板中撰写文章，即使您以前使用过 Hugo，也值得阅读，因为许多功能需要设置特定变量。

## 命名和路径

创建一个新文件，使用 `hugo new content/post/YYYY-MM-DD-TITLE.md`。您可以根据自己的喜好更改路径，但请注意，所有文章都应该放在根目录的 {{< filepath src="content/post" >}} 中。

## 前言

基本上，您需要在文章顶部填写[前言](https://gohugo.io/content-management/front-matter/)，如下所示：

```yaml
---
title: 标题
date: YYYY-MM-DD HH:MM:SS +/-TTTT
draft: true
---
```

您可以根据需要添加以下字段：
```yaml
categories: [主分类, 子分类] # 只支持两个分类
tags: [标签]     # 标签名称应始终为小写
pin: true      # 表示这篇文章将显示在首页顶部
description: 文章描述 # 该文章的描述
```

> 文章的_布局_默认已设置为`post`，因此无需在前言块中添加变量_layout_。
{ .prompt-tip }

### 分类和标签

每篇文章的 `categories` 设计为最多包含两个元素，而 `tags` 中的元素数量可以从零到无穷大。例如：

```yaml
---
categories: [动物, 昆虫]
tags: [蜜蜂]
---
```

### 作者信息

文章的作者信息通常不需要在 _前言_ 中填写，默认情况下，它们将从配置文件的 `social.name` 和 `social.links` 的第一个条目中获取。但您也可以按如下方式覆盖它：

在 `data/authors.yaml` 中添加作者信息（如果您的网站没有此文件，请创建一个）。

```yaml { file="data/authors.yml" }
<作者ID>:
  name: <全名>
  url: <作者的主页>
```

然后使用 `author` 指定单个条目或 `authors` 指定多个条目：

```yaml
---
author: <作者ID>                     # 单个条目
# 或
authors: [<作者1ID>, <作者2ID>]   # 多个条目
---
```

如果您不想在每篇文章的前言中指定作者，可以在 {{< filepath src="config/_default/params.toml" >}} 中设置全局作者。

```yaml { file="config/_default/params.toml" }
author: <作者ID>
```

> 在每篇文章前言中指定的作者将覆盖全局作者设置。因此，如果任何文章的作者与全局作者不同，可以直接在其前言中添加作者。
{ .prompt-info }

要在启用 i18n 的站点上支持多语言作者信息，您可以在 {{< filepath src="data/authors/" >}} 下组织特定语言的 YAML 文件中的作者数据。例如：

- 英语：{{< filepath src="data/authors/en.yaml" >}}
- 简体中文：{{< filepath src="data/authors/zh-CN.yaml" >}}

只需用相应的作者详细信息填充每个文件：

```yaml { file="data/authors/en.yaml" }
<作者ID>:
  name: <作者英文名>
  url: <作者的主页>
```

```yaml { file="data/authors/zh-CN.yaml" }
<作者ID>:
  name: <作者中文名>
  url: <作者的主页>
```

### 文章描述

默认情况下，文章的第一句话用于在首页的文章列表、_进一步阅读_ 部分以及RSS源的XML中显示。如果您不想为文章显示自动生成的描述，可以使用 _前言_ 中的 `description` 字段自定义它，如下所示：

```yaml
---
description: 文章的简短摘要。
---
```

此外，`description` 文本也将显示在文章页面的文章标题下方。

## 目录

默认情况下，目录（TOC）显示在文章的右侧面板上。如果您想全局关闭它，请转到 {{< filepath src="config/_default/params.toml" >}} 并将变量 `toc` 的值设置为 `false`。如果您想为特定文章关闭TOC，请将以下内容添加到文章的[前言](https://gohugo.io/content-management/front-matter/)中：

```yaml
---
toc: false
---
```

## 评论

评论的全局设置由 {{< filepath src="config/_default/params.toml" >}} 文件中的 `comments.provider` 选项定义。一旦为此变量选择了评论系统，所有文章将启用评论。

如果您想关闭特定文章的评论，请将以下内容添加到文章的**前言**中：

```yaml
---
comments: false
---
```

## 媒体

在 _Chirpy_ 中，我们将图片、音频和视频称为媒体资源。

### URL前缀

> URL 前缀功能正在开发中。
{ .prompt-warning }

有时，我们必须为一篇文章中的多个资源定义重复的URL前缀，这是一项可以通过设置两个参数来避免的繁琐任务。

- 如果您使用CDN托管媒体文件，可以在 {{< filepath src="config/_default/params.toml" >}} 中指定 `cdn`。然后，站点头像和文章的媒体资源的URL将以CDN域名为前缀。

  ```yaml { file="config/_default/params.toml" }
  cdn: https://cdn.com
  ```

- 要为当前文章/页面范围指定资源路径前缀，请在文章的 _前言_ 中设置 `media_subpath`：

  ```yaml
  ---
  media_subpath: /path/to/media/
  ---
  ```

选项 `site.cdn` 和 `page.media_subpath` 可以单独使用或组合使用，以灵活组合最终的资源URL：`[site.cdn/][page.media_subpath/]file.ext`

### 图片

#### 标题

在图片的下一行添加 HTML 属性 `caption`，然后它将作为标题显示在图片底部：

```markdown
![图片描述](/path/to/image)
{ caption="图片的标题" }
```

#### 尺寸

为防止图片加载时页面内容布局发生偏移，我们应该为每张图片设置宽度和高度。

```markdown
![桌面视图](/assets/img/sample/mockup.png)
{ width="700" height="400" }
```

> 对于SVG，您至少必须指定其 _宽度_，否则它不会被渲染。
{ .prompt-info }

#### 位置

默认情况下，图片居中，但您可以使用 `normal`、`left` 和 `right` 类之一指定位置。

> 一旦指定了位置，就不应添加图片标题。
{ .prompt-warning }

- **普通位置**

  在下面的示例中，图片将左对齐：

  ```markdown
  ![桌面视图](/assets/img/sample/mockup.png)
  { .normal }
  ```

- **向左浮动**

  ```markdown
  ![桌面视图](/assets/img/sample/mockup.png)
  { .left }
  ```

- **向右浮动**

  ```markdown
  ![桌面视图](/assets/img/sample/mockup.png)
  { .right }
  ```

#### 暗/亮模式

您可以使图片跟随暗/亮模式的主题偏好。这需要您准备两张图片，一张用于暗模式，一张用于亮模式，然后为它们分配特定的类（`dark` 或 `light`）：

```markdown
![仅亮模式](/path/to/light-mode.png)
{ .light }
![仅暗模式](/path/to/dark-mode.png)
{ .dark }
```

#### 阴影

程序窗口的截图可以考虑显示阴影效果：

```markdown
![桌面视图](/assets/img/sample/mockup.png)
{ .shadow }
```

#### 预览图片

如果您想在文章顶部添加图片，请提供分辨率为 `1200 x 630` 的图片。请注意，如果图片的宽高比不符合 `1.91 : 1`，图片将被缩放和裁剪。

了解这些先决条件后，您可以开始设置图片的属性：

```yaml
---
image:
  path: /path/to/image
  alt: 图片替代文本
---
```

请注意，[`media_subpath`](#url前缀) 也可以传递给预览图片，也就是说，当它已经设置好时，属性 `path` 只需要图片文件名。

### 视频

#### 社交媒体平台

您可以使用以下语法嵌入来自社交媒体平台的视频：

```hugo
{{</* embed/{Platform}.html id="{ID}" */>}}
```

其中 `Platform` 是平台名称的小写形式，`ID` 是视频 ID。

下表显示了如何从给定的视频 URL 中获取我们需要的两个参数，您还可以了解当前支持的视频平台。

| 视频 URL                                                                                          | 平台       | ID             |
| -------------------------------------------------------------------------------------------------- | ---------- | :------------- |
| [https://www.**youtube**.com/watch?v=**H-B46URT4mg**](https://www.youtube.com/watch?v=H-B46URT4mg) | `youtube`  | `H-B46URT4mg`  |
| [https://www.**twitch**.tv/videos/**1634779211**](https://www.twitch.tv/videos/1634779211)         | `twitch`   | `1634779211`   |
| [https://www.**bilibili**.com/video/**BV1Q44y1B7Wf**](https://www.bilibili.com/video/BV1Q44y1B7Wf) | `bilibili` | `BV1Q44y1B7Wf` |

#### 视频文件

如果您想直接嵌入视频文件，请使用以下语法：

```hugo
{{</* embed/video.html src="{URL}" */>}}
```

其中 `URL` 是指向视频文件的 URL，例如 `/path/to/sample/video.mp4`。

您还可以为嵌入的视频文件指定其他属性。以下是允许的属性的完整列表。

- `poster='/path/to/poster.png'` — 视频的海报图片，在视频下载时显示
- `title='文本'` — 显示在视频下方的标题，外观与图片标题相同
- `autoplay=true` — 视频在准备好后自动开始播放
- `loop=true` — 在视频播放结束时自动回到起点
- `muted=true` — 音频最初将被静音
- `types` — 指定其他视频格式的扩展名，用 `|` 分隔。确保这些文件与您的主视频文件存在于同一目录中。

考虑使用上述所有内容的示例：

```liquid
{{</*
  embed/video.html
  src="/path/to/video.mp4"
  types="ogg|mov"
  poster="poster.png"
  title="演示视频"
  autoplay=true
  loop=true
  muted=true
*/>}}
```

### 音频

如果您想直接嵌入音频文件，请使用以下语法：

```liquid
{{</*  embed/audio.html src="{URL}" */>}}
```

其中 `URL` 是指向音频文件的 URL，例如 `/path/to/audio.mp3`。

您还可以为嵌入的音频文件指定其他属性。以下是允许的属性的完整列表。

- `title='文本'` — 显示在音频下方的标题，外观与图片标题相同
- `types` — 指定其他音频格式的扩展名，用 `|` 分隔。确保这些文件与您的主音频文件存在于同一目录中。

考虑使用上述所有内容的示例：

```hugo
{{</*
  include embed/audio.html
  src='/path/to/audio.mp3'
  types='ogg|wav|aac'
  title='演示音频'
*/>}}
```

## 置顶文章

您可以将一篇或多篇文章置顶到首页顶部，置顶的文章按照它们的发布日期以倒序排序。通过以下方式启用：

```yaml
---
pin: true
---
```

## 提示框

有几种类型的提示框：`tip`、`info`、`warning` 和 `danger`。它们可以通过向引用块添加类 `prompt-{type}` 来生成。例如，按如下方式定义 `info` 类型的提示框：

```md
> 提示框示例文本。
{ .prompt-info }
```

## 语法

### 内联代码

```md
`内联代码部分`
```

### 文件路径高亮

```hugo
{{</* filepath src="/path/to/a/file.extend" */>}}
```

### 代码块

Markdown 符号 ```` ``` ```` 可以轻松创建代码块，如下所示：

````md
```
这是一个纯文本代码片段。
```
````

#### 指定语言

使用 ```` ```{language} ```` 您将获得带有语法高亮的代码块：

````markdown
```yaml
key: value
```
````

#### 指定文件名

您可能已经注意到代码语言将显示在代码块的顶部。如果您想用文件名替换它，可以添加 `file` 属性来实现：

````markdown
```shell { file="path/to/file" }
# content
```
````

## 数学公式

我们使用 [**MathJax**][mathjax] 来生成数学公式。出于网站性能的原因，默认情况下不会加载数学功能。但可以通过以下方式启用：

[mathjax]: https://www.mathjax.org/

```yaml
---
math: true
---
```

启用数学功能后，您可以使用以下语法添加数学公式：

- **块级数学公式** 应该使用 `$$ math $$` 添加，**必须** 在 `$$` 之前和之后留有空行
  - **插入方程编号** 应该使用 `$$\begin{equation} math \end{equation}$$` 添加
  - **引用方程编号** 应该在方程块中使用 `\label{eq:label_name}` 和在文本中使用 `\eqref{eq:label_name}` 内联引用（见下面的示例）
- **内联数学公式**（在行中）应该使用 `$$ math $$` 添加，在 `$$` 之前或之后不要有任何空行
- **内联数学公式**（在列表中）应该使用 `\$$ math $$` 添加

```markdown
<!-- 块级数学公式，保留所有空行 -->

$$
LaTeX_数学表达式
$$

<!-- 方程编号，保留所有空行 -->

$$
\begin{equation}
  LaTeX_数学表达式
  \label{eq:label_name}
\end{equation}
$$

可以引用为 \eqref{eq:label_name}。

<!-- 行内数学公式，不要有空行 -->

"Lorem ipsum dolor sit amet, $$ LaTeX_数学表达式 $$ consectetur adipiscing elit."

<!-- 列表中的内联数学公式，第一个 `$` 需要转义 -->

1. \$$ LaTeX_数学表达式 $$
2. \$$ LaTeX_数学表达式 $$
3. \$$ LaTeX_数学表达式 $$
```

[mathjax-exts]: https://docs.mathjax.org/en/latest/input/tex/extensions/index.html

## Mermaid

> Mermaid 支持正在开发中
{ .prompt-warning }

[**Mermaid**](https://github.com/mermaid-js/mermaid) 是一个很棒的图表生成工具。要在您的文章中启用它，请将以下内容添加到 YAML 块中：

```yaml
---
mermaid: true
---
```

然后您可以像其他 markdown 语言一样使用它：将图表代码用 ```` ```mermaid ```` 和 ```` ``` ```` 包围起来。

## 了解更多

要了解更多关于撰写 Hugo 文章的知识，请访问 [Hugo 文档](https://gohugo.io/documentation/)。 