# PersonalWebsite

RexKing624 的个人网站，展示个人简介、开发项目、摄影入口与联系方式。

## 技术栈

- [Vue 3](https://vuejs.org/)：页面组件与响应式状态
- [Vite](https://vite.dev/)：本地开发与生产构建
- 原生 CSS：响应式布局、深浅主题与滚动进入动画
- Local Storage：保存语言和主题偏好
- Intersection Observer：实现只播放一次的项目进入动画

## 网站功能

- 中文、日文、英文三语切换
- 根据浏览器语言自动选择初始语言，无法识别时使用英文
- 默认深色主题，支持深浅主题切换
- 个人介绍、Skills、社交账号与联系方式
- 四个精选开发项目及对应截图、状态和外部链接
- 独立摄影作品路径 `/photography`，目前为“施工中”占位页
- 桌面端与移动端响应式布局
- 尊重系统的“减少动态效果”设置

## 本地开发流程

要求：Node.js 18 或更高版本。

```bash
npm install
npm run dev
```

浏览器打开 Vite 输出的本地地址即可预览。编辑 `src/` 中的 Vue 组件或样式后，页面会自动更新。

## 构建流程

```bash
npm run build
```

生产文件会生成在 `dist/` 目录。需要在本地检查生产版本时：

```bash
npm run preview
```

## 项目结构

```text
├── public/assets/          # Logo、人物照片与项目截图
├── src/App.vue             # 主页内容、三语文案与交互逻辑
├── src/PhotographyPage.vue # 摄影作品占位页
├── src/style.css           # 全站视觉、主题与响应式样式
├── src/main.js             # Vue 入口
└── vite.config.js          # Vite 配置
```

## 内容更新

- 主页文案和项目资料：`src/App.vue`
- 摄影页面：`src/PhotographyPage.vue`
- 项目截图：`public/assets/projects/`
- 全站样式：`src/style.css`

## License

All rights reserved © 2026 RexKing624.
