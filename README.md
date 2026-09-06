# 个人博客使用说明

本仓库是一个用 **Hexo 8** + **Stellar 1.44** 主题搭建的静态博客，部署在 GitHub Pages。
读完这份说明，你就能独立完成「写文章 → 本地预览 → 推到 GitHub → 网站自动更新」的全流程。

> **约定**：下面所有命令都在项目根目录 `C:\Users\user\Desktop\myblog` 下执行；
> 终端默认是 Git Bash。Windows 用户也可以用 PowerShell，但路径写法略有不同。

---

## 目录速览

```
myblog/
├── _config.yml              站点主配置（站点名、作者、头像等）
├── _config.stellar.yml      Stellar 主题配置（菜单、评论、渐变等）
├── source/                  源文件（你写文章、放图片都在这里）
│   ├── _posts/              博客文章（Markdown 文件）
│   ├── about/               关于页面
│   └── images/              图片资源
├── themes/stellar/          Stellar 主题（git submodule，不要手动改里面的代码）
├── scaffolds/               文章/页面模板
├── public/                  hexo generate 输出的静态站点（自动生成，别手动改）
├── .github/workflows/       GitHub Actions 自动部署脚本
└── package.json             Node 项目清单
```

---

## 一、第一次拉取代码后要做的事

如果你换了一台电脑，或者仓库刚从 GitHub 上 clone 下来，要先把依赖和子模块装上。

### 第 1 步：安装 Node.js

本博客在 Node.js 22 上验证通过。推荐用 [nvm-windows](https://github.com/coreybutler/nvm-windows) 或直接装 v22。

打开终端确认：
```bash
node -v
# 期望输出：v22.22.2 或更高 22.x
```

### 第 2 步：启用 pnpm

Hexo 在这个项目里用 pnpm 装包（而不是 npm）。pnpm 走 corepack 自带，不需要单独装：

```bash
corepack enable
corepack prepare pnpm@latest --activate
```

### 第 3 步：克隆仓库并进入目录

```bash
git clone https://github.com/LDX613/LDX613.github.io.git
cd LDX613.github.io
```

### 第 4 步：初始化子模块

Stellar 主题是 **git submodule**，默认 clone 不会拉下来，必须手动：

```bash
git submodule update --init --recursive
```

> **为什么需要这一步**：themes/stellar/ 目录在 GitHub 上只是一个指针文件，不含真实代码。
> 没有这一行，主题目录是空的，`hexo generate` 会失败。

### 第 5 步：安装依赖

```bash
pnpm install
```

这一步会把 Hexo、所有插件（feed、搜索等）装到 `node_modules/`。

---

## 二、写一篇新博客

### 第 1 步：创建文章文件

**推荐方式：命令行创建（自动套用模板）**

```bash
pnpm hexo new "我的第一篇文章"
```

执行成功后会在 `source/_posts/我的第一篇文章.md` 生成一个文件，
里面已经填好了默认的 frontmatter（标题、日期等）。

**手动方式**

直接在 `source/_posts/` 下新建一个 `.md` 文件，文件名用英文或拼音（避免中文文件名引发编码问题），然后复制下面这个最小 frontmatter：

```markdown
---
title: 我的第一篇文章
date: 2026-09-06 13:00:00
tags:
  - 随笔
categories:
  - 生活
---

# 正文从这里开始

写你想写的 Markdown 内容……
```

### 第 2 步：编辑 frontmatter（文章头部信息）

frontmatter 是每篇文章开头的 `---` 包裹的 YAML 段，决定这篇文章怎么展示。
**常用字段**：

| 字段 | 作用 | 例子 |
|---|---|---|
| `title` | 文章标题 | `我的第一篇文章` |
| `date` | 发布时间 | `2026-09-06 13:00:00` |
| `tags` | 标签（多个） | `[Hexo, 建站]` |
| `categories` | 分类（一般一个） | `[随笔]` |
| `description` | 摘要（首页卡片显示） | `一篇介绍建站的随笔` |
| `cover` | 封面图（卡片背景） | `/images/cover.jpg` |
| `banner.image` | 顶部横幅图 | `/images/banner.jpg` |
| `card.cover` / `banner.headline` / `banner.tagline` | Stellar 主题特有字段 | 见官方文档 |

完整字段参考 `scaffolds/post.md`，里面有 Stellar 主题支持的几乎所有选项。

### 第 3 步：写正文

正文就是标准 Markdown，支持：

- 标题：`# / ## / ###`
- 代码块：```` ``` ````
- 引用：`> 文字`
- 链接：`[文字](https://...)`
- 图片：`![描述](/images/xxx.jpg)`
- 数学公式（需要在前置里 `render.math: katex`）
- 流程图（需要在前置里 `render.diagrams: mermaid`）

### 第 4 步：本地预览

```bash
pnpm hexo server
```

浏览器打开 `http://localhost:4000/`，你会看到：
- 首页文章列表
- 点进单篇文章
- 分类、标签、归档页
- 关于页

> ⚠️ **两个容易踩的坑**：
>
> 1. **改了 `_config.yml` 或 `_config.stellar.yml` 后**，hexo server **不会自动重载**——
>    必须 Ctrl+C 关掉再重启。改主题模板文件会自动 rebuild，不用重启。
> 2. 浏览器可能缓存，按 **Ctrl + Shift + R** 硬刷新才能看到最新效果。

### 第 5 步：停掉本地预览

预览完想关掉，按 **Ctrl + C**。

---

## 三、把博客发到网上（部署）

部署是 **全自动** 的：你只需要把代码 push 到 GitHub，GitHub Actions 会自动跑 `hexo generate` 并发布到 GitHub Pages。

### 第 1 步：提交修改

```bash
git add .
git status   # 检查一下都改了哪些文件
git commit -m "新增：xxx 文章"
```

> **注意 `.gitignore`**：
> `node_modules/`、`public/`、`db.json`、`.workbuddy/` 都被 ignore 了，
> 不会进 git。**你只需要 commit 你的文章、配置改动**。

### 第 2 步：推送到 GitHub

```bash
git push origin master
```

### 第 3 步：等待 GitHub Actions 完成

- 打开 `https://github.com/LDX613/LDX613.github.io/actions`
- 看到绿色的 ✓ 表示部署成功
- 一般 1-3 分钟

### 第 4 步：访问网站

部署成功后访问：
**https://LDX613.github.io**

### 首次部署要做的一次性配置

如果你在 GitHub 上是第一次部署，还需要：

1. 进入仓库 → **Settings** → **Pages**
2. **Source** 选 **GitHub Actions**（不是 "Deploy from a branch"）
3. 进入 **Settings** → **Actions** → **General** → **Workflow permissions**
4. 选 **Read and write permissions**，保存

---

## 四、添加图片

把图片放在 `source/images/` 下，文件名用英文或拼音。
然后在文章里引用：

```markdown
![描述](/images/my-photo.jpg)
```

> **为什么是 `/images/xxx.jpg` 而不是 `./images/xxx.jpg`**：
> 站点最终部署在 `LDX613.github.io`，根 URL 是 `https://LDX613.github.io/`，
> 所以图片绝对路径就是 `/images/xxx.jpg`。

---

## 五、修改主题样式与配色

### 改菜单高亮色、侧边栏颜色、全局渐变

打开 `_config.stellar.yml`：

- `menubar.items[*].theme` —— 每个菜单项的高亮颜色（16 进制色值）
- `style.leftbar.background-color-light / -dark` —— 侧边栏底色
- `inject.head` —— 直接塞到 `<head>` 末尾的 CSS，最适合放渐变背景

### 加自定义样式（不想动主题源码）

在 `source/_data/` 下新建 `styles.styl`（如果不存在），
写 Stylus 语法，Stellar 会自动编译进主样式表：

```stylus
// 自定义样式示例
.post-title {
  font-weight: 700;
}
```

或者更简单：在 `inject.head` 里直接写 `<style>` 块，**别忘了加 `!important`** 避免被主题样式覆盖。

---

## 六、常用命令速查

| 命令 | 作用 |
|---|---|
| `pnpm hexo new "标题"` | 创建新文章 |
| `pnpm hexo new page "标题"` | 创建新独立页面 |
| `pnpm hexo server` | 启动本地预览（端口 4000） |
| `pnpm hexo generate` | 生成静态文件到 `public/` |
| `pnpm hexo clean` | 清空 `public/` 和缓存（出问题时用） |
| `pnpm hexo deploy` | 手动部署（一般不用，GitHub Actions 自动跑） |
| `git submodule update --remote` | 拉取主题最新版本（升级主题用） |

---

## 七、常见问题

### Q1：本地预览时页面样式乱了 / 图片加载失败

多半是浏览器缓存。按 **Ctrl + Shift + R** 硬刷新。

### Q2：改了 `_config.stellar.yml` 后看不到效果

hexo server 不会自动重载配置。停掉服务（Ctrl+C）再 `pnpm hexo server` 重启。

### Q3：GitHub Actions 部署失败

进 `https://github.com/LDX613/LDX613.github.io/actions` 看报错日志。
最常见的原因：

- **子模块没拉到**—— 检查 `.github/workflows/deploy.yml` 里有 `submodules: recursive`
- **权限不够**—— Settings → Actions → Workflow permissions 改成 Read and write
- **Pages 没启用**—— Settings → Pages → Source 选 GitHub Actions

### Q4：写完文章忘了加分类 / 标签，分类页看不到这篇文章

Hexo 必须在 frontmatter 里至少有 `categories:` 或 `tags:` 才会把文章列入分类/标签页。
加了之后要 `pnpm hexo generate`（或本地预览会自动跑）才会生成对应的 `categories/xxx/index.html` 和 `tags/xxx/index.html`。

### Q5：升级 Stellar 主题后样式大变 / 部分功能失效

主题配置字段可能变了。打开 https://xaoxuu.com/wiki/stellar/ 看新版文档，对照 `CHANGELOG.md` 的「升级注意」做迁移。

升级主题命令：
```bash
cd themes/stellar
git pull origin main
cd ..
git add themes/stellar
git commit -m "升级 Stellar 主题"
```

⚠️ 注意：我们之前改过 `themes/stellar/layout/{categories,tags,archive}.ejs`（修复菜单高亮），
升级时**这几个文件会被覆盖**。需要重新打 patch 或手动改一遍。

---

## 八、写博客的推荐节奏

最后给你一个日常发博客的最佳实践流程，避免来回返工：

1. **`pnpm hexo new "标题"`** 先创建文件骨架
2. **同时准备好封面/横幅图**（如果有的话），丢进 `source/images/`
3. **先填好 frontmatter**（title / date / tags / categories / cover）再写正文
4. **`pnpm hexo server`**，边写边看效果
5. **本地确认无误后 Ctrl+C 停服务**
6. **`git add . && git commit -m "..." && git push`** 完成发布
7. **去 Actions 看一眼绿色 ✓，再去网站验证**

有任何问题随时问我 🙂