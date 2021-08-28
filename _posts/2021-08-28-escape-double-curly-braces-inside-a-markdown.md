---
layout: post
title:  "How to escape double curly braces in Jekyll"
categories: [ markdown, jekyll ]
image: assets/images/jekyll-escape.jpg
tags: [ jekyll, markdown ]
---
If you are using Jekyll for your blog site or writing some code documentation, you may face the issue with the double curly braces. The problem is that Jekyll uses liquid tags and if you try to solve the issue by escaping the special characters in markdown will not solve the problem as well, cause the double curly braces will be ripped out by the liquid processor.

## Prerequisites
* Jekyll

## Solution
{% assign openTag = '{%' %}
{% raw %}
**Step 1**. You will have to use `{% raw %}` tag to ensure that the content is unmodified by Jekyll:
```bash
 {% raw %}
# some command with {{.json}}
{% endraw %}{{ openTag }} endraw %}
```
{% assign openTag = '{%' %}
{% raw %}
**Step 2**. If you have to put your command into a `code block` then you need additional code formatting to make your content render as code:
```bash
 {% raw %}
# ```bash
# I'm a code block that contains {{.json}}
# ```
{% endraw %}{{ openTag }} endraw %}
```

## Conclusion
Using `raw` and `endraw` tags is the quickest way to escape any special character including the double curly braces in Jekyll. Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.