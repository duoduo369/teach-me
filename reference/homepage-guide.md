# 生成首页

用户说"生成首页"时加载这份指南。

## 步骤

在 `lessons/` 目录下生成 `homepage.html`，和其他文章同级：

- 封面图 — 用户提供本地路径或 URL，也可让 AI 生成。本地图片复制到该课 `assets/cover/` 下，页面引用 `../assets/cover/xxx.jpg`。URL 直接引用
- 课程标题 — 必填，用户输入
- 一句话简介 — 可选
- 内容列表 — 从 `course-nav.js` 的 lessons 数据生成，列出每课标题和链接
- 首页出现在导航里，像 PPT 的第一页

## HTML 模板

homepage.html 在 `lessons/` 下，CSS/JS 路径与普通文章一致：

```html
<!doctype html>
<html lang="zh-CN">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>课程标题</title>
    <link rel="stylesheet" href="../assets/course.css">
    <script src="../assets/course-nav.js" defer></script>
    <script src="../assets/open-links-new-tab.js" defer></script>
  </head>
  <body>
    <main>
      <section class="hero">
        <p class="eyebrow">课程首页</p>
        <h1>课程标题</h1>
        <p class="lede">一句话简介</p>
      </section>
      <section>
        <figure class="diagram">
          <img src="../assets/cover/xxx.jpg" alt="课程封面">
        </figure>
      </section>
      <section>
        <h2>内容</h2>
        <ul>
          <li><a href="0001-xxx.html">0001 文章标题</a></li>
          <li><a href="0002-yyy.html">0002 文章标题</a></li>
        </ul>
      </section>
    </main>
  </body>
</html>
```

首页只保留导航、标题、封面图、内容列表。