---
title: "Manacher's Algorithm"
date: 2023-06-09T08:04:10+07:00
author: "Ngo Van Trung"
tags: ["math", "algorithm"]
categories: ["knowledge"]
math: true
draft: true
# cover:
#   image: "https://www.interviewhelp.io/blog/images/leetcode-41.png"
#   alt: "Cover"
#   caption: "Cover"
#   relative: false # To use relative path for cover image, used in hugo Page-bundles
# cover.hidden: true
---

Oke $$d_{odd}[]$$ bla

```cpp
vector<int> manacher_odd(string s) {
    int n = s.size();
    s = "$" + s + "^";
    vector<int> p(n + 2);
    for(int i = 1; i <= n; i++) {
        while(s[i - p[i]] == s[i + p[i]]) {
            p[i]++;
        }
    }
    return vector<int>(begin(p) + 1, end(p) - 1);

}
```

`This is Inline Code`

```html {linenos=true}
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Example HTML5 Document</title>
    <meta
      name="description"
      content="Sample article showcasing basic Markdown syntax and formatting for HTML elements."
    />
  </head>
  <body>
    <p>Test</p>
  </body>
</html>
```
