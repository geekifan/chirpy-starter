---
title: 自定义网站图标
date: 2019-08-11 00:34:00 +0800
description: >-
  通过这个全面概述开始学习 Chirpy 的基础知识。
  您将学习如何安装、配置和使用您的第一个基于 Chirpy 的网站，以及如何将其部署到网络服务器。
categories:
  - 博客
  - 教程
tags:
  - 网站图标
---

[**Chirpy**](https://github.com/cotes2020/jekyll-theme-chirpy/) 的[网站图标](https://www.favicon-generator.org/about/)放置在 {{< filepath src="assets/img/favicons/" >}} 目录中。您可能想用自己的图标替换它们。以下部分将指导您创建和替换默认网站图标。

## 生成网站图标

准备一张尺寸为 512x512 或更大的正方形图像（PNG、JPG 或 SVG），然后前往在线工具 [**Real Favicon Generator**](https://realfavicongenerator.net/)，点击 <kbd>Select your Favicon image</kbd> 按钮上传您的图像文件。

在下一步中，网页将显示所有使用场景。您可以保留默认选项，滚动到页面底部，点击 <kbd>Generate your Favicons and HTML code</kbd> 按钮生成网站图标。

## 下载与替换

下载生成的包，解压并从提取的文件中删除以下两个文件：

- {{< filepath src="browserconfig.xml" >}}
- {{< filepath src="site.webmanifest" >}}

然后将剩余的图像文件（{{< filepath src=".PNG" >}} 和 {{< filepath src=".ICO" >}}）复制到您的 Hugo 站点的 {{< filepath src="assets/img/favicons/" >}} 目录中，覆盖原始文件。如果您的 Hugo 站点还没有这个目录，只需创建一个。

下表将帮助您理解网站图标文件的变化：

| 文件             | 来自在线工具                  | 来自 Chirpy |
|---------------------|:---------------------------------:|:-----------:|
| `*.PNG`             | ✓                                 | ✗           |
| `*.ICO`             | ✓                                 | ✗           |

<!-- markdownlint-disable-next-line -->
>  ✓ 表示保留，✗ 表示删除。
{ .prompt-info }

下次构建站点时，网站图标将被自定义版本替换。 