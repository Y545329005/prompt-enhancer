# prompt-enhancer（提示词增强 Skill）

一个**跨工具可用**的 Agent Skill：把模糊需求一键改写为结构化、可审查、可执行的 prompt，让任何支持 SKILL.md 的 Agent（Codex / opencode / Claude Code / 其他 agent 兼容工具）都能按统一方式"先想清楚、再干活"。

> 设计原则：产物必须能**跨边界**使用（被用户审查 / 复制到其他工具 / 下次复用），因此**默认只产出文本，不执行**；用户明确说"直接做"才执行。

## 效果

输入：`帮我写个周报`

输出：一段结构化 prompt（角色 / 目标 / 输入与上下文 / 执行要点 / 输出格式 / 验收标准 / 明确不做什么），供你复制、编辑，或让 Agent 按此执行。

## 快速使用（安装后）

自然语言触发（推荐，两边都支持）：

- `优化下这个需求：帮我写个周报`
- `帮我增强 prompt：...`
- `把这条需求写清楚：...`

显式触发（100% 生效）：

- Codex：输入 `$prompt-enhancer`（`$` 唤起技能选择）
- opencode：输入 `/prompt-enhancer`
- Claude Code：直接提及技能名

行为约定（写在 SKILL.md 中，Agent 会遵守）：

1. 需求模糊（缺受众 / 交付物 / 约束 / 验收标准）→ **最多一轮追问**（≤3 问，每问附推荐默认值）；已够具体 → 零追问直接产出。
2. 默认模式**只产出文本**：不读文件、不调用工具、不执行原任务。
3. 用户说「直接做 / 按此执行」→ 才以增强后的定义执行。

## 安装

Skill 的本质是一个目录 + `SKILL.md`（YAML frontmatter 含 `name` / `description`）。以下路径均为对应工具的原生发现目录，**目录名必须为 `prompt-enhancer`**（与 frontmatter `name` 一致）：

| 工具 | 安装路径 |
|---|---|
| Codex | `~/.codex/skills/prompt-enhancer/` |
| opencode | `~/.config/opencode/skills/prompt-enhancer/` |
| Claude Code | `~/.claude/skills/prompt-enhancer/` |
| agent 兼容 | `~/.agents/skills/prompt-enhancer/` |

命令示例（软链方式，便于以后 `git pull` 更新）：

```bash
git clone https://github.com/Y545329005/prompt-enhancer.git ~/prompt-enhancer
mkdir -p ~/.codex/skills ~/.config/opencode/skills ~/.claude/skills
ln -s ~/prompt-enhancer ~/.codex/skills/prompt-enhancer
ln -s ~/prompt-enhancer ~/.config/opencode/skills/prompt-enhancer
ln -s ~/prompt-enhancer ~/.claude/skills/prompt-enhancer
```

> Codex 用户也可以直接在对话中说："用 skill-installer 从本仓库安装 prompt-enhancer"。

安装完成后**新开一个会话**即可使用（技能列表会刷新）。

## 仓库结构

```
prompt-enhancer/
├── SKILL.md     # 技能本体：完整工作流 + 「增强版 prompt」固定结构
└── README.md    # 本说明
```

## 方法论出处

输出字段结构来自通用 prompt engineering 最佳实践，与社区对 WorkBuddy「增强提示词」输出结构的实测描述一致；WorkBuddy 官方未公开其内部公式（截至 2026-09 官方文档无相关说明），本项目**不声称官方出处**。
