# AGENTS.md

## 项目目标
维护并迭代 `KevinGuo-Ink` 个人站点（Astro + astro-pure），优先保证内容发布效率、页面稳定性、SEO 正确性。

## 代码分层与修改边界
- 业务层：`src/**`（默认所有需求都应先在这里完成）。
- 主题层：`astro-pure`（来自 npm 依赖）；`packages/pure/**` 是本地主题源码镜像，**默认不参与构建**。
- 只有当任务明确要求“修改主题内核”时，才修改 `packages/pure/**`，并说明是否需要 `BUN_LINK_PKG=true` + `bun link` 验证。

## 环境与命令约定
- 推荐 Node 版本：`22.x`（Vercel Serverless Runtime 对齐）。
- 默认包管理器：`npm`（仓库有 `package-lock.json`，除非任务明确要求 bun）。

常用命令：
- `npm run dev`
- `npm run check`
- `npm run build`
- `npm run new -- "Post Title"`

## 改动前后工作流
1. 先读相关入口文件再改（页面 -> 布局 -> 配置 -> 组件 -> 插件）。
2. 小步修改，避免一次性大范围重构。
3. 改动后至少运行 `npm run check`。
4. 只要涉及路由、内容渲染、配置、构建链，必须再跑 `npm run build`。
5. 在结果中明确报告：成功/失败、关键告警、残留风险。

## 仓库特定注意事项
- `astro.config.ts` 的 `site` 必须与真实线上域名保持一致（影响 canonical/sitemap/RSS）。
- `src/site.config.ts` 是站点配置单一事实源，菜单、footer、waline、pagefind、typography 等优先在这里改。
- `src/content.config.ts` 定义博客 frontmatter schema，新增内容字段必须先更新 schema。
- 标签路径来自 `tags` 原值；若标签含空格会产生可读性差的 URL，新增标签时优先用 slug 友好命名。
- `src/components/waline/**` 依赖外部 Waline 服务，改动时注意客户端加载与超时行为。

## 不应编辑的内容
- 构建产物与依赖目录：`dist/**`、`.astro/**`、`.vercel/**`、`node_modules/**`。
- 非任务相关文件不做顺手改动（包括格式化全仓、批量重排、无关重命名）。

## 代码风格
- 保持现有技术栈：Astro + TypeScript + UnoCSS，避免引入不必要框架。
- 沿用现有 import 别名：`@/assets`、`@/components`、`@/layouts`、`@/site-config`。
- 遵守仓库格式规则（Prettier + ESLint），不要手动对抗格式化结果。

## 交付要求
- 说明改了什么、为什么改、如何验证。
- 给出可直接复现的命令。
- 若未能完成某一步（例如本地环境限制），明确写出阻塞点与建议下一步。

