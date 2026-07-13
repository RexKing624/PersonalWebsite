# PersonalWebsite

[中文](#中文) · [日本語](#日本語) · [English](#english)

XerGnik — Designer, Developer & Film Photographer.

## 中文

RexKing624 的个人网站，用于展示个人简介、开发项目、摄影入口与联系方式。

### 技术栈

- Vue 3：组件与响应式状态
- Vite：本地开发和生产构建
- 原生 CSS：响应式布局、深浅主题和滚动动画
- Local Storage：保存语言及主题偏好
- Intersection Observer：实现只播放一次的项目进入动画

### 功能

- 中文、日文、英文三语切换
- 自动检测浏览器语言，无法识别时使用英文
- 默认深色主题，可切换并保存主题偏好
- 个人介绍、Skills、社交账号和联系方式
- 四个精选开发项目及截图、状态和外部链接
- `/photography` 摄影作品占位页
- 桌面端与移动端响应式布局
- 支持系统“减少动态效果”设置

### 本地运行与构建

需要 Node.js 18 或更高版本。

```bash
npm install
npm run dev
```

生成生产版本：

```bash
npm run build
npm run preview
```

## 日本語

RexKing624のプロフィール、開発プロジェクト、写真作品への入口、連絡先を掲載する個人サイトです。

### 技術スタック

- Vue 3：コンポーネントとリアクティブな状態管理
- Vite：ローカル開発と本番ビルド
- Native CSS：レスポンシブレイアウト、ライト／ダークテーマ、スクロールアニメーション
- Local Storage：言語とテーマ設定の保存
- Intersection Observer：初回のみ再生されるプロジェクト表示アニメーション

### 主な機能

- 中国語、日本語、英語の3言語切り替え
- ブラウザ言語の自動判定。判定できない場合は英語を使用
- ダークテーマを標準とし、切り替えた設定を保存
- プロフィール、Skills、SNSアカウント、連絡先
- 4つの開発プロジェクトとスクリーンショット、公開状況、外部リンク
- `/photography` の写真作品プレースホルダーページ
- デスクトップ／モバイル対応
- OSの「視差効果を減らす」設定に対応

### ローカル実行とビルド

Node.js 18以上が必要です。

```bash
npm install
npm run dev
```

本番ビルドを作成する場合：

```bash
npm run build
npm run preview
```

## English

The personal website of RexKing624, featuring a profile, selected development projects, a photography entry point, and contact details.

### Tech stack

- Vue 3 for components and reactive state
- Vite for local development and production builds
- Native CSS for responsive layouts, themes, and scroll motion
- Local Storage for language and theme preferences
- Intersection Observer for one-time project reveal animations

### Features

- Chinese, Japanese, and English language switching
- Automatic browser-language detection with English as the fallback
- Dark theme by default with a persistent theme preference
- Profile, skills, social accounts, and contact details
- Four selected development projects with screenshots, availability, and external links
- A `/photography` placeholder route for future photography work
- Responsive desktop and mobile layouts
- Support for the operating system’s reduced-motion preference

### Local development and build

Node.js 18 or newer is required.

```bash
npm install
npm run dev
```

Create and preview a production build:

```bash
npm run build
npm run preview
```

## Project structure

```text
├── public/assets/          # Logos, portrait, favicon, and project screenshots
├── src/App.vue             # Homepage content, translations, and interactions
├── src/PhotographyPage.vue # Photography placeholder page
├── src/style.css           # Visual system, themes, and responsive styles
├── src/main.js             # Vue entry point
└── vite.config.js          # Vite configuration
```

## License

All rights reserved © 2026 RexKing624.
