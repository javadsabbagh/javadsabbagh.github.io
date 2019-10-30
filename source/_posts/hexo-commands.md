---
title: Hexo Commands
date: 2019-10-02 15:30:38
tags: [hexo, blogging]
---

- Create a new post
```bash
hexo new post <post-file-name>
```

- Running local server
```bash
hexo server
```

- Deploy to github server
```bash
hexo generate && hexo deploy
```

- Add RTL support to your articles:
 - Use *directions: rtl* option in header section, not recommended
 - Wrap your paragraph with  html tag.
 ```html 
 <p dir='rtl' style="background-color: blue;"></p> 
 ```