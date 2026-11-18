# Counseling Case Report Skill - 个案报告生成器

一个 Claude Code Skill，帮助心理咨询师将个案咨询文稿转化为三维度深度分析报告的草稿。

## 核心能力

从一份个案咨询文稿（文字记录），生成三份专业报告草稿，并由审核员做原文溯源。案主版在咨询师审阅前不得发送。

| 报告 | 负责队友 | 理论基础 | 用途 |
|------|-----------|---------|------|
| 01-案主版报告 | agent-a | 实用、通俗，不替咨询师发言 | 草稿，咨询师审阅前不得发送 |
| 02-思想结构拆解 | agent-b | 维特根斯坦语言游戏 + DBS 五层拆解 | 内部督导，不得发送给案主 |
| 03-默会知识提取 | agent-c | Polanyi 默会知识理论 | 内部督导，不得发送给案主 |
| 00-审核报告 | agent-d | 原文溯源零容忍审核 | 质量保障；未通过时不生成案主版 PDF |

## 工作流程

```
Step 1:  扫描文稿 → Step 2:  确认基本信息
                        ↓
                 Step 2.5: 语义预分析与说话人确认
                        ↓
Step 3:  创建目录 → Step 3.5: 分析策略预告
                        ↓
                 Step 3.7: 临床安全确认（强制）
                        ↓
Step 4:  启动队友 A/B/C → Step 5: 监控进度 → Step 6: 收集报告
                        ↓
                 Step 6.5: 原文溯源审核（agent-d）
                        ↓
Step 7:  按审核结果生成 PDF → Step 8: 追加编码 → Step 9: 关闭队友 → Step 10: 汇报
```

## 快速开始

### 1. 安装 Skill

```bash
git clone https://github.com/SensationHeal/counseling-case-report-skill.git
mkdir -p ~/.claude/skills/case-report
cp counseling-case-report-skill/SKILL.md ~/.claude/skills/case-report/SKILL.md
```

安装后命令名是 `/case-report`。本技能设置了 `disable-model-invocation: true`，不会在闲聊时自动加载，也不会自动扫描文稿目录。

### 2. 在 Claude Code 中使用

```
/case-report
```

### 3. 前置条件

- Claude Code（交互式会话）
- 启用 Agent Teams（实验功能，默认关闭）。在环境变量或 `~/.claude/settings.json` 中设置：

```bash
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1
```

```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

一个会话只有一个团队。队友用 Agent 工具按 `agent-a` 到 `agent-d` 启动；`TaskCreate` 只登记共享任务，不负责启动队友。

- pandoc（`brew install pandoc`）— 用于 PDF 生成
- Python 3 + websockets（`pip3 install websockets`）— PDF 渲染
- 本机已有文稿目录、编码台账，以及 `md_to_pdf.py`（见 `SKILL.md` 中的路径）。这些文件不在本仓库内

## 四个队友详解

### agent-a - 案主报告撰写员
- 提炼核心价值，不做流水账
- 只写文稿里已经出现的发现，不替咨询师向案主说话
- 通俗语言，避免心理学术语
- 不布置「本周必做」一类作业

### agent-b - 思想结构拆解员
- 维特根斯坦语言游戏理论：看同一个词在不同人口中是不是同一个意思
- DBS 五层拆解：字面 → 操作定义 → 真实指向 → 底层动力 → 消解
- 输出认知结构图谱和核心概念词典

### agent-c - 默会知识提取员
- 基于 Michael Polanyi："We can know more than we can tell."
- 四维挖掘：不可言说 / 身体化 / 情境依赖 / 关系性
- 挖掘防御机制、隐性恐惧、默会智慧

### agent-d - 原文溯源审核员
- 对三份报告的每一个核心论断逐一审核
- 精确定位原文行号和引用
- 零容忍推断：找不到依据一律标记「待修订」
- 存在「待修订」或「部分支撑」时，不生成案主版 PDF

## 输出示例

```
📁 文件位置：个案报告/虚构案主甲-2000.1.1-DL-CASE-2000-001/

📄 生成文件：
├─ 00-审核报告.md          (原文溯源验证)
├─ 01-案主版报告.md        (草稿，咨询师审阅前不得发送)
├─ 01-案主版报告.pdf       (仅审核全部通过时生成)
├─ 02-思想结构拆解.md      (内部督导，不得发送给案主)
├─ 02-思想结构拆解.pdf
├─ 03-默会知识提取.md      (内部督导，不得发送给案主)
├─ 03-默会知识提取.pdf
└─ 归档清单.md
```

上面的案主名、日期和编码都是虚构占位，不是真实个案。

## 开发者

德领 | 心理咨询师 x AI Builder

## License

MIT
