# 湖北大学制图组

这是湖北大学制图组官网的 Vue 3 + Element Plus 版本。

## 技术栈

- Vue 3
- Element Plus
- Vue Router
- Vite
- Marked (用于渲染 Markdown 文件)

## 项目结构

```
hubuCartographicGroup/
├── public/
│   ├── content/          # Markdown 内容文件
│   │   ├── personal-info.md
│   │   ├── lab-introduction.md
│   │   ├── mentor-introduction.md
│   │   ├── projects.md
│   │   ├── papers.md
│   │   └── ...
│   └── img/              # 图片资源
├── src/
│   ├── components/
│   │   ├── MarkdownContent.vue    # Markdown 渲染组件
│   │   └── pages/                 # 各个页面组件
│   │       ├── HomePage.vue
│   │       ├── ProjectsPage.vue
│   │       ├── PapersPage.vue
│   │       └── ...
│   ├── views/
│   │   └── Home.vue               # 主视图
│   ├── router/
│   │   └── index.js               # 路由配置
│   ├── App.vue
│   └── main.js
├── index.html
├── package.json
└── vite.config.js
```

## 安装依赖

```bash
npm install
```

## 开发

```bash
npm run dev
```

## 构建

```bash
npm run build
```

## 内容管理

所有页面内容都存储在 `public/content/` 目录下的 Markdown 文件中。每个页面组件对应一个或多个 Markdown 文件：

- `personal-info.md` - 个人信息
- `lab-introduction.md` - 实验室简介
- `mentor-introduction.md` - 导师简介
- `projects.md` / `projects-preview.md` - 科研项目
- `papers.md` / `papers-preview.md` - 学术论文
- `softwriting-patent.md` / `softwriting-patent-preview.md` - 软著专利
- `honour.md` / `honour-preview.md` - 获奖荣誉
- `cooperation-team.md` - 合作团队
- `student-team.md` - 学生团队

修改内容时，只需编辑对应的 Markdown 文件即可，无需修改代码。
