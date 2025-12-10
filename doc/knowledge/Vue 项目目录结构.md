## Vue 项目目录结构

使用 Vue CLI 创建的项目通常包含以下核心文件和文件夹

```
my-vue-project/
├── node_modules/          # 项目依赖包
├── public/                # 静态资源（不经过构建处理）
├── src/                   # 源代码目录（核心）
├── tests/                 # 测试文件（可选）
├── .gitignore            # Git 忽略配置
├── package.json          # 项目配置和依赖
├── README.md             # 项目说明文档
└── vue.config.js         # Vue 配置文件（可选）
```

### public 目录

- 作用：存放静态资源，这些文件会直接复制到构建输出目录，不经过 Webpack 处理
- 关键文件：
index.html：应用的主 HTML 模板文件
favicon.ico：网站图标
- 使用场景：需要直接引用路径的资源（如图标）、与 webpack 不兼容的库。

### src 目录

这是项目的核心开发目录，包含了所有源代码和资源

#### 入口文件

- main.js：Vue 应用的入口文件，负责初始化 Vue 实例、挂载到 DOM 和全局配置
- App.vue：根组件，包含应用的全局布局和结构

#### 资源与组件目录

- assets/：存放会被 Webpack 处理的静态资源
- components/：可复用的 Vue 组件
- views/或 pages/：页面级组件（通常与路由对应）

#### 路由与状态管理

- router/：存放路由配置文件，定义 URL 路径与组件的映射关系
- store/：Vuex 状态管理相关文件，用于集中管理应用状态

#### 工具与服务层

- utils/​ 或 helpers/：工具函数库
- services/：API 请求和服务层封装
- plugins/：Vue 插件配置

### 项目配置文件
- package.json：定义项目依赖、脚本命令和元数据，包含项目名称、版本、许可证等信息
- vue.config.js：Vue 项目的自定义配置文件