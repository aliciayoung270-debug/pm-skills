# pm-skills

面向产品经理的 Claude Code 插件，覆盖「原型生成 → PRD文档 → 原型标注」完整需求交付流程。

## 包含的 Skills

### `/prototype-gen` — 产品原型生成

根据需求背景和功能描述，自动生成高保真 HTML 原型页面，并同步推送到 GitHub Pages 预览，需求数据存入 Notion。

**触发方式**：「生成原型」「做个原型」「帮我画页面」「高保真原型」

**执行流程**：
1. 结构化解析需求（需求名称、背景、功能列表、页面模块）
2. 将需求数据写入 Notion 数据库
3. 生成高保真 HTML 原型（飞书/钉钉风格，内联 CSS+JS，支持完整交互）
4. 推送到 GitHub 仓库，生成 GitHub Pages 预览链接
5. 更新导航首页 `index.html`，追加原型入口

---

### `/prd-gen` — PRD 文档生成

原型确认后，根据需求数据自动生成结构完整的 PRD 文档，写入 Notion 并渲染为 HTML 网页版推送 GitHub。

**触发方式**：「生成PRD」「写PRD」「出需求文档」「原型确认了，生成文档」

**执行流程**：
1. 从 Notion 读取需求数据（背景、功能列表、原型链接）
2. 按标准模板生成 PRD（需求背景 / 功能说明 / 数据需求 / 非功能需求 / 里程碑）
3. 写入 Notion 需求页面正文
4. 渲染为带目录导航的 HTML 网页版，推送 GitHub Pages
5. 支持在对话中直接编辑，每次修改自动同步 Notion

---

### `/prototype-annotation` — 原型标注

PRD 确认后，将功能点以模块化聚合方式标注在 HTML 原型上，生成带交互浮窗的标注版原型，研发人员可通过浮窗获取完整开发指令，无需再查阅 PRD。

**触发方式**：「生成原型标注」「标注原型」「把PRD标注到原型上」「加标注」

**两种工作流**：
- **Workflow A（初始化标注）**：针对新页面，将 PRD 功能点聚合标注到原型上
- **Workflow B（标注内容更新）**：针对已有标注，精准更新增量变化

**标注规范**：
- 同一功能区域的需求整合为一个角标+浮窗，避免冗余
- 浮窗支持 Hover 触发、自由拖拽、智能边缘避让
- 需求编号反向写入 PRD，实现双向对应

---

## 完整工作流

```
用户提供需求
     ↓
/prototype-gen   →  高保真原型（GitHub Pages）+ Notion 需求数据
     ↓
确认原型无误
     ↓
/prd-gen         →  PRD 文档（Notion + GitHub Pages HTML版）
     ↓
确认 PRD 无误
     ↓
/prototype-annotation  →  标注版原型（研发直接使用）
```

## 安装

在 Claude Code 的 `settings.json` 中添加：

```json
{
  "plugins": {
    "pm-skills@github:aliciayoung270-debug/pm-skills": true
  }
}
```

## 依赖

本插件依赖以下 MCP 服务，需提前配置：

- **GitHub MCP** — 用于推送原型/PRD 文件到仓库
- **Notion MCP** — 用于读写需求数据库

## 项目配置

首次使用前，修改各 SKILL.md 中的项目配置字段：

| 字段 | 说明 |
|------|------|
| GitHub 用户名 | 你的 GitHub 账号 |
| GitHub 仓库名 | 存放原型和PRD的仓库 |
| GitHub Pages 根地址 | 对应的 Pages 访问地址 |
| Notion 数据库 | PM需求库的 data_source_id |
