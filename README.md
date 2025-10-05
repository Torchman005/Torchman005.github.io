# Luminous's Tiny Home（Hexo 博客）

基于 Hexo 7 与 Reimu 主题的静态博客

Reimu主题原作者站点传送门：[Reimu](https://d-sketon.github.io/)

## 项目概览
- 博客框架：Hexo 7
- 主题：`hexo-theme-reimu`
- 包管理：`pnpm`
- 部署：`hexo-deployer-git` 推送到 GitHub Pages

## 环境要求
- Node.js ≥ 18（推荐 LTS）
- pnpm ≥ 8
- Git（部署与版本管理）

建议使用本项目的本地依赖执行命令（`pnpm exec`），避免全局 Hexo 版本差异导致的问题 

## 快速开始
```bash
# 安装依赖
pnpm install

# 启动本地开发服务器（默认 4000 端口）
pnpm exec hexo server
# 或自定义端口
pnpm exec hexo server -p 4001

# 生成静态文件到 `public/`
pnpm exec hexo generate

# 部署到 GitHub Pages（需先配置凭证）
pnpm exec hexo deploy
```

## 常用命令
- 初始化与清理
  - `pnpm exec hexo clean` 清理缓存与旧构建
- 写作
  - `pnpm exec hexo new post "你的文章标题"` 新建文章到 `source/_posts/`
- 服务器
  - `pnpm exec hexo server -p 4001` 以 4001 端口启动本地预览
- 生成与部署
  - `pnpm exec hexo generate` 生成静态文件
  - `pnpm exec hexo deploy` 推送至远程仓库

也可使用 `package.json` 中的脚本：
- `pnpm run clean` 等价 `hexo clean`
- `pnpm run build` 等价 `hexo generate`
- `pnpm run server` 等价 `hexo server`
- `pnpm run deploy` 等价 `hexo deploy`

## 写作指南
- 基本 Front-matter 示例：
```yaml
---
title: 文章标题
date: 2025-06-17 12:00:00
tags: [标签1, 标签2]
categories: [分类A]
cover: /images/cover.png
lang: zh-CN # 多语言可设置为 en
---
```
- 页面与资源：在 `source/` 下维护（如 `about/`、`tags/`）；图片等资源可放置于 `source/_data/` 或文章同目录的资源文件夹 
- 多语言：主题对 `lang` Front-matter 有支持；英文内容可设 `lang: en` 

## 配置说明
- 站点主配置：`_config.yml`
  - 站点信息、`url`、分页、渲染、部署等（已设置 `skip_render: README.md`，不会被 Hexo 处理）
- 主题配置：`themes/reimu/_config.yml`
  - 外观、动画、搜索、评论、字体、PWA 等丰富配置项
- 语言文案：`themes/reimu/languages/`

## 搜索功能（二选一）
- 生成器搜索（离线 JSON）
  - 主题配置 `generator_search.enable: true`
  - 生成 `public/search.json`，前端本地匹配实现搜索
- Algolia 搜索（在线索引）
  - 依赖 `@reimujs/hexo-algoliasearch`
  - 建议通过环境变量提供敏感信息：
    - `ALGOLIA_APP_ID`
    - `ALGOLIA_ADMIN_API_KEY`
    - `ALGOLIA_INDEX_NAME`
  - 站点 `_config.yml` 中 `algolia` 节填写基础字段；启用 `algolia_search` 时请关闭 `generator_search.enable`（避免冲突）
  - 索引命令：
```bash
pnpm exec hexo algolia      # 索引到 Algolia
pnpm exec hexo algolia -n   # 索引但不清空现有索引
```

## 部署到 GitHub Pages
`_config.yml` 已配置 `hexo-deployer-git`：
```yaml
deploy:
  type: git
  repo: https://github.com/Torchman005/Torchman005.github.io.git
  branch: main
```
部署步骤：
```bash
pnpm exec hexo clean && pnpm exec hexo generate
pnpm exec hexo deploy
```
- 若遇认证问题，优先使用 HTTPS 地址，并在本机配置凭证或使用 Personal Access Token 
- `.deploy_git/` 目录由插件管理，包含部署分支的工作内容 

## 故障排查
- 生成报错或数据未更新
  - 先执行 `pnpm exec hexo clean`，再 `pnpm exec hexo generate`
- 报错 “Spawn failed”（子进程启动失败）
  - 统一使用 `pnpm exec` 执行 Hexo 命令，避免全局版本差异
  - 确认 `node -v`、`pnpm -v`、`git --version` 均可用并在 PATH 中
  - 使用调试模式定位：`pnpm exec hexo generate --debug`
- 端口占用（`EADDRINUSE`）
  - 自定义端口：`pnpm exec hexo server -p 4001`
- 标签或文件名大小写导致的 `ENOENT`
  - 统一大小写并清理后重建：`pnpm exec hexo clean && pnpm exec hexo generate`
- EMFILE（打开文件过多）或内存问题
  - 分批生成或升级环境；参考 Hexo 官方文档建议
- 更多：`https://hexo.io/docs/troubleshooting.html`

## 安全与密钥
- 不要在仓库中明文存储敏感密钥；优先使用环境变量（Algolia 等）
- GitHub Pages 部署建议使用 Token 并限制权限范围

## 目录结构（关键路径）
- `source/`：原始内容（文章、页面、数据）
- `public/`：生成后的静态文件
- `themes/reimu/`：主题配置、脚本与资源
- `scripts/`：自定义标签与扩展（如 `scripts/tags/note.js`）
- `.deploy_git/`：部署分支的工作目录（由 `hexo-deployer-git` 管理）

## 许可与致谢
- 主题：`hexo-theme-reimu`（MIT License）
- 内容版权归作者所有，转载请注明来源 

如需进一步个性化（搜索、评论、PWA、动画、字体等），请编辑 `themes/reimu/_config.yml` 并结合主题注释进行调整 