---
title: "Testing Hugo's Fenced Round Corner Code Blocks"
date: 2018-10-07T01:31:18+09:00
tags: [hugo, css]
categories: [note, howto]
---

While I really, like the Bootstrap theme from [Xzya](https://github.com/Xzya/hugo-bootstrap), I really want the code block to have large padding and round corner. I couldn't really figure out the way for Hugo to load and apply this single css to all the generated html. Since I don't really want to spend a lot of time figuring this out, I'm just gonna forked [Xzya](https://github.com/Xzya/hugo-bootstrap) repository and add the following css to the `<PROJECT_ROOT>/themes/static/css/style.css`

```css
.highlight {
  margin:0 0 25px;
  overflow:hidden;
  padding:20px;
  background-color:#272822;
  border:1px solid #afcde3;
  border-radius: 16px;
}
```

Also, in order to enable code fenced, I have to enable the Hugo's options in the `config.toml` as well.

``` text
# Code Block
pygmentsCodeFences = true
pygmentsStyle = "monokai"
```

And it works!