# 生成首页

用户说"生成首页"时加载这份指南。

## 步骤

在 `lessons/` 目录下生成 `homepage.html`，和其他文章同级：

- 封面图 — 用户提供本地路径或 URL，也可让 AI 生成。本地图片复制到该课 `assets/cover/` 下，页面引用 `../assets/cover/xxx.jpg`。URL 直接引用
- 课程标题 — 必填，用户输入
- 一句话简介 — 可选
- 首页正文到封面图为止，不在正文里重复 lesson 列表
- references 入口也不在正文渲染；lesson / references 都交给导航脚本展示
- 正文不要出现"内容""速查""参考资料"这类分区标题
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
    </main>
  </body>
</html>
```

首页只保留导航、标题、封面图。
不要在页面正文里再写 lesson 列表、"内容""速查"这类标题，也不要把 `references/` 里的页面列到正文里。
