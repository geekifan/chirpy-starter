---
title: Writing a New Post
date: 2019-08-08 14:10:00 +0800
categories:
  - Blogging
  - Tutorial
tags:
  - writing
---


This tutorial will guide you how to write a post in the _Chirpy_ template, and it's worth reading even if you've used Hugo before, as many features require specific variables to be set.

## Naming and Path

Create a new file using `hugo new content/post/YYYY-MM-DD-TITLE.md`. You can change the path as you like, but note that all the posts should be placed in {{< filepath src="content/post" >}} of the root directory.

## Front Matter

Basically, you need to fill the [Front Matter](https://gohugo.io/content-management/front-matter/) as below at the top of the post:

```yaml
---
title: TITLE
date: YYYY-MM-DD HH:MM:SS +/-TTTT
draft: true
---
```

You can add the following fields as needed:
```yaml
categories: [TOP_CATEGORY, SUB_CATEGORY] # only two categories are supported
tags: [TAG]     # TAG names should always be lowercase
pin: true      # it means this post will show at the top of the home page.
description: Hello, World! # description of this post
```

> The posts' _layout_ has been set to `post` by default, so there is no need to add the variable _layout_ in the Front Matter block.
{ .prompt-tip }

### Categories and Tags

The `categories` of each post are designed to contain up to two elements, and the number of elements in `tags` can be zero to infinity. For instance:

```yaml
---
categories: [Animal, Insect]
tags: [bee]
---
```

### Author Information

The author information of the post usually does not need to be filled in the _Front Matter_ , they will be obtained from variables `social.name` and the first entry of `social.links` of the configuration file by default. But you can also override it as follows:

Adding author information in `data/authors.yaml` (If your website doesn't have this file, don't hesitate to create one).

```yaml { file="data/authors.yml" }
<author_id>:
  name: <full name>
  url: <homepage_of_author>
```

And then use `author` to specify a single entry or `authors` to specify multiple entries:

```yaml
---
author: <author_id>                     # for single entry
# or
authors: [<author1_id>, <author2_id>]   # for multiple entries
---
```

If you don't want to specify the author in the frontmatter of every article, you can set a global author in {{< filepath src="config/_default/params.toml" >}}.

```yaml { file="config/_default/params.toml" }
author: <author_id>
```

> The author specified in each article's frontmatter will override the global author setting. So if any article has a different author than the global one, feel free to add the author directly in its frontmatter.
{ .prompt-info }

To support multilingual author information on an i18n-enabled site, you can organize author data in language-specific YAML files under {{< filepath src="data/authors/" >}}. For instance:

- English: {{< filepath src="data/authors/en.yaml" >}}
- Simplified Chinese: {{< filepath src="data/authors/zh-CN.yaml" >}}

Simply populate each file with the respective author details:

```yaml { file=" data/authors/en.yaml" }
<author_id>:
  name: <author_name_en>
  url: <homepage_of_author>
```

```yaml { file=" data/authors/zh-CN.yaml" }
<author_id>:
  name: <author_name_zh_CN>
  url: <homepage_of_author>
```

### Post Description

By default, the first words of the post are used to display on the home page for a list of posts, in the _Further Reading_ section, and in the XML of the RSS feed. If you don't want to display the auto-generated description for the post, you can customize it using the `description` field in the _Front Matter_ as follows:

```yaml
---
description: Short summary of the post.
---
```

Additionally, the `description` text will also be displayed under the post title on the post's page.

## Table of Contents

By default, the **T**able **o**f **C**ontents (TOC) is displayed on the right panel of the post. If you want to turn it off globally, go to {{< filepath src="config/_default/params.toml" >}} and set the value of variable `toc` to `false`. If you want to turn off TOC for a specific post, add the following to the post's [Front Matter](https://gohugo.io/content-management/front-matter/):

```yaml
---
toc: false
---
```

## Comments

The global setting for comments is defined by the `comments.provider` option in the {{< filepath src="config/_default/params.toml" >}} file. Once a comment system is selected for this variable, comments will be enabled for all posts.

If you want to close the comment for a specific post, add the following to the **Front Matter** of the post:

```yaml
---
comments: false
---
```

## Media

We refer to images, audio and video as media resources in _Chirpy_.

### URL Prefix

> URL prefix is under development.
{ .prompt-warning }

From time to time we have to define duplicate URL prefixes for multiple resources in a post, which is a boring task that you can avoid by setting two parameters.

- If you are using a CDN to host media files, you can specify the `cdn` in {{< filepath src="config/_default/params.toml" >}}. The URLs of media resources for site avatar and posts are then prefixed with the CDN domain name.

  ```yaml  { file="config/_default/params.toml" }
  cdn: https://cdn.com
  ```


- To specify the resource path prefix for the current post/page range, set `media_subpath` in the _front matter_ of the post:

  ```yaml
  ---
  media_subpath: /path/to/media/
  ---
  ```

The option `site.cdn` and `page.media_subpath` can be used individually or in combination to flexibly compose the final resource URL: `[site.cdn/][page.media_subpath/]file.ext`

### Images

#### Caption

Add an html attribute `caption` to the next line of an image, then it will become the caption and appear at the bottom of the image:

```markdown
![img-description](/path/to/image)
{ caption="Your caption of images" }
```

#### Size

To prevent the page content layout from shifting when the image is loaded, we should set the width and height for each image.

```markdown
![Desktop View](/assets/img/sample/mockup.png)
{ width="700" height="400" }
```

> For an SVG, you have to at least specify its _width_, otherwise it won't be rendered.
{ .prompt-info }


#### Position

By default, the image is centered, but you can specify the position by using one of the classes `normal`, `left`, and `right`.

> Once the position is specified, the image caption should not be added.
{ .prompt-warning }

- **Normal position**

  Image will be left aligned in below sample:

  ```markdown
  ![Desktop View](/assets/img/sample/mockup.png)
  { .normal }
  ```

- **Float to the left**

  ```markdown
  ![Desktop View](/assets/img/sample/mockup.png)
  { .left }
  ```

- **Float to the right**

  ```markdown
  ![Desktop View](/assets/img/sample/mockup.png)
  { .right }
  ```

#### Dark/Light mode

You can make images follow theme preferences in dark/light mode. This requires you to prepare two images, one for dark mode and one for light mode, and then assign them a specific class (`dark` or `light`):

```markdown
![Light mode only](/path/to/light-mode.png)
{ .light }
![Dark mode only](/path/to/dark-mode.png)
{ .dark }
```

#### Shadow

The screenshots of the program window can be considered to show the shadow effect:

```markdown
![Desktop View](/assets/img/sample/mockup.png)
{ .shadow }
```

#### Preview Image

If you want to add an image at the top of the post, please provide an image with a resolution of `1200 x 630`. Please note that if the image aspect ratio does not meet `1.91 : 1`, the image will be scaled and cropped.

Knowing these prerequisites, you can start setting the image's attribute:

```yaml
---
image:
  path: /path/to/image
  alt: image alternative text
---
```

Note that the [`media_subpath`](#url-prefix) can also be passed to the preview image, that is, when it has been set, the attribute `path` only needs the image file name.


### Video

#### Social Media Platform

You can embed videos from social media platforms with the following syntax:

```hugo
{{</* embed/{Platform}.html id="{ID}" */>}}
```

Where `Platform` is the lowercase of the platform name, and `ID` is the video ID.

The following table shows how to get the two parameters we need in a given video URL, and you can also know the currently supported video platforms.

| Video URL                                                                                          | Platform   | ID             |
| -------------------------------------------------------------------------------------------------- | ---------- | :------------- |
| [https://www.**youtube**.com/watch?v=**H-B46URT4mg**](https://www.youtube.com/watch?v=H-B46URT4mg) | `youtube`  | `H-B46URT4mg`  |
| [https://www.**twitch**.tv/videos/**1634779211**](https://www.twitch.tv/videos/1634779211)         | `twitch`   | `1634779211`   |
| [https://www.**bilibili**.com/video/**BV1Q44y1B7Wf**](https://www.bilibili.com/video/BV1Q44y1B7Wf) | `bilibili` | `BV1Q44y1B7Wf` |

#### Video Files

If you want to embed a video file directly, use the following syntax:

```hugo
{{</* embed/video.html src="{URL}" */>}}
```

Where `URL` is a URL to a video file e.g. `/path/to/sample/video.mp4`.

You can also specify additional attributes for the embedded video file. Here is a full list of attributes allowed.

- `poster='/path/to/poster.png'` — poster image for a video that is shown while video is downloading
- `title='Text'` — title for a video that appears below the video and looks same as for images
- `autoplay=true` — video automatically begins to play back as soon as it can
- `loop=true` — automatically seek back to the start upon reaching the end of the video
- `muted=true` — audio will be initially silenced
- `types` — specify the extensions of additional video formats separated by `|`. Ensure these files exist in the same directory as your primary video file.

Consider an example using all of the above:

```liquid
{{</*
  embed/video.html
  src="/path/to/video.mp4"
  types="ogg|mov"
  poster="poster.png"
  title="Demo video"
  autoplay=true
  loop=true
  muted=true
*/>}}
```

### Audios

If you want to embed an audio file directly, use the following syntax:

```liquid
{{</*  embed/audio.html src="{URL}" */>}}
```

Where `URL` is a URL to an audio file e.g. `/path/to/audio.mp3`.

You can also specify additional attributes for the embedded audio file. Here is a full list of attributes allowed.

- `title='Text'` — title for an audio that appears below the audio and looks same as for images
- `types` — specify the extensions of additional audio formats separated by `|`. Ensure these files exist in the same directory as your primary audio file.

Consider an example using all of the above:

```hugo
{{</*
  include embed/audio.html
  src='/path/to/audio.mp3'
  types='ogg|wav|aac'
  title='Demo audio'
*/>}}
```

## Pinned Posts

You can pin one or more posts to the top of the home page, and the fixed posts are sorted in reverse order according to their release date. Enable by:

```yaml
---
pin: true
---
```

## Prompts

There are several types of prompts: `tip`, `info`, `warning`, and `danger`. They can be generated by adding the class `prompt-{type}` to the blockquote. For example, define a prompt of type `info` as follows:

```md
> Example line for prompt.
{ .prompt-info }
```

## Syntax

### Inline Code

```md
`inline code part`
```

### Filepath Highlight

```hugo
{{</* /path/to/a/file.extend */>}}
```

### Code Block

Markdown symbols ```` ``` ```` can easily create a code block as follows:

````md
```
This is a plaintext code snippet.
```
````

#### Specifying Language

Using ```` ```{language} ```` you will get a code block with syntax highlight:

````markdown
```yaml
key: value
```
````


#### Specifying the Filename

You may have noticed that the code language will be displayed at the top of the code block. If you want to replace it with the file name, you can add the attribute `file` to achieve this:

````markdown
```shell { file="path/to/file" }
# content
```
````

## Mathematics

We use [**MathJax**][mathjax] to generate mathematics. For website performance reasons, the mathematical feature won't be loaded by default. But it can be enabled by:

[mathjax]: https://www.mathjax.org/

```yaml
---
math: true
---
```

After enabling the mathematical feature, you can add math equations with the following syntax:

- **Block math** should be added with `$$ math $$` with **mandatory** blank lines before and after `$$`
  - **Inserting equation numbering** should be added with `$$\begin{equation} math \end{equation}$$`
  - **Referencing equation numbering** should be done with `\label{eq:label_name}` in the equation block and `\eqref{eq:label_name}` inline with text (see example below)
- **Inline math** (in lines) should be added with `$$ math $$` without any blank line before or after `$$`
- **Inline math** (in lists) should be added with `\$$ math $$`

```markdown
<!-- Block math, keep all blank lines -->

$$
LaTeX_math_expression
$$

<!-- Equation numbering, keep all blank lines  -->

$$
\begin{equation}
  LaTeX_math_expression
  \label{eq:label_name}
\end{equation}
$$

Can be referenced as \eqref{eq:label_name}.

<!-- Inline math in lines, NO blank lines -->

"Lorem ipsum dolor sit amet, $$ LaTeX_math_expression $$ consectetur adipiscing elit."

<!-- Inline math in lists, escape the first `$` -->

1. \$$ LaTeX_math_expression $$
2. \$$ LaTeX_math_expression $$
3. \$$ LaTeX_math_expression $$
```

[mathjax-exts]: https://docs.mathjax.org/en/latest/input/tex/extensions/index.html

## Mermaid

> Mermaid support is under development
{ .prompt-warning }

[**Mermaid**](https://github.com/mermaid-js/mermaid) is a great diagram generation tool. To enable it on your post, add the following to the YAML block:

```yaml
---
mermaid: true
---
```

Then you can use it like other markdown languages: surround the graph code with ```` ```mermaid ```` and ```` ``` ````.

## Learn More

For more knowledge about writing Hugo posts, visit the [Hugo Documentation](https://gohugo.io/documentation/).
