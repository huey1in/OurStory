# Our Story

![Our Story logo](public/assets/logo.png)

[![GitHub Repo](https://img.shields.io/badge/GitHub-huey1in%2Fourstory-181717?logo=github)](https://github.com/huey1in/ourstory)
![Astro](https://img.shields.io/badge/Astro-7.x-ff5d01?logo=astro&logoColor=white)
<a href="https://linux.do"><img src="https://img.shields.io/badge/LINUX%20DO-社区-f0b752?style=flat-square" alt="LINUX DO"></a>
![Netlify](https://img.shields.io/badge/deploy-Netlify-00c7b7?logo=netlify&logoColor=white)

体验地址：[zhlyxh.com](https://zhlyxh.com)

一个用 Astro 构建的双页纪念站。


## 功能

- 首页 `/`：首屏合照、手绘装饰、在一起天数、时间线、页脚。
- 照片墙 `/photos`：从 `src/content/timeline/*.md` 汇集照片。
- 图片预览：照片墙图片可点击放大，支持背景点击关闭和 `Esc` 关闭。
- 移动端导航：小屏幕使用汉堡菜单。
- 页面切换：使用 Astro `ClientRouter`，并在切换前预热目标页面图片。
- 视觉风格：纸张背景、拍立得照片、胶带、手绘圈选中状态、Laura Cursive 字体。
- 品牌资源：站点 Logo 使用 `public/assets/logo.png`，浏览器标签图标使用 `public/favicon.png`。

## 开发

```bash
npm install
npm run dev
```

常用命令：

```bash
npm run build
npm run preview
```

项目当前只依赖 Astro。

Node.js 版本要求：

```text
>=22.12.0
```

## 内容更新

时间线内容放在：

```text
src/content/timeline/
```

新增一条记录时，可以复制模板：

```text
src/content/timeline/_template.md.example
```

复制后改名为 `.md`，例如：

```text
src/content/timeline/02-some-day.md
```

每条时间线需要这些字段：

```yaml
---
order: 2
date: 2024.10.12
title: 我们的某一天
image: /assets/photos/example.jpg
alt: 这张照片的简短描述
side: right
tilt: tilt-right-soft
---

这里写这一天的故事。
```

字段说明：

- `order`：排序，数字越小越靠前。
- `date`：页面显示的日期文本。
- `title`：时间线标题，也会用于照片墙标题。
- `image`：照片路径，通常放在 `public/assets/photos/`。
- `alt`：图片说明。
- `side`：时间线左右位置，支持 `left`、`right`。
- `tilt`：拍立得倾斜样式，支持 `tilt-left`、`tilt-left-soft`、`tilt-right`、`tilt-right-soft`。

照片墙不需要单独维护，它会读取时间线中的 `image`、`date`、`title` 和 `alt`。

## 项目配置

站点级配置在：

```text
src/site.config.ts
```

当前包含：

- `copyright`：页脚版权信息。
- `heroMedia.image`：首页首屏照片。
- `heroMedia.caption`：首页首屏照片日期。
- `relationship.startDate`：在一起天数的开始日期。

Astro 站点地址配置在：

```text
astro.config.mjs
```

## 目录

```text
src/
  components/        页面组件
  content/timeline/  时间线 Markdown 内容
  layouts/           基础 HTML 布局
  pages/             路由页面
  styles/            全局样式
  site.config.ts     项目配置

public/
  assets/
    illustrations/   手绘插图
    photos/          照片
  fonts/             Laura Cursive 字体
```

## 部署

[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/huey1in/ourstory)

仓库里已经包含 Netlify 配置：

```text
netlify.toml
```

配置会让 Netlify 使用 Node.js 22，执行 `npm run build`，并发布 `dist` 目录。

### 首次部署

1. 打开 Netlify Dashboard。
2. 选择 `Add new site -> Import an existing project`。
3. 连接 GitHub，并选择仓库：

```text
huey1in/OurStory
```

4. Netlify 会读取 `netlify.toml`，构建配置应为：

```text
Build command: npm run build
Publish directory: dist
Node version: 22
```

5. 点击 `Deploy`，等待构建完成。

### 日常更新

以后新增时间线、替换照片或调整配置后，只需要提交并推送：

```bash
git add .
git commit -m "Update story content"
git push
```

Netlify 会自动拉取最新代码并重新部署。

### 自定义域名

如果使用 `zhlyxh.com`，在 Netlify 中进入：

```text
Site configuration -> Domain management
```

添加自定义域名：

```text
zhlyxh.com
```

然后按 Netlify 提示到域名服务商处配置 DNS。正式域名也需要同步写在：

```text
astro.config.mjs
```

当前配置为：

```js
export default defineConfig({
  site: "https://zhlyxh.com",
});
```

### 部署排查

- 如果构建失败，先在本地运行 `npm run build` 查看错误。
- 如果提示 Node.js 版本不支持，确认 Netlify 使用的是 Node.js 22。
- 如果页面没有更新，确认代码已经推送到 Netlify 连接的分支。
- 如果图片没有显示，确认图片放在 `public/assets/photos/`，并且 Markdown 中使用 `/assets/photos/xxx.jpg` 这种路径。
