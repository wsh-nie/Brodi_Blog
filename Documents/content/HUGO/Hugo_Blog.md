---
title: "使用HUGO自动生成个人博客网站"
date: 2020-04-03T22:30:54+08:00
draft: false
description : "本文主要介绍配置hugo环境，生成站点，添加主题，运行并修改主题"
---

# 环境配置  

本机通过windows操作系统的一个软件管理器`scoop`来进行安装`hugo`  

首先`scoop search hugo`来确定`hugo`所在的`bucket`，发现在`main`中，所以可以直接使用`scoop install hugo`安装  

# 创建站点  

安装好之后，直接使用`hugo new site [ProgramName]`命令创建原始站点  

自动生成项目的文件结构如下：  

```project

|   archetypes/
|       default.md
|   content/
|   data/
|   layouts/
|   resources/
|   static/
|   themes/
|   config.toml
```
1. `archetypes`文件夹中默认生成`default.md`文档，其中包含使用`hugo new files`创建文档时的预先配置的前端内容  

2. `content`是文档存储文件夹，其中的每个顶级文件夹可以作为文章类型  

3. `data`该文件夹用于存储生成网站时，`Hugo`可以使用的配置文件  

4. `layouts`该文件夹以`html`形式存储`content`中文件在网页中展示的样式  

5. `resources`用于缓存一些文件来加快生成网站速度  

6. `static`存储静态内容，`css`、`javascripts`等  

7. `themes`用于存储已经可用的展示文件的主题样式  

# 添加主题  

切换到`themes`目录`cd themes`

通过`git clone https://github.com/spf13/hyde.git`把选择好的主题下载到`themes`文件夹中，并在项目中(非`themes`文件夹中)的config.toml文件中添加`themes = "hyde"`  

可以在[这里](https://themes.gohugo.io/)查看选择主题  

# 运行

运行项目，生成网站`hugo server --buildDrafts`  

将要展示文件中的`draft`改成`false`可直接使用`hugo server`运行，因为`hugo`默认不对`draft`生成页面

```
Building sites …
                   | EN
-------------------+-----
  Pages            | 10
  Paginator pages  |  0
  Non-page files   |  0
  Static files     |  6
  Processed images |  0
  Aliases          |  0
  Sitemaps         |  1
  Cleaned          |  0
```
# 修改主题

1. 修改`themes/hyde/config.toml`设置网站命名
```js
title = "次人之欢"
```

2. 根据`sidebar.html`展示，修改`themes/hyde/config.toml`设置签名及版权声明
```html
<div class="sidebar-about">
    <a href="{{ .Site.BaseURL }}"><h1>{{ .Site.Title }}</h1></a>
    <p class="lead">
    {{ with .Site.Params.description }} {{.}} {{ else }}{{end}}
    </p>
</div>
```
```js
[params]
description = "精通细节是理解更深和更基本概念的先决条件"
copyright = "Copyright (c) 2020 wsh.nie"
```

3. 修改首页展示文章简略格式  

去除掉不可控的`{{ .Summary }}`改为可控的`{{ .Description }}`  
```html
<!-- {{ .Summary }}-->{{ .Description }}
```
   并修改文件的页面头部Front Matter中设置  
```markdown
description : "本文主要介绍配置hugo环境，生成站点，添加主题，运行并修改主题"
```

4. 修改时间显示格式  

修改`themes/hyde/layouts/index.html`  
```html
<time  class="post-date">{{ .Date.Format "2006-01-02 15:04:05" }}</time>
```  
修改`themes/hyde/layout/_default/single.html`  
```html
<time  class="post-date">{{ .Date.Format "2006-01-02 15:04:05" }}</time>
```

# 参考文章

[HUGO官方文档](https://gohugo.io/getting-started/quick-start/)  
[在Hugo中定制Hyde](https://note.qidong.name/2017/06/22/customize-hyde/)  