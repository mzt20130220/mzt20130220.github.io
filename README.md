# ZT Blog

一个简洁优雅的 Jekyll 技术博客。

## 快速开始

### 本地运行

1. 确保已安装 Ruby 和 Bundler

2. 安装依赖：
```bash
bundle install
```

3. 启动本地服务器：
```bash
bundle exec jekyll serve
```

4. 打开浏览器访问 http://localhost:4000

### 部署到 GitHub Pages

1. 将项目推送到 GitHub 仓库
2. 在仓库 Settings -> Pages 中启用 GitHub Pages
3. 选择 branch 为 main，folder 为 / (root)
4. 等待几分钟后即可访问

## 写文章

在 `_posts` 目录下创建 Markdown 文件，命名格式：

```
年-月-日-文章标题.md
```

例如：`2024-01-15-javascript-closure.md`

文章头部需要包含以下信息：

```yaml
---
title: "文章标题"
date: 2024-01-15
categories: [分类]
tags: [标签1, 标签2]
excerpt: "文章摘要"
---
```

## 项目结构

```
├── _posts/          # 博客文章
├── _layouts/        # 页面布局模板
├── _config.yml      # 配置文件
├── assets/          # 静态资源
├── index.html       # 首页
├── about.md         # 关于页面
└── archive.md       # 归档页面
```

## 自定义

编辑 `_config.yml` 文件可以修改：
- 网站标题、描述
- 社交链接
- 分页设置等

## 许可证

MIT License