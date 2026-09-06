# L.D.X. 的小屋

> 一个基于 **Hexo** + **Stellar** 主题的静态个人博客。

[![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-live-brightgreen?style=flat-square)](https://ldx613.github.io)
[![Hexo](https://img.shields.io/badge/Hexo-8.1-blue?style=flat-square&logo=hexo)](https://hexo.io)
[![Stellar](https://img.shields.io/badge/Stellar-1.44.0-3DC550?style=flat-square)](https://github.com/xaoxuu/hexo-theme-stellar)

## 在线访问

**https://ldx613.github.io**

---

## 简介

这是我的个人博客，用来记录学习、技术和生活里的各种想法。

博客采用 [Hexo](https://hexo.io/) 作为静态站点生成器，配合 [Stellar](https://github.com/xaoxuu/hexo-theme-stellar) 主题，整体风格清新简洁；内容使用 Markdown 编写，通过 GitHub Actions 自动部署到 GitHub Pages。

---

## 技术栈

| 项目 | 说明 |
|---|---|
| [Hexo](https://hexo.io/) | 静态博客生成器 |
| [Stellar](https://github.com/xaoxuu/hexo-theme-stellar) | 博客主题 |
| [GitHub Pages](https://pages.github.com/) | 静态站点托管 |
| [GitHub Actions](https://github.com/features/actions) | 自动化构建与部署 |
| [pnpm](https://pnpm.io/) | 包管理工具 |

---

## 主要特性

- 清新四色渐变背景（亮色 / 暗色双主题）
- 响应式侧边栏菜单，点击后正确高亮当前分类
- giscus 评论系统（基于 GitHub Discussions）
- RSS 订阅支持
- 自定义头像与 favicon
- 自动化的 GitHub Actions 部署流程

---

## 本地运行

### 1. 克隆仓库

```bash
git clone https://github.com/LDX613/LDX613.github.io.git
cd LDX613.github.io
```

### 2. 安装依赖

本项目使用 pnpm 管理依赖：

```bash
corepack enable
corepack prepare pnpm@latest --activate
pnpm install
```

### 3. 启动本地预览

```bash
pnpm hexo server
```

浏览器打开 `http://localhost:4000/` 即可查看。

> 修改 `_config.yml` 或 `_config.stellar.yml` 后，需要**重启** hexo server 才能生效。

### 4. 构建静态站点

```bash
pnpm hexo generate
```

生成后的静态文件位于 `public/` 目录。

---

## 项目结构

```
myblog/
├── _config.yml              # Hexo 站点主配置
├── _config.stellar.yml      # Stellar 主题配置
├── source/                  # 博客源文件
│   ├── _posts/              # 文章（Markdown）
│   ├── about/               # 关于页面
│   └── images/              # 图片资源
├── themes/stellar/          # Stellar 主题（普通目录）
├── scaffolds/               # 文章模板
├── .github/workflows/       # GitHub Actions 部署脚本
└── package.json             # 项目依赖
```

---

## 部署

项目已配置 GitHub Actions，提交代码到 `master` 分支后会自动构建并部署到 GitHub Pages。

```bash
git add .
git commit -m "新增文章"
git push origin master
```

部署进度可在 [Actions](https://github.com/LDX613/LDX613.github.io/actions) 页面查看。

---

## 写作

新建一篇文章：

```bash
pnpm hexo new "文章标题"
```

然后在 `source/_posts/` 下编辑生成的 Markdown 文件即可。

---

## 许可

除特别声明外，博客原创内容采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) 许可协议。

主题代码版权归 [Stellar](https://github.com/xaoxuu/hexo-theme-stellar) 原作者所有。
