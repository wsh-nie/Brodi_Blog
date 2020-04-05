---
title: "在Hugo中使用Latex公式"
date: 2020-04-05T23:48:11+08:00
draft: false
Description: "HUGO 0.68 添加MathJax，方便使用Latex数学公式"
---

使用数学公式基本是刚需，但是Hugo无法直接把markdown文件中的数学公式渲染出效果，所以需要使用JavaScript来渲染。MathJax是目前最流行的解决方案。

因为Hugo版本众多，使用[**零壹轩**](https://note.qidong.name)大佬的方案并不能完美解决问题，在网站看到其他方案，当结合使用时，发现时可以解决问题的，至于原理，暂时就不追究了。

## 添加MathJax.html文件
把如下代码放在`themes/hyde/layout/partials/MathJax.html`文件中
```html
<script type="text/javascript"
        async
        src="https://cdn.bootcss.com/mathjax/2.7.3/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
MathJax.Hub.Config({
  tex2jax: {
    inlineMath: [['$','$'], ['\\(','\\)']],
    displayMath: [['$$','$$'], ['\[','\]']],
    processEscapes: true,
    processEnvironments: true,
    skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
    TeX: { equationNumbers: { autoNumber: "AMS" },
         extensions: ["AMSmath.js", "AMSsymbols.js"] }
  }
});

MathJax.Hub.Queue(function() {
    // Fix <code> tags after MathJax finishes running. This is a
    // hack to overcome a shortcoming of Markdown. Discussion at
    // https://github.com/mojombo/jekyll/issues/199
    var all = MathJax.Hub.getAllJax(), i;
    for(i = 0; i < all.length; i += 1) {
        all[i].SourceElement().parentNode.className += ' has-jax';
    }
});
</script>

<style>
code.has-jax {
    font: inherit;
    font-size: 100%;
    background: inherit;
    border: inherit;
    color: #515151;
}
</style>

```

把这个partial模板添加到<head>中，即可正常工作。
```js
{{ partial "MathJax.html" . }}
```

但是在书写行列式的时候，发现行列式无法进行竖排版。  

结合其他方案:  

1. 将所有下标_用\转义，即\\\_替换所有_
2. 将换行\\\\也用\转义，即换行需要\\\\\\\\\\\\，6个（为了显示这6个，我在.md文档里写了12个）

以下，举个例子：
$$
\mathbf{V}_1 \times \mathbf{V}_2 =  \begin{vmatrix} 
\mathbf{i} & \mathbf{j} & \mathbf{k} \\\\\\
\frac{\partial X}{\partial u} &  \frac{\partial Y}{\partial u} & 0 \\\\\\
\frac{\partial X}{\partial v} &  \frac{\partial Y}{\partial v} & 0 \\\\\\
\end{vmatrix}
$$
完成了！！！

## 参考文章
[在Hugo中使用MathJax](https://note.qidong.name/2018/03/hugo-mathjax/)  
[解决Mathjax数学公式渲染问题](https://ketchuppp.xyz/post/%E8%A7%A3%E5%86%B3Mathjax%E6%95%B0%E5%AD%A6%E5%85%AC%E5%BC%8F%E6%B8%B2%E6%9F%93%E9%97%AE%E9%A2%98/#%E6%B3%A8%E6%84%8F)  