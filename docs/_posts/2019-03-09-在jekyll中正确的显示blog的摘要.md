---
title: "2019-03-09-在jekyll中正确的显示blog的摘要"
last_modified_at: 2019-03-09
---
Jekyll文档说显示摘要可以用 post.excerpt，默认是显示第一段，不过我试过他总会显示全部内容，google了一下，如果要显示摘要，用下面语句  

```jekyll
{{ post.excerpt | strip_html | truncatewords:75 }}
```

这个会显示文章起始75个字，完美实现。strip_html的意思是过滤掉html里面所有的tag。估计是如果这75个字里面包含有html tag的话，会导致显示不正常。