# prompt-enhancer（意图编译器 Skill）

一个**跨工具可用**的 Agent Skill：把用户的自然语言需求或已有 Prompt 编译为**意图保真、低歧义、可执行的最小充分任务指令**，让任何支持 SKILL.md 的 Agent（Codex / opencode / Claude Code / 其他 agent 兼容工具）都能按统一方式"先想清楚、再干活"。

> 设计原则：产物必须能**跨边界**使用（被用户审查 / 复制到其他工具 / 下次复用），因此**默认只产出文本，不执行**；用户明确说"直接做"才执行。

## 效果

输入：`帮我写个周报`

输出：一份增强后的 prompt——按复杂度自适应：简单需求直接润色、一屏可读；所有代填与默认显式标注（`【 】` 或「默认：X，可改」），不把假设冒充用户要求；复杂任务才按需启用 目标 / 输入与待提供项 / 执行要点 / 输出格式 / 验收标准 / 明确不做什么 等字段。

## 快速使用（安装后）

自然语言触发：

- `优化下这个需求：帮我写个周报`
- `帮我增强 / 润色 prompt：...`
- `把这条需求写清楚 / 说清楚：...`
- `帮我写 Prompt` / `这个 Prompt 怎么改`

显式触发（100% 生效）：

- Codex：输入 `$prompt-enhancer`（`$` 唤起技能选择）
- opencode：输入 `/prompt-enhancer`
- Claude Code：直接提及技能名

行为约定（写在 SKILL.md 中，Agent 会遵守）：

1. **意图保真**：保留用户原句关键表述与全部显式约束，不新增未授权目标；已有 Prompt 一律"修复"而非重写。
2. **歧义判据**：只有"未知信息取值不同会导致明显不同最优结果"才追问（最多 1 轮、≤3 问、每题带推荐默认值）；能安全假设就代填并标注「默认：X，可改」；金额 / 编号 / 日期等数据类未知量用 `【 】` 占位而非追问。
3. **复杂度自适应**：简单=直接润色少改；中等=补关键上下文 / 约束 / 输出要求；复杂=明确范围、执行方式、决策边界与完成标准。
4. **默认只产出文本**：不读文件、不调用工具、不执行原任务；仅当用户明确要求读取 / 结合某个文件或上下文时才读，读取前一句话说明。
5. 输出前自检 3 问：有没有改原意 / 有没有代填没标注 / 有没有删掉也不影响结果的废话。

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
├── SKILL.md                  # 技能本体：决策规则 + 输出规则 + 执行边界 + 自检
├── examples/regression.md    # 5 个典型输入的回归清单（改版回归与迭代输入集）
└── README.md                 # 本说明
```

## 版本说明

v1：固定 7 段输出结构的"需求增强"模板。
v2（当前）：改为"意图编译器"——以意图保真、假设透明、复杂度自适应为判断规则。判断规则经对抗审查与三方盲测（模板版 / 原则版 / v2）验证，v2 在 5 个典型输入上被盲审选为基线。本项目不声称任何外部官方出处。
