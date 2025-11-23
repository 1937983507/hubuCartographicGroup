# 迁移说明

## 已完成的工作

### 1. 项目结构搭建
- ✅ 创建了 Vue 3 + Vite 项目基础配置
- ✅ 配置了 Element Plus 组件库
- ✅ 配置了 Vue Router
- ✅ 配置了 Marked 用于 Markdown 渲染

### 2. 组件开发
- ✅ `Home.vue` - 主视图，包含导航栏、左侧栏和右侧内容区
- ✅ `MarkdownContent.vue` - Markdown 内容渲染组件
- ✅ `HomePage.vue` - 首页组件
- ✅ `ProjectsPage.vue` - 科研项目页面
- ✅ `PapersPage.vue` - 学术论文页面
- ✅ `SoftwritingPatentPage.vue` - 软著专利页面
- ✅ `HonourPage.vue` - 获奖荣誉页面
- ✅ `CooperationTeamPage.vue` - 合作团队页面
- ✅ `StudentTeamPage.vue` - 学生团队页面

### 3. 内容抽离
所有 HTML 中的静态内容已抽离到 `public/content/` 目录下的 Markdown 文件：

- ✅ `personal-info.md` - 基本信息
- ✅ `lab-introduction.md` - 实验室简介
- ✅ `mentor-introduction.md` - 导师简介
- ✅ `projects.md` / `projects-preview.md` - 科研项目（完整版/预览版）
- ✅ `papers.md` / `papers-preview.md` - 学术论文（完整版/预览版）
- ✅ `softwriting-patent.md` / `softwriting-patent-preview.md` - 软著专利（完整版/预览版）
- ✅ `honour.md` / `honour-preview.md` - 获奖荣誉（完整版/预览版）
- ✅ `cooperation-team.md` - 合作团队（待补充内容）
- ✅ `student-team.md` - 学生团队（待补充内容）

### 4. 功能实现
- ✅ 页面切换功能
- ✅ 返回顶部功能
- ✅ Markdown 内容动态加载和渲染
- ✅ 响应式布局
- ✅ 导航栏高亮显示

## 待完成的工作

### 1. 图片资源迁移
需要将以下图片从 `fabricTagCloud3/img/` 复制到 `public/img/`：
- `hubu-logo.png`
- `chengxiaoqiang.png`
- `labIntroduction.png`
- `mentor.png`
- `projects.png`
- `papers.png`
- `softwriting&patent.png`
- `honour.png`

### 2. 样式优化
- 可以根据需要进一步优化样式，使其更接近原页面
- 可以添加更多 Element Plus 组件来增强交互体验

### 3. 内容补充
- 补充合作团队和学生团队的内容到对应的 Markdown 文件

## 使用方法

1. 安装依赖：
```bash
npm install
```

2. 复制图片资源到 `public/img/` 目录

3. 启动开发服务器：
```bash
npm run dev
```

4. 修改内容时，只需编辑 `public/content/` 目录下对应的 Markdown 文件即可

## 技术特点

- **内容与代码分离**：所有内容都存储在 Markdown 文件中，便于维护
- **组件化设计**：每个页面都是独立的组件，便于扩展
- **响应式布局**：使用 Flexbox 实现响应式布局
- **现代化技术栈**：Vue 3 + Element Plus + Vite

