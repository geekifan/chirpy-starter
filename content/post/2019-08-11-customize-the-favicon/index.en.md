---
title: Customize the Favicon
date: 2019-08-11 00:34:00 +0800
description: >-
  Get started with Chirpy basics in this comprehensive overview.
  You will learn how to install, configure, and use your first Chirpy-based website, as well as deploy it to a web server.
categories:
  - Blogging
  - Tutorial
tags:
  - favicon
---

The [favicons](https://www.favicon-generator.org/about/) of [**Chirpy**](https://github.com/cotes2020/jekyll-theme-chirpy/) are placed in the directory {{< filepath src="assets/img/favicons/" >}}. You may want to replace them with your own. The following sections will guide you to create and replace the default favicons.

## Generate the favicon

Prepare a square image (PNG, JPG, or SVG) with a size of 512x512 or more, and then go to the online tool [**Real Favicon Generator**](https://realfavicongenerator.net/) and click the button <kbd>Select your Favicon image</kbd> to upload your image file.

In the next step, the webpage will show all usage scenarios. You can keep the default options, scroll to the bottom of the page, and click the button <kbd>Generate your Favicons and HTML code</kbd> to generate the favicon.

## Download & Replace

Download the generated package, unzip and delete the following two from the extracted files:

- {{< filepath src="browserconfig.xml" >}}
- {{< filepath src="site.webmanifest" >}}

And then copy the remaining image files ({{< filepath src=".PNG" >}} and {{< filepath src=".ICO" >}}) to cover the original files in the directory {{< filepath src="assets/img/favicons/" >}} of your Hugo site. If your Hugo site doesn't have this directory yet, just create one.

The following table will help you understand the changes to the favicon files:

| File(s)             | From Online Tool                  | From Chirpy |
|---------------------|:---------------------------------:|:-----------:|
| `*.PNG`             | ✓                                 | ✗           |
| `*.ICO`             | ✓                                 | ✗           |

<!-- markdownlint-disable-next-line -->
>  ✓ means keep, ✗ means delete.
{ .prompt-info }

The next time you build the site, the favicon will be replaced with a customized edition.
