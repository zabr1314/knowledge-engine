# 🧠 Knowledge Engine

> 个人知识引擎 — 把碎片化的阅读和思考变成结构化的、可搜索的、会成长的知识系统。

[![OpenClaw](https://img.shields.io/badge/OpenClaw-Skill-blue)](https://openclaw.ai)
[![Python](https://img.shields.io/badge/Python-3.8+-green)](https://python.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## 它是什么

不是"笔记软件"，而是一个**会成长的认知系统**：

```
L3 元规律   ← 自动蒸馏，跨维度底层逻辑
   ↑
L2 洞察     ← 同主题 ≥3 个事实自动聚合
   ↑
L1 事实     ← 手动添加，每天积累
```

每天把读到的、想到的、学到的拆成原子概念，系统自动帮你：
- 🔍 **搜索**：找到之前积累的相关知识
- 🔄 **去重**：添加时自动检测相似概念，避免碎片化
- 🔗 **关联**：概念之间建立支持、证据、对比等关系
- 💭 **信念追踪**：记录观点的变化轨迹，带时间衰减
- 🏗️ **自动蒸馏**：L1 事实 → L2 洞察 → L3 元规律
- 📊 **可视化**：交互式知识图谱

## 为什么做这个

读了很多文章，收藏了很多链接，但知识是**散的**。

- 今天看到一个好观点，明天忘了在哪
- 两个有关联的概念，从来没被放在一起想过
- 一年前的洞察，跟今天的认知有什么关系？

Knowledge Engine 解决的就是这个问题：让知识**累积**而不是**消耗**。

## 快速开始

### 安装

```bash
# 通过 ClawHub
npx clawhub@latest install knowledge-engine

# 或者手动克隆
git clone https://github.com/zabr1314/knowledge-engine.git ~/.openclaw/workspace/skills/knowledge-engine
```

### 使用

**搜索已有知识：**
```bash
python3 scripts/concept_manager.py search --query "分发"
```

**添加新概念：**
```bash
python3 scripts/concept_manager.py add \
  --concept "分发能力 > 制作能力" \
  --source "HN 2026-03-27" \
  --context "当AI让创作成本趋近于零，稀缺资源转移到分发" \
  --tags "创业,分发,AI" \
  --confidence medium
```

**关联概念：**
```bash
python3 scripts/concept_manager.py link \
  --from "分发能力 > 制作能力" \
  --to "品味是最后的差异化" \
  --relation "supports"
```

**信念追踪：**
```bash
python3 scripts/concept_manager.py believe \
  --belief "Self-belief是做出来的不是喊出来的" \
  --reasoning "Musk的conviction是十几年失败中锤出来的"
```

**知识蒸馏（每周执行）：**
```bash
python3 scripts/concept_synthesis.py --days 7
```

**生成可视化：**
```bash
python3 scripts/visualize.py
# 输出 ~/Desktop/knowledge-graph.html
```

**Python API（供子 Agent 使用）：**
```python
from ke_api import ke

results = ke.search("创业", limit=5)
ke.concept("新洞察", source="文章", tags=["AI"])
beliefs = ke.beliefs(top=5)
ctx = ke.context("创业", recent=3, semantic=5)
```

## 核心特性

### 循环检测

添加概念时自动检查相似度：
- ≥0.6 自动合并（避免碎片化）
- ≥0.4 提醒警告（用户确认）

### 信念时间衰减

信念不会永远保持高置信度：
- 7 天内不变
- 7-14 天降一级
- 14-30 天再降
- 被挑战额外降一级
- 有近期更新保持或提升

### L1→L2→L3 自动蒸馏

```
≥3 个同标签 L1 → 自动生成 L2 洞察
≥2 个同标签 L2 → 自动生成 L3 元规律（需人工验证）
```

## 架构

```
skills/knowledge-engine/
├── SKILL.md                          ← OpenClaw skill 元数据 + 使用指南
├── README.md                         ← 你正在读的这个
├── LICENSE                           ← MIT
├── scripts/
│   ├── concept_manager.py            ← 概念/信念 CRUD + 搜索 + 图谱 (900+ 行)
│   ├── concept_synthesis.py          ← 蒸馏引擎：Reflection + L1→L2→L3
│   ├── ke_api.py                     ← Python API 单例，供子 Agent 使用
│   ├── eval_knowledge_engine.py      ← 六项评估套件
│   ├── visualize.py                  ← D3.js 交互式知识图谱生成器
│   └── viz-template.html             ← 可视化模板
└── resources/
    ├── concept-schema.md             ← 概念卡片 Schema
    └── belief-schema.md              ← 信念卡片 Schema
```

**零外部依赖** — 纯 Python 标准库（sqlite3 + json + os）。

## 数据存储

所有数据存在 `$OPENCLAW_WORKSPACE/memory/` 下：

```
memory/
├── concepts/          ← 概念卡片（JSON，每个概念一个文件）
├── beliefs/           ← 信念追踪（JSON）
├── insights/          ← 蒸馏报告（Markdown）
├── knowledge.db       ← SQLite 索引
└── reading-memory.md  ← 已读文章索引（防重复）
```

## 评估

```bash
python3 scripts/eval_knowledge_engine.py
```

六项测试：Storage / Retrieval / Association / Confidence / Synthesis / Pruning

当前通过率：88% ✅

## 路线图

- [x] 概念 CRUD + 搜索
- [x] 概念关联 + 知识图谱
- [x] 信念追踪 + 时间衰减
- [x] 循环检测（自动去重）
- [x] L1→L2→L3 自动蒸馏
- [x] 子 Agent API
- [x] 交互式可视化
- [ ] chromadb 向量搜索（语义匹配）
- [ ] 概念质量 LLM-as-Judge 评估
- [ ] 多 Agent 共享知识库接口

## 设计哲学

> **Harness（控制层）要为废弃而建，Knowledge（认知层）要为累积而建。**

这是从 Hermès Engineering 系列文章中提炼的核心洞察：

- 工程 Harness 会随模型进化被淘汰
- 知识系统越积累越有价值
- 今天的概念卡片，一年后是认知护城河

## 作者

**Atlas** — [洪玉麟](https://github.com/zabr1314) 的 AI 助手

## License

MIT
