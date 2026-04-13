# Counseling Case Report Skill - 个案报告生成器

一个 Claude Code Skill，帮助心理咨询师将个案咨询文稿自动转化为三维度深度分析报告。

## 核心能力

从一份个案咨询文稿（文字记录），自动生成三份专业报告：

| 报告 | 负责 Agent | 理论基础 | 用途 |
|------|-----------|---------|------|
| 01-案主版报告 | Agent A | 实用行动导向 | 可发送给案主 |
| 02-思想结构拆解 | Agent B | 维特根斯坦语言游戏 + DBS 五层拆解 | 内部督导使用 |
| 03-默会知识提取 | Agent C | Polanyi 默会知识理论 | 内部督导使用 |
| 00-审核报告 | Agent D | 原文溯源零容忍审核 | 质量保障 |

## 工作流程

```
Step 1:  扫描文稿 → Step 2:  确认基本信息
                        ↓
                 Step 2.5: 语义预分析与说话人确认
                        ↓
Step 3:  创建目录 → Step 3.5: 分析策略预告
                        ↓
Step 4:  启动4个Agent → Step 5: 监控进度 → Step 6: 收集报告
                        ↓
                 Step 6.5: 原文溯源审核（Agent D）
                        ↓
Step 7:  生成PDF → Step 8: 生成编码 → Step 9: 关闭团队 → Step 10: 汇报
```

## 快速开始

### 1. 安装 Skill

```bash
git clone https://github.com/SensationHeal/counseling-case-report-skill.git
cp counseling-case-report-skill/SKILL.md ~/.claude/commands/case-report.md
```

### 2. 在 Claude Code 中使用

```
/case-report
```

或直接说：

```
帮我生成个案报告
```

### 3. 前置条件

- Claude Code（需要 Agent Teams 功能）
- pandoc（`brew install pandoc`）— 用于 PDF 生成
- Python 3 + websockets（`pip3 install websockets`）— PDF 渲染

## 四个 Agent 详解

### Agent A - 案主报告撰写员
- 提炼核心价值，不做流水账
- 每个洞察配"你能做什么"的行动建议
- 通俗语言，避免心理学术语
- 包含本周必做、本月思考、下次咨询准备

### Agent B - 思想结构拆解员
- 维特根斯坦语言游戏理论：看同一个词在不同人口中是不是同一个意思
- DBS 五层拆解：字面 → 操作定义 → 真实指向 → 底层动力 → 消解
- 输出认知结构图谱和核心概念词典

### Agent C - 默会知识提取员
- 基于 Michael Polanyi："We can know more than we can tell."
- 四维挖掘：不可言说 / 身体化 / 情境依赖 / 关系性
- 挖掘防御机制、隐性恐惧、默会智慧

### Agent D - 原文溯源审核员
- 对三份报告的每一个核心论断逐一审核
- 精确定位原文行号和引用
- 零容忍推断：找不到依据一律标记"待修订"
- 输出验证通过/待修订/部分支撑的分类统计

## 输出示例

```
📁 文件位置：个案报告/案主名-日期/

📄 生成文件：
├─ 00-审核报告.md          (原文溯源验证)
├─ 01-案主版报告.md        (可发给案主)
├─ 01-案主版报告.pdf       (带品牌页眉/页脚)
├─ 02-思想结构拆解.md      (内部使用)
├─ 02-思想结构拆解.pdf
├─ 03-默会知识提取.md      (内部使用)
├─ 03-默会知识提取.pdf
└─ 归档清单.md
```

## 开发者

德领 | 心理咨询师 x AI Builder

## License

MIT
