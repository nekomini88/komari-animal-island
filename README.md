# Komari Animal Island

动物森友会（Animal Crossing）暖色调主题，用于 [Komari Monitor](https://github.com/komari-monitor/komari) 监控面板。基于 `komari-theme-emerald` 模板与 animal-island-ui 设计令牌（design tokens）定制，风格温暖治愈。

## 技术栈

- Vue 3 + Vite + reka-ui（shadcn-vue 风格组件）+ Tailwind CSS v4 + Pinia + vue-echarts

## 安装

1. 从 [Releases](https://github.com/nekomini88/komari-animal-island/releases) 下载 `komari-animal-island-build-*.zip`
2. 打开 Komari 管理后台 → 主题管理 → 上传主题 → 选择 zip 文件
3. 刷新页面即可生效

## 开发

```bash
bun install
bun run dev        # 启动开发服务器
bun run lint       # ESLint 检查并修复
```

## 构建

```bash
bun run build      # 类型检查 + 生产构建
```

构建产物（由 `vite.config.ts` 打包）：

- `dist/` — 构建后的静态站点
- `komari-animal-island-build-<sha>.zip` — 可上传到 Komari 的主题包（内含 `dist/` + `komari-theme.json` + `preview.png`）

```bash
bun run preview    # 预览生产构建
```

## 发布

```bash
bun run publish    # 同步提升 package.json 与 komari-theme.json 的版本号并 git add
```

提交并推送后，GitHub Actions（`release-on-version-bump.yml`）检测到版本号变更，会自动构建主题 zip 并创建 GitHub Release。

## 目录结构

```
komari-animal-island/
├── src/                # 主题源码（Vue 组件 / 样式）
├── public/images/      # 运行时静态资源（旗帜 / logo 等）
├── scripts/            # publish.ts 发布脚本、background*.frag 背景着色器
├── komari-theme.json   # 主题清单（名称 / 版本 / 可配置项）
├── docs/preview.png    # 发布预览图
├── vite.config.ts      # 构建与 zip 打包
└── package.json
```

## License

MIT
