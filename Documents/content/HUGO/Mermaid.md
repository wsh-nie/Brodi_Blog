---
title: "HUGO中使用Mermaid作流程图"
date: 2020-04-06T11:20:50+08:00
draft: false
Description: "本文在HUGO中集成Mermaid进行绘制流程图"
---

## Mermaid  

`Mermaid` 是一个用于绘制流程图、状态图、时序图的`JS`库。  

`Mermaid` 会将流程图绘制成`SVG`格式渲染在网页上。  

## 引入Mermaid  

在`themes/hyde/layouts/partials/head.html`中引入`Mermaid`  

```js
<!-- mermaid JS -->
<script src="https://unpkg.com/mermaid/dist/mermaid.min.js"></script>
<script>mermaid.initialize({startOnLoad:true});</script>
```
## 使用Mermaid

在`markdown`文件中使用如下形式代码进行绘制流程图  
```html
<div class="mermaid">
    graph TD
        D[Data] & I(Input) --> H[hypothesis] 
        H[hypothesis] --> O(Output)
</div>
```
发现似乎没用，右键检查网页发现`markdown`中插入的`html`被渲染成`<!-- raw HTML omitted -->`

## Hugo版本的安全性

原来是因为`Hugo`自`0.60.0`版本以后安全性得到了提高，所以需要关闭安全模式。  

在`config.toml`文件中加入如下
```toml
[markup.goldmark.renderer]
unsafe= true
```

最后上述流程图代码效果如下：
<div class="mermaid">
    graph TD
        D[Data] & I(Input) --> H[hypothesis] 
        H[hypothesis] --> O(Output)
</div>

## 参考文章  
[Hugo中使用Mermaid](https://blog.hgtweb.com/2019/hugo-mermaid/)  
[Hugo 0.60.0 Raw HTML omitted Issue](https://discourse.gohugo.io/t/hugo-0-60-0-raw-html-omitted-issue/22010)  
[mermaid官方文档](https://mermaid-js.github.io/mermaid/#/)  
[mermaid github仓库](https://github.com/mermaid-js/mermaid)  