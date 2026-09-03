# 导航

课程 > 1 篇时加载这份指南。

## 规则

`assets/course-nav.js` 必须是**完整的可执行脚本**——不只是数据，还要有渲染代码。脚本负责：

- 注入顶部导航栏和底部上一课/下一课
- 当前页高亮用 `aria-current="page"`
- 只有 lesson 页在底部补上一课/下一课
- 如果用户生成了 homepage.html，导航里加一个"首页"链接

## 完整示例

```js
// course-nav.js
(function () {
  function basename(p) {
    const parts = String(p || "").replace(/\\/g, "/").split("/").filter(Boolean);
    return decodeURIComponent(parts[parts.length - 1] || "");
  }

  const path = (location.pathname || location.href || "").replace(/\\/g, "/");
  const current = basename(path);

  const lessons = [
    { file: "0001-xxx.html", label: "0001 xxx" },
    { file: "0002-yyy.html", label: "0002 yyy" },
  ];

  const refs = [
    { file: "map.html", label: "速查" },
  ];

  const lessonFiles = {};
  lessons.forEach(function (item) { lessonFiles[item.file] = true; });
  const refFiles = {};
  refs.forEach(function (item) { refFiles[item.file] = true; });

  const inLessons = /\/lessons\//i.test(path) || !!lessonFiles[current];
  const inReferences = /\/references\//i.test(path) || !!refFiles[current];

  const rootPrefix = "..";
  const lessonPrefix = inLessons ? "." : "../lessons";
  const refPrefix = inReferences ? "." : "../references";
  const homepageHref = (inLessons ? "." : "../lessons") + "/homepage.html";
  const sep = '<span class="course-nav-sep" aria-hidden="true">·</span>';

  function link(href, label, isCurrent) {
    if (isCurrent) {
      return '<span class="course-nav-current" aria-current="page">' + label + "</span>";
    }
    return '<a href="' + href + '">' + label + "</a>";
  }

  const lessonLinks = lessons
    .map(function (item) {
      return link(lessonPrefix + "/" + item.file, item.label, current === item.file);
    })
    .join(sep);

  const refLinks = refs
    .map(function (item) {
      return link(refPrefix + "/" + item.file, item.label, current === item.file);
    })
    .join(sep);

  // 上一课 / 下一课
  var prevNext = "";
  if (inLessons) {
    var idx = lessons.findIndex(function (item) { return item.file === current; });
    if (idx >= 0) {
      var parts = [];
      if (idx > 0) {
        parts.push('<a href="' + lessonPrefix + "/" + lessons[idx - 1].file + '">← ' + lessons[idx - 1].label + "</a>");
      } else {
        parts.push('<span class="course-nav-muted">← 已是第一篇</span>');
      }
      if (idx < lessons.length - 1) {
        parts.push('<a href="' + lessonPrefix + "/" + lessons[idx + 1].file + '">' + lessons[idx + 1].label + " →</a>");
      } else {
        parts.push('<span class="course-nav-muted">已是最后一篇 →</span>');
      }
      prevNext = '<div class="course-nav-row course-nav-prevnext">' + parts.join("") + "</div>";
    }
  }

  var nav = document.createElement("nav");
  nav.className = "course-nav";
  nav.setAttribute("aria-label", "课程导航");

  var homepageLink = link(homepageHref, "首页", current === "homepage.html") + sep;

  nav.innerHTML =
    '<div class="course-nav-row">' +
    homepageLink +
    lessonLinks +
    "</div>" +
    (refs.length > 0
      ? '<div class="course-nav-row course-nav-refs">' +
        '<span class="course-nav-label">速查</span>' +
        refLinks +
        "</div>"
      : "") +
    prevNext;

  function mount() {
    var main = document.querySelector("main");
    if (!main) return;
    main.insertBefore(nav, main.firstChild);
    if (prevNext) {
      var bottom = document.createElement("nav");
      bottom.className = "course-nav course-nav-bottom";
      bottom.setAttribute("aria-label", "上一篇下一篇");
      bottom.innerHTML = prevNext;
      main.appendChild(bottom);
    }
  }

  if (document.readyState === "loading") {
    document.addEventListener("DOMContentLoaded", mount);
  } else {
    mount();
  }
})();
```

## 页面引用

页面头部只引用脚本。每课的 `course.css`、`open-links-new-tab.js`、`course-nav.js` 都放在该课自己的 `assets/` 里：

```html
<link rel="stylesheet" href="../assets/course.css">
<script src="../assets/course-nav.js" defer></script>
<script src="../assets/open-links-new-tab.js" defer></script>
```
