# 互联网小白

> 盛年不重来，一日难再晨。

博客已迁移至 [DiyDream](https://diydream.yanxin.ga/)，以 VuePress 静态网站形式发布，避免 Markdown 转 HTML 总出现格式 bug。

如果需要 Markdown 同步 WordPress，代码依旧可用，后续更新可参考 [WordPressXMLRPCTools](https://github.com/zhaoolee/WordPressXMLRPCTools)。

<!-- TABLE OF CONTENTS 有序为<ol>，无序为<ul> -->
<details>
  <summary>Table of Contents</summary>
  <ul>
    <li><a href="#-文章目录">📜 文章目录</a></li>
    <li><a href="#-项目说明">✨ 项目说明</a></li>
    <li><a href="#-使用流程">🍥 使用流程</a></li>
    <li><a href="#-使用问题">🤔 使用问题</a></li>
  </ul>
</details>

---start---

_2022.07.23 更新_

## 📜 文章目录

[电脑上实现微信双开，无需网页版或第三方软件](https://newzone.top/posts/2017-04-18-wechat_multi_open.html)

---end---

## ✨ 项目说明

[WordPressXMLRPCTools](https://github.com/zhaoolee/WordPressXMLRPCTools) 能用 Markdown 生成博客，推送更新到 Github 后，通过 Github Actions 自动将文章更新到 WordPress，并将 WordPress 网站的文章索引更新到 Github 仓库的 README.md，供搜索引擎收录。​

基于 WordPressXMLRPCTools，我做了三点修改。

### 发布设置

针对发布功能设置，修改了 `main.py` 和 `Pipfile` 文件。

功能性调整：

- 修复无法覆盖更新文章的 bug；
- 将 markdown 转 HTML 的模块，从 markdown 换为 markdown2，效果待后续确认。

文章样式修改：

- 调整项目页的目录标题和时间格式；
- 将文章 meta 标签从 categories 和 tags 改为更通用的 category 和 tag；
- 文章上方添加文章编辑时间，默认为格林尼治标准时间，比北京晚 8 小时；
- 将文章页尾部的本文永久更新地址从标题格式调整为常规字母；

### 草稿箱

`_post` 路径内新建 `TEMP` 文件夹，用于存放文章草稿。

WordPress 推送程序会忽略 `_post` 子文件夹的内容，换言之，`TEMP` 文件夹不会发布到 WordPress 网站。

### 文章聚合页

文章聚合页有两套方案，分别使用了 docsify 和 VuePress 两种网站生成工具。两者都是将博客文章聚合在一个页面，能对文章进行快速定位和管理。docsify 门槛较低，推荐小白用户选择。

**docsify 方案**在主目录新增 `.nojekyll`，`index.html`，`_sidebar.md` 文件。docsify 示例：<https://rockbenben.github.io/Blog_WP/>。
![](http://tc.seoipo.com/2022-05-26-20-12-56.png)

**VuePress 方案**修改了 `.github/workflows/main.yml`，将博客文章复制到另一个 VuePress 笔记的 repo，以 VuePress 静态网站架构来管理文章知识。VuePress 与 docsify 效果类似，不想折腾 VuePress 的话删除「copy blog」命令即可。VuePress 示例：<https://newzone.top/notes/>。

### 后续修改方向

- 修复 md 转 wordpress 中的层级丢失，这是 markdown 转换中的问题。^[[Python 自动发布 markdown 文章到 WordPress 网站](https://blog.csdn.net/linyusen_com/article/details/82887216)]
  - 测试将 markdown 转为 markdown2 没看到区别，待后续测试。根据 markdown2 issue，它 7 月初已经修复了，但还没同步到发布版本。
  - 很多人说 markdown 有更强的扩展性，考虑是否要换回来。
  - 考虑用 [markdown-it](https://github.com/markdown-it/markdown-it) 来解析 html。
- 在 md 文件中指定发布时间。试过在 `main.py` 中添加 `post_date` 参数，但报错，后续研究下 XMLRPC 的 post 参数。

## 🍥 使用流程

1. 进入项目页面，选择 [原版](https://github.com/zhaoolee/WordPressXMLRPCTools) 或 [修改版](https://github.com/rockbenben/Blog_WP)，点击「Use this template」，复制模板文件。
2. 回到你新建的 repo，删除 \_post 文件夹中的所有文件，参照主目录下 `example_article.md` 的格式编辑文章。
3. 按 [WordPressXMLRPCTools 安装步骤](https://github.com/zhaoolee/WordPressXMLRPCTools#%E7%94%A8github-actions%E5%86%99markdown%E6%96%87%E7%AB%A0%E8%87%AA%E5%8A%A8%E6%9B%B4%E6%96%B0%E5%88%B0wordpress) 执行，如遇报错，查看下方使用问题。
4. 修改主目录下的 `index.html` 和 `_sidebar.md`，调整 docsify 网页设置。
   - `index.html` 修改 docsify 网页标题、描述和关键词。
   - `_sidebar.md` 修改 docsify 网页侧边栏，加入博客文章的标题和位置。

## 🤔 使用问题

### 文章发布不成功

`_post` 文件夹添加了文档，但同步后，`README.md` 和 WordPress 并没更新文章。

- 文章后缀必须为「.md」，不支持「.markdown」或其他后缀格式。
- 进入 repo 页面中的 `Actions`，检查最近一次的 update 是否正确。
  ![](http://tc.seoipo.com/2022-05-26-20-36-56.png)

### Error: git denied to github-actions[bot]

遇到 GitHub Actions 报错：`git denied to github-actions[bot]` 或 `Process completed with exit code 128`。

依次点击该 repository 的`Setting - Code and automation - Actions - General`，然后在 Workflow permissions 中开启「Read and write permissions」。

### Error: Process completed with exit code 1

遇到 GitHub Actions 报错：`Error: Process completed with exit code 1`，可能是文章内容触发了服务器的安全规则，拒绝了文章的同步。

如果出现该项报错，暂时关闭服务器防火墙，可解决问题。同步完成后，记得重新开启防火墙。

### 无法覆盖更新原文章 ​

修改旧文章并同步后，WordPress 站的文章没同步修改，而是新增了一篇相同的文章。

- 检查 md 文件名有没有出现大写字母，有没有更改文件名。
- 进入 WordPress 后台，设置 - 固定链接，选中自定义结构，并将文章链接设为 `/p/%postname%`。

如果修改版在检查后依然出现此问题，建议手动将新文章内容覆盖旧文章，然后删除新文章。​ 这个 bug 可以当作是强提醒。当 WordPress 新增了旧文章，你就被提醒要在其他平台修改该文章，让文章版本保持统一。​

### WordPress 发布时间与实际不符 ​

同步文章后，WordPress 显示的文章发布时间是 GitHub push 时间，而非文章真实的发布时间。​

如果你将旧文章转移到 WordPress，文章的发布时间需在 WordPress 后台手动修改，无法在 Markdown 文件中指定 WordPress 显示的发布时间。

### 有序列表编号有误

文章中设定的编号是 `3`，但同步到 WordPress 后，变成了 `1`。

有序列表中穿插了图片、段落，编号就会重置到 `1`。去除图片与旧序列的空行，就能识别正确编号。

### 无序列表只有一个层级

Markdown 转 WordPRess 文章时，默认规则无法识别缩进级别。多层级列表只会显示一个层级，多层级的列表也只会显示到第一层级。

### 代码块样式古怪

如果代码块跟在列表后并使用多个空格递进了，wordpress 会将其识别为行内代码，呈现高亮模式。

只有在无层级时，wordpress 才能正确识别代码块。
