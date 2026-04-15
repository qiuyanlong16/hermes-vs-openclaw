---
title: OpenClaw vs Hermes Agent Comparison Report — Design Spec
date: 2026-04-15
status: draft
---

# OpenClaw vs Hermes Agent 对比报告 — 设计文档

## 1. 项目概述

**目标**：创建一个精美的单页 Web 对比报告，从能力、安全性、部署、Channel 接入等维度全面对比 OpenClaw 和 Hermes Agent 两个 AI Agent 平台。

**受众**：技术决策者、开发者、产品团队。中英双语。

**交付物**：单文件 `index.html`（含内联 CSS/JS），通过 `python3 -m http.server` 启动。

## 2. 视觉设计

### 2.1 风格定位

- **科技感 + 专业性** — 类似科技媒体评测页面（如 The Verge、Ars Technica）
- **深色主题为主**，支持深色/浅色切换
- **配色**：OpenClaw 用珊瑚橙色系 (#FF6B35)，Hermes 用金色系 (#FFD700)
- **字体**：Inter / system-ui（英文），Noto Sans SC（中文）

### 2.2 布局结构（从上到下）

```
┌─────────────────────────────────────────────┐
│ 1. HERO — 大标题 + 副标题 + 双语切换        │
├─────────────────────────────────────────────┤
│ 2. 快速结论 — 3 张选型建议卡片              │
├─────────────────────────────────────────────┤
│ 3. 雷达图 — 5 维能力对比 (Chart.js)         │
├─────────────────────────────────────────────┤
│ 4. 对比维度 (6 sections)                    │
│    4a. 能力与架构                           │
│    4b. 安全性                               │
│    4c. 部署方式                             │
│    4d. Channel 接入                         │
│    4e. Memory 与学习能力                    │
│    4f. 生态与扩展                           │
├─────────────────────────────────────────────┤
│ 5. 选型决策树 — 场景推荐                    │
├─────────────────────────────────────────────┤
│ 6. 数据来源与参考链接                       │
└─────────────────────────────────────────────┘
```

### 2.3 响应式

- 桌面端（>1024px）：两栏对比表格
- 平板端（768-1024px）：单栏，卡片堆叠
- 移动端（<768px）：单栏，可折叠面板

## 3. 内容 — 对比维度

### 3.1 能力与架构

| 项目 | OpenClaw | Hermes Agent |
|------|----------|-------------|
| **架构模式** | Hub-spoke（Gateway 中心路由） | 单 Agent 向外扩展 |
| **定位** | 平台/基础设施（Platform） | 自主运营者（Operator） |
| **内置 Skills/Tools** | 52+ Skills | 40+ Tools |
| **并行子 Agent** | 支持 | 支持（spawn isolated subagents） |
| **浏览器控制** | 支持 | 完整 Web 控制（搜索/提取/浏览/视觉/图像生成/TTS） |
| **自动化** | 通过 Gateway | 内置 Cron 调度 |
| **语音模式** | 否 | 是（CLI/Telegram/Discord/Discord VC） |

### 3.2 安全性

| 项目 | OpenClaw | Hermes Agent |
|------|----------|-------------|
| **安全审计** | `openclaw security audit` 内置审计工具 | YOLO Mode + 审批超时 + 永久白名单 |
| **Trust Model** | Trust Boundary Matrix + Hardened Baseline (60s) | 容器隔离 + Docker 安全标志 + 资源限制 |
| **命令审批** | Scope-first 个人助手模型 | 危险命令审批 + 多种审批模式 |
| **平台隔离** | Gateway/Node 信任概念 + 共享 Workspace 风险评估 | 平台白名单 + DM 配对系统 + 环境变量直通控制 |
| **容器安全** | 支持 | 支持 + Sudo 支持 + 文件系统持久化控制 |
| **研究预检** | Researcher preflight checklist | 终端后端安全对比表 |

### 3.3 部署方式

| 项目 | OpenClaw | Hermes Agent |
|------|----------|-------------|
| **后端** | Gateway + Nodes + Plugin 槽位 | 6 种终端后端 |
| **部署方式** | 本地、Docker、K8s | Local、Docker、SSH、Daytona、Singularity、Modal |
| **Serverless** | 否 | Daytona + Modal（空闲时近乎零成本） |
| **安装速度** | 快速安装 | 60 秒安装（Linux/macOS/WSL2） |
| **Web UI** | 内置 Web Control UI + Mobile Nodes | 否（主要 CLI + Gateway） |
| **远程部署** | 远程 macOS Nodes（Linux Gateway） | SSH 后端 + 云端 VM 无需 SSH 直接对话 |
| **配置** | 热重载 + 编程式 RPC 更新 | 配置文件 + Provider 解析 |

### 3.4 Channel 接入

| 项目 | OpenClaw | Hermes Agent |
|------|----------|-------------|
| **数量** | 20+ | 15+ |
| **共同支持** | Slack, Discord, Telegram, WhatsApp, Web | Slack, Discord, Telegram, WhatsApp, CLI |
| **OpenClaw 独有** | iMessage, SMS, 内置浏览器聊天 | — |
| **Hermes 独有** | — | Signal, Matrix, Email, DingTalk(钉钉), Feishu(飞书), WeCom(企业微信), Home Assistant |
| **架构** | Gateway 统一路由，所有消息进同一 session | Messaging Gateway，支持跨平台对话连续性 |
| **团队场景** | 共享 Slack Workspace 风险评估 | DM 配对系统 |

### 3.5 Memory 与学习能力

| 项目 | OpenClaw | Hermes Agent |
|------|----------|-------------|
| **持久化 Memory** | 每个 Assistant 独立存储，团队场景数据隔离 | 三层：Session/Persistent/Skill Memory |
| **检索** | 标准持久化存储 | FTS5 全文检索 + LLM 摘要 |
| **用户建模** | 否 | Honcho 辩证用户建模 |
| **自学习** | 否 | 自动技能生成 + 技能自改进 + 周期性提示 |
| **记忆容量** | 标准 | 容量管理 + 重复预防 + 安全扫描 |
| **外部 Provider** | 否 | 支持外部 Memory Providers |
| **会话搜索** | 否 | Session Search（session_search vs memory） |

### 3.6 生态与扩展

| 项目 | OpenClaw | Hermes Agent |
|------|----------|-------------|
| **GitHub Stars** | ~353K | ~64K |
| **License** | MIT | MIT |
| **Skill 生态** | ClawHub（10,700+ Skills），文件优先级系统 | agentskills.io 兼容，Skills Hub |
| **插件系统** | Plugin slots（Exclusive 槽位）+ npm 可安装 | Plugin System（内置） |
| **MCP** | 支持 | 支持（有实用设置模式和教程） |
| **Model 支持** | 多模型 | 更广泛模型支持（Nous Portal, OpenRouter, OpenAI, 任意端点） |
| **研究能力** | 否 | RL 训练（Atropos）、Batch Processing、Trajectory Export |
| **多 Agent** | Multi-agent routing | Subagent 并行 |

## 4. 雷达图维度与评分

评分标准：1-5 分（基于客观数据对比）

| 维度 | OpenClaw | Hermes | 说明 |
|------|----------|--------|------|
| 生态成熟度 | 5 | 3 | 353K vs 64K stars, 10,700+ vs ~100 skills |
| 自主学习能力 | 2 | 5 | Hermes 有自学习循环和自动技能生成 |
| 部署灵活性 | 3 | 4 | Hermes 有 Serverless 选项和 6 种后端 |
| 安全性 | 4 | 4 | 各有侧重，都较完善 |
| Channel 覆盖 | 5 | 3 | 20+ vs 15+，OpenClaw 覆盖更广泛 |
| Memory 能力 | 3 | 5 | Hermes 有 FTS5 + LLM 摘要 + 用户建模 |
| 上手难度 | 4 | 4 | 都支持快速安装 |
| Web UI | 5 | 2 | OpenClaw 有内置 Web Control UI |

## 5. 选型决策树

### 选择 OpenClaw 如果：
- 需要 **20+ 消息通道** 覆盖（特别是 iMessage, SMS, Web UI）
- 重视 **团队场景** 的数据隔离和共享 Assistant
- 需要 **Web Control UI** 和移动端访问
- 依赖 **庞大 Skill 生态**（10,700+ ClawHub Skills）
- 需要 **内置安全审计工具**

### 选择 Hermes Agent 如果：
- 需要 **自主学习和自我改进** 能力
- 需要 **Signal/钉钉/飞书/企业微信** 等通道
- 需要 **Serverless 部署**（低成本空闲）
- 需要 **语音交互**（CLI/Telegram/Discord）
- 需要 **FTS5 全文检索 + LLM 记忆摘要**
- 进行 **AI 研究**（RL 训练, Batch Processing）

## 6. 技术实现

### 6.1 文件结构

```
oc-vs-hermes/
├── index.html          # 主页面（内联 CSS + JS）
├── docs/
│   └── superpowers/
│       └── specs/
│           └── 2026-04-15-openclaw-vs-hermes-design.md
└── .claude/
    └── mcp.json
```

### 6.2 技术栈

- **纯 HTML/CSS/JS**（无构建步骤）
- **Tailwind CSS**（CDN 版本）
- **Chart.js**（CDN 版本，雷达图）
- **Google Fonts**（Inter + Noto Sans SC）
- **CSS 动画**（Intersection Observer 滚动动画）

### 6.3 语言切换

- 顶部语言切换按钮（中/EN）
- 每个文本元素使用 `data-zh` 和 `data-en` 属性存储双语内容
- JS 切换时替换 `textContent`

### 6.4 启动方式

```bash
cd /home/qiuyanlong/worespace/oc-vs-hermes
python3 -m http.server 8080
# 浏览器打开 http://localhost:8080
```
