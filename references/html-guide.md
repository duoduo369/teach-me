# HTML 结构

写文章时加载这份指南。

## 文章页模板

一篇干净的 HTML 文章：

```html
<!doctype html>
<html lang="zh-CN">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>文章标题</title>
    <link rel="stylesheet" href="../assets/course.css">
    <script src="../assets/course-nav.js" defer></script>
    <script src="../assets/open-links-new-tab.js" defer></script>
  </head>
  <body>
    <main>
      <section class="hero">
        <p class="eyebrow">简短标签</p>
        <h1>文章标题</h1>
        <p class="lede">一句话说明这篇讲什么。</p>
      </section>
      <!-- 正文 sections -->
    </main>
  </body>
</html>
```

## 结构标准

正面标准，按这个写：

- 标题在桌面宽度下一行排得下
- hero 区干净：eyebrow + h1 + lede。eyebrow 用简短标签。如果有对应的速查页，在 lede 下方加一行速查链接。这个规则只用于普通文章页，不用于 `homepage.html`
- 正文用 `<section>` 分节，每节一个 `<h2>` 起头
- 代码用 `<pre>` + `<code>`，权威中文条文用 `<pre class="prompt-zh">`
- 表格用 `<div class="table-wrap">` 包裹 `<table>`
- 卡片用 `<article class="card">`，网格用 `<div class="grid">`
- 提示用 `<div class="callout">`
- 文章结尾停在最后一段事实

## 配图

只在第三步用户确认要配图时才加：

- 图放在该课 `assets/` 里
- 用 `<figure class="diagram">` 包裹 `<img>` + `<figcaption>`
- 每张图都让一段关系一眼可见
