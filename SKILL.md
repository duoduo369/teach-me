---
name: teach-me
display_name: Teach Me
display_name_en: Teach Me
description: 学习万物。遇到读不懂的文档、啃不透的代码库、总也学不会的技能，用 teach-me 为你生成一套贴合个人学习需求的本地 HTML 课程。
description_zh: 学习万物。遇到读不懂的文档、啃不透的代码库、总也学不会的技能，用 teach-me 为你生成一套贴合个人学习需求的本地 HTML 课程。
description_en: Turn what you want to learn into a course you can keep building on.
version: 1.1.0
author: duoduo369
disable-model-invocation: true
---

用户想学一个东西，或继续已经开始的课程。

这个 skill 会围绕主题搭好一个可持续推进的教学工作区，持续产出 lesson HTML、references 速查页和 learning record。文章要自然、耐读，适合一路学下去，而不是一次性拼出来的资料。

## 教学工作区

当前目录就是教学工作区。学习状态落在这些文件里：

说明：教学工作区里的速查页统一放在 `./references/`。本 skill 自带的说明文件也位于同名的 `references/` 目录里；在实际使用时，教学工作区和 skill 安装目录不是同一个目录。

- `MISSION.md` — 为什么学这个。所有教学决策都从这里出发。格式见 [MISSION-FORMAT.md](references/MISSION-FORMAT.md)。
- `RESOURCES.md` — 高信任资料清单。课里的判断从这里引，不用参数化知识胡编。格式见 [RESOURCES-FORMAT.md](references/RESOURCES-FORMAT.md)。
- `./lessons/*.html` — 学习文章。一篇讲清一件事，是教学工作区的主产物。命名 `0001-<slug>.html`，编号递增。
- `./references/*.html` — 速查页。从文章里压出来的精华——术语表、命令速查、流程图。以后回看走这里。
- `./learning-records/*.md` — 学习记录。类似 ADR，记录非显然的学会的东西，用来算下一篇文章该写什么。命名 `0001-<slug>.md`，编号递增。格式见 [LEARNING-RECORD-FORMAT.md](references/LEARNING-RECORD-FORMAT.md)。
- `./assets/*` — 该课程的可复用组件。导航脚本放这里。
- `GLOSSARY.md` — 术语表。一旦有术语确认，就创建并保持一致。格式见 [GLOSSARY-FORMAT.md](references/GLOSSARY-FORMAT.md)。
- `NOTES.md` — 草稿。记录用户偏好、工作笔记。

## 工作流

### 第一步：确认 MISSION

如果 `MISSION.md` 为空或不存在，先 interview 用户：为什么学这个？学会之后能做什么不一样的事？有什么限制（时间、基础、偏好）？

完成标准：`MISSION.md` 写完，用户确认。模糊的使命比没有使命更糟——追问到底。

### 第二步：确认 RESOURCES

在 `RESOURCES.md` 充实之前，帮用户找到至少 2–3 个高信任资料（一手来源、公认专家、同行评审）。每一条加一行注释：覆盖什么、什么时候用。找不到好资料的地方，写进 `## Gaps`。

完成标准：`RESOURCES.md` 有至少 2 条 Knowledge 条目，或明确标出 Gaps。

### 第三步：询问配图

问用户：**要不要在文章里配图？** 默认不加。配图会消耗更多 token。

如果用户说不要，后续文章不主动加图。如果用户说要，在写文章时判断——图只加在关系用文字不容易一眼看懂的地方（分叉、对照轴、两套同名流程）。

完成标准：用户明确回答要或不要。

### 第四步：写文章

这是主产物。一篇 HTML 文件，放在 `./lessons/`，命名 `0001-<slug>.html`。

**首次生成时：** 把 `course.css` 和 `open-links-new-tab.js` 拷到该课 `assets/` 里。这两份文件在共享位置（通常在 `课程/assets/`），读出来写入该课 `assets/` 即可。后续文章不用再拷。

**写之前：** 读 [references/chinese-writing-guide.md](references/chinese-writing-guide.md)，读 [references/html-guide.md](references/html-guide.md)。

**写的时候：**

- 一篇只讲一件事，讲完就停。
- 知识点从 RESOURCES.md 里引，每篇至少一个外部引用。
- 在读者最近发展区——刚好够挑战，但不会卡住。

**写完之后：** 对正文跑一遍[自检清单](references/chinese-writing-guide.md#自检清单)。代码块、`pre.prompt-zh` 嵌入块、导航列表不动。

完成标准：文章 HTML 存在，正文通过自检清单。

### 第五步：写速查页和 learning-record

文章写完后：

- 如果内容值得以后回看，压一个速查页到 `./references/`。速查表、术语对照、命令列表——这些是回看的东西。不值得回看的文章不压。
- 如果用户展示了真正的理解（不是"这节讲过了"，而是能用对），写一条 learning-record。只记录证明学会了的东西。

完成标准：该压的速查页已压，该记的 learning-record 已记。

### 续写

用户说"继续"、"下一篇"、或在新 session 打 `/teach-me next`：

1. 读 `learning-records/`，算最近发展区
2. 读 `MISSION.md`，确认方向没偏
3. 读 `GLOSSARY.md`，新术语保持一致
4. 生成新文章（第四步）
5. 如果课程 > 1 篇，检查所有已有文章是否都引用了 `course-nav.js`——没有的补上。更新 `course-nav.js` 的列表，加入新文章

### 可选：生成首页

用户说"生成首页"时，读 [references/homepage-guide.md](references/homepage-guide.md)。
首页正文只保留 hero 和封面图，到 cover 就结束；lesson / references 入口交给导航，不在正文里重列。

## 导航

课程只有一篇时：不需要导航。

课程 > 1 篇时，读 [references/nav-guide.md](references/nav-guide.md)。
