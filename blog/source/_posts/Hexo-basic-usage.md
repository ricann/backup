---
title: Hexo basic usage
subtitle: Install, config Hexo and Next
date: 2017-06-13 20:52:33
author: Ricann
head-img: 
categories:  tools
tags:  
 - tools
 - hexo
 - next
keywords:  tools, hexo, next
comments: true
---


## Hexo
### local run
 + npm install hexo-cli -g
 + hexo init blog
 + cd blog
 + npm install
 + hexo server

<!--more-->
 
### add tabs
#### about
>hexo new page "about"

#### tags
create page:
>hexo new page "tags"
 
 attention: you must add following code into index.md:
 >type: "tags"

#### categories
create page:
>hexo new page "categories"
 
attention: you must add following code into index.md:
>type: "categories"

#### local search
 + npm install hexo-generator-search --save
 + add following code into site _config.yml
```
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```
 
### write and deploy
 + hexo new "My New Post"
 + hexo clean
 + hexo generate
 + hexo deploy
 
### links
 + [Hexo](https://hexo.io/)
 + [documentation](https://hexo.io/docs/)
 + [troubleshooting](https://hexo.io/docs/troubleshooting.html)
 + [GitHub](https://github.com/hexojs/hexo/)

## Next
### left align
File: source/css/_schemes/Mist/index.styl
```
.page-post-detail {
  .post-title,
  .post-meta { text-align: left; }
```

Also add following code at the end of the file:
```
.comments {margin: 0 !important;}
#yun-tie-sdk-wrap{padding:0 !important;}
```

### add comments
1. register an ID in [gentie](https://gentie.163.com/)
2. add configuration in themes/next/_config.yml
>gentie_productKey: #your-gentie-product-key

3.  add file *gentie.swig* in *layout/_scripts/third-party/comments/*
```
{% if not (theme.duoshuo and theme.duoshuo.shortname) and not theme.duoshuo_shortname and not theme.disqus_shortname and not theme.hypercomments_id %}
  {% if theme.gentie_productKey %}
    {% set gentie_productKey = theme.gentie_productKey %}
    <script>
      var cloudTieConfig = {
        url: document.location.href, 
        sourceId: "",
        productKey: "{{gentie_productKey}}",
        target: "cloud-tie-wrapper"
      };
    </script>
    <script src="https://img1.ws.126.net/f2e/tie/yun/sdk/loader.js"></script>
  {% elseif theme.gentie_productKey %}
      <div id="cloud-tie-wrapper" class="cloud-tie-wrapper">
      </div>
  {% endif %}
{% endif %}

{% include './comments/gentie.swig' %}
```

### links
 + [Next](http://theme-next.iissnan.com/)
 + [GitHub](https://github.com/iissnan/hexo-theme-next.git)
