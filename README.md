# xhs-analysis

> 爬取小红书账号的笔记和封面图，自动生成涵盖 7 大维度的账号方法论报告。

一个 Claude Code 技能，可以爬取小红书账号主页、下载封面图，并生成结构化的方法论报告和交互式数据看板。

<img width="1364" height="1183" alt="报告示例 1" src="https://github.com/user-attachments/assets/ce90dc9c-4708-482d-9c52-2dff8eba0f74" />
<img width="1361" height="820" alt="数据看板" src="https://github.com/user-attachments/assets/3dd3925d-e1fb-45ca-b595-005ccc14fe48" />
<img width="1365" height="1180" alt="报告示例 2" src="https://github.com/user-attachments/assets/172bcb42-2766-4a71-8c66-226089751294" />
<img width="1362" height="1163" alt="报告示例 3" src="https://github.com/user-attachments/assets/a8011018-e856-4c1a-b4bc-674dcf04f5b9" />
<img width="1364" height="1137" alt="报告示例 4" src="https://github.com/user-attachments/assets/a3035026-4ddb-40e2-87d6-41bafe2b3752" />

---

## 功能特点

- 🔍 **智能爬取** — 自动打开 Chromium 浏览器，滚动收集最多 100 条笔记数据
- 🖼️ **封面下载** — 自动下载所有笔记封面图，支持视觉分析
- 📊 **七维分析** — 账号概览、爆款拆解、标题模式、封面视觉、选题聚类、内容形式对比、方法论总结
- 📄 **Markdown 报告** — 生成结构化方法论报告（`report.md`）
- 📈 **JSON 数据** — 输出原始分析数据（`analysis.json`）
- 🌐 **交互看板** — 生成可视化 HTML 数据看板（`dashboard.html`）

---

## 目录

- [安装](#安装)
- [快速开始](#快速开始)
- [使用方式](#使用方式)
- [工作原理](#工作原理)
- [输出说明](#输出说明)
- [环境要求](#环境要求)
- [常见问题](#常见问题)

---

## 安装

```bash
# 1. 克隆技能到 Claude Code 目录
cd ~/.claude/skills
git clone https://github.com/zhangchitc/analyze-xiaohongshu.git analyze-xiaohongshu

# 2. 安装依赖
cd analyze-xiaohongshu
pip3 install -r scripts/requirements.txt
playwright install chromium
```

---

## 快速开始

在 Claude Code 中直接输入：

```
/analyze-xiaohongshu https://www.xiaohongshu.com/user/profile/xxx
```

或使用自然语言：

- "分析小红书账号 https://www.xiaohongshu.com/user/profile/xxx"
- "爬取小红书 xxx 的数据并生成报告"
- "生成账号方法论报告"

---

## 使用方式

### 方式一：使用 Skill 命令

```
/analyze-xiaohongshu <小红书链接或用户ID>
```

### 方式二：自然语言描述

直接描述你的需求，Claude Code 会自动调用技能。

### 首次使用

首次运行需要：
1. 自动打开 Chromium 浏览器
2. 扫码登录小红书账号（仅首次需要）
3. 登录后自动开始爬取

---

## 工作原理

```
┌─────────────────────────────────────────────────────────────────┐
│                        analyze-xiaohongshu                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1️⃣ 爬取数据                                                   │
│     └─→ 打开 Chromium，扫码登录，滚动收集最多 100 条笔记        │
│        └─→ 提取：标题、封面图 URL、点赞数、内容类型             │
│                                                                 │
│  2️⃣ 下载封面                                                   │
│     └─→ 保存封面图到本地目录（用于视觉分析）                    │
│                                                                 │
│  3️⃣ 七维分析                                                   │
│     └─→ Claude 读取数据 + 封面图，从 7 个维度深度分析           │
│        ├─ 账号概览与核心数据                                    │
│        ├─ Top 10 爆款笔记拆解                                   │
│        ├─ 标题模式分析                                          │
│        ├─ 封面图视觉分析                                        │
│        ├─ 内容选题聚类                                          │
│        ├─ 内容形式对比（视频 vs 图文）                          │
│        └─ 可复制的方法论总结                                    │
│                                                                 │
│  4️⃣ 生成报告                                                   │
│     └─→ output/report.md（Markdown 方法论报告）                │
│     └─→ output/analysis.json（结构化 JSON 数据）               │
│                                                                 │
│  5️⃣ 构建看板                                                   │
│     └─→ output/dashboard.html（交互式 HTML 数据看板）          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 输出说明

技能运行后会生成以下文件：

| 文件 | 说明 |
|------|------|
| `output/report.md` | 完整的 Markdown 方法论报告，包含 7 大维度分析 |
| `output/analysis.json` | 结构化 JSON 数据，便于程序二次处理 |
| `output/dashboard.html` | 交互式 HTML 数据看板，含图表和可视化 |
| `output/covers/` | 下载的封面图原始文件 |

---

## 环境要求

- Python 3.8+
- [Playwright](https://playwright.dev/python/)（含 Chromium 浏览器）
- 支持 Skill 的 Claude Code

---

## 常见问题

### Q1: 首次使用需要登录吗？
是的，首次运行需要扫码登录小红书账号。登录信息会被浏览器保存，后续无需重复登录。

### Q2: 每次能爬取多少笔记？
最多 100 条笔记（滚动加载的最新笔记）。

### Q3: 封面图保存在哪里？
保存在 `output/covers/` 目录下，按笔记 ID 命名。

### Q4: 支持视频笔记吗？
支持，会分析视频笔记的点赞、收藏等数据，并在内容形式对比维度进行分析。

### Q5: 生成的报告在哪里查看？
报告会自动生成在 `output/` 目录下，用任意 Markdown 预览工具或文本编辑器打开 `report.md` 即可。

---

## 相关资源

- [Claude Code 官方文档](https://docs.anthropic.com/en/docs/claude-code/overview)
- [Playwright Python 文档](https://playwright.dev/python/)
- [小红书开放平台](https://open.xiaohongshu.com/)
