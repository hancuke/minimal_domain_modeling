# Minimal Domain Modeling

把业务需求转化为最简充分领域模型的可移植 Agent Skill。也适用于评估、简化已有模型的概念边界、职责和生命周期，不代替技术架构或实现设计。

## Skills 与职责

| Skill | 适用任务 | 职责边界 |
|---|---|---|
| `communication-policy` | 组织、改写或审阅回答、设计、模型与研究报告 | 根据读者目标和理解依赖组织信息；不替代专业判断、交付要求或执行门槛 |
| `minimal-domain-modeling` | 明确业务含义，建立领域模型，或评估、简化已有业务边界 | 输出简洁的系统级业务模型：概念与职责、协作与生命周期、业务不变量；不负责软件结构或实现 |
| `code-structure-review` | 独立评估现有代码的职责边界、调用者负担、耦合和修改局部性 | 提供代码证据、问题优先级和改进方向；不预设重构手段，不定稿目标设计，不修改代码 |
| `software-design` | 根据领域模型、明确需求，或现有实现与优化目标设计软件 | 必要时诊断现状，设计代码对象、模块、接口和迁移验证方案；仅在要求代码变更且方案获批后交给 `implement` |
| `implement` | 实施获批的软件设计，或结构与行为已明确的局部代码变更 | 在既定职责和契约内安排文件与函数，完成实现、行为验证与边界验收，不擅自改变设计 |
| `ui-design` | 设计界面、评审渲染结果，或实现页面、组件和样式 | 保留业务行为和数据契约；设计与评审不修改代码，实现需检查渲染及相关交互 |

按任务选择所需 Skill，不要求每次依次运行全部。需要决定业务含义时进入领域建模，代码也可作为业务证据；独立评估现有代码结构进入 `code-structure-review`；已有明确行为但需要设计结构的技术改动进入 `software-design`，既有结构与契约内的局部变更可直接进入 `implement`，无需先生成领域模型文档或运行结构评审。混合任务先确认受影响的业务规则，再设计实现。

`communication-policy` 横向约束沟通方式，任务 Skill 定义专业内容、流程和验证要求。它不强制统一大纲，也不是其他 Skill 独立安装的必需依赖；持续启用由目标 Agent 的常驻指令负责，见下方启用说明。

领域模型是业务语义契约，不是类或模块蓝图。`software-design` 可以重新划分代码边界，但不能擅自改变已确认的业务含义；遇到歧义时只交回受影响的业务问题，不默认重做整个领域模型。

`code-structure-review` 负责独立判断“现有结构有什么问题，是否值得解决”，不以文件大小或相似语法直接判定结构好坏。集中知识、拆分职责、移动归属、简化接口或保留现状都是可能结果；module deepening 是减少调用者知识负担的一种手段，不是评审的预设方向。评审提供问题证据、影响场景、收益、风险和成本，不要求完整接口、文件树或迁移方案。

`software-design` 负责“目标软件结构怎样设计”，确定职责归属、接口契约、状态与副作用所有权、依赖方向以及迁移验证方案，也能按设计目标诊断现有实现，不强制先运行独立评审。用户要求落实已有诊断建议时，复用选定候选、证据、约束和验证需求，只检查设计相关假设。设计请求以方案结束；需要设计的实施请求先提交方案等待明确批准，批准后交给 `implement` 修改代码。只有物理布局影响重要设计边界时才展开文件布局，不强制完整文件树。

`implement` 负责“已确认的设计如何落到可定位、可验证的代码”。实施前将既定职责映射到文件和函数，复用已有方案；明显的局部修复不强制单独输出 Code Placement Map。实施后同时检查行为与边界，避免业务操作混入文件读写、编码和格式解析细节。按变化原因分离，不机械追求文件数量、固定分层或一函数一文件。私有辅助函数、内部命名、局部算法及既定职责内的文件落位可自行调整；需要实质改变职责、模块边界、对外契约、依赖方向、失败语义或行为时，暂停受影响部分，交回 `software-design` 并等待批准，不边写边改架构。
 

## 文件与安装边界

```text
minimal_domain_modeling/
├── README.md
├── skills/
│   ├── communication-policy/
│   │   └── SKILL.md
│   ├── minimal-domain-modeling/
│   │   ├── SKILL.md
│   │   ├── references/
│   │   │   └── information-and-retention.md
│   │   └── assets/
│   │       └── model-template.md
│   ├── code-structure-review/
│   │   ├── SKILL.md
│   │   └── references/
│   │       ├── design-principles.md
│   │       ├── dependencies-and-verification.md
│   │       └── alternatives.md
│   ├── software-design/
│   │   └── SKILL.md
│   ├── implement/
│   │   └── SKILL.md
│   └── ui-design/
│       └── SKILL.md
└── evaluations/
    ├── cases.json
    └── design-cases.json
```

`skills/` 下每个子目录定义一个 Skill。Skill 可以协作或共享参考材料；存在必需依赖时，应说明依赖对象与读取条件，安装时确保所需 Skill 和参考文件可用。根目录的 README 和评估场景面向维护者，不属于运行时 Skill，也不需要加载到 Agent 上下文。

`minimal-domain-modeling` 正文负责分析、澄清和场景验证；保留要求的专项判断放在 reference 中；template 统一为问题与目标、概念与职责、生命周期与协作、业务不变量四部分。文档描述选定业务范围内的完整模型，不包含本次任务范围、变更清单或验证过程；已有模型原位更新，每条事实只写一次。信息保留义务融入相关职责、生命周期或不变量，未知事实就地标注，不补造全局规则。其文件引用均相对 Skill 目录解析，不依赖此仓库的绝对路径、脚本或网络。

通用表达规则仅维护在 `communication-policy/SKILL.md`，不再单独维护 `Agent Writing Standard.md`。各任务正文保留专业原则、示例、流程和交付要求，不强制引用 Communication Policy。共享术语与架构约束在 `code-structure-review/references/design-principles.md`；depth 是评价维度之一，不凌驾于职责内聚、独立变化和迁移成本之上。依赖语义、seam 放置和测试迁移细则在 `dependencies-and-verification.md`；方案比较方法在 `alternatives.md`，评审阶段比较改进方向，设计阶段展开目标方案。

`code-structure-review` 评估候选前读取共享参考。`software-design` 仅在模块边界、依赖隔离或重要架构取舍时读取它；简单局部改动不加载参考。依赖替代、seam 调整或测试迁移时读取 `dependencies-and-verification.md`；有实质性方案差异或明确要求替代方案时读取 `alternatives.md`。共享参考继续随评审 Skill 分发，不另建 Skill；读取参考不启动独立评审流程。

`software-design` 获批后需要实施时加载 `implement`，交接既有方案而不重复生成文档。`implement` 正文本身不依赖其他 Skill 的参考文件；实施中需要重新设计时才交回 `software-design`。缺少所需 Skill 时明确报告，不将未执行的流程视为已完成。

## 在另一台机器安装

### 从 GitHub 安装（推荐）

目标机器需要 Node.js、npm 和 Git。在目标业务项目根目录执行：

```sh
# 查看仓库中可安装的 Skills，不执行安装
npx skills add hancuke/minimal_domain_modeling --list

# 安装领域建模 Skill，交互选择目标 Agent
npx skills add hancuke/minimal_domain_modeling --skill minimal-domain-modeling
```

默认安装到当前项目。安装到 Claude Code 个人目录，供所有项目使用：

```sh
npx skills add hancuke/minimal_domain_modeling \
  --skill minimal-domain-modeling \
  --agent claude-code \
  --global
```

将 `--skill` 的值替换为 `communication-policy`、`code-structure-review`、`implement` 或 `ui-design` 可安装其他 Skill。安装 `software-design` 时也安装其参考依赖和实施交接 Skill：

```sh
npx skills add hancuke/minimal_domain_modeling --skill code-structure-review
npx skills add hancuke/minimal_domain_modeling --skill software-design
npx skills add hancuke/minimal_domain_modeling --skill implement
```

三次安装选择相同 Agent 和安装范围，使三个目录处于同一 Skill 父目录。不要假设安装工具会自动解析跨 Skill 依赖；安装后检查 `software-design/SKILL.md` 中的三个相对引用均可读取，并确认 Agent 能找到 `implement`。缺失依赖应明确报告，不能把未加载的规则视为已应用。

`npm install skills` 只安装 Skills CLI，不会安装本仓库的 Skill；无需将本仓库发布到 npm。

GitHub 安装读取已推送的文件；私有仓库需要目标机器配置有访问权限的 Git 凭据。更新已有安装前先比较版本与本地修改，避免覆盖。

安装方式参考 [Skills CLI 文档](https://github.com/vercel-labs/skills/blob/main/docs/commands.md)。

### 安装后启用 Communication Policy

在目标项目根目录执行项目级安装：

```sh
npx skills add hancuke/minimal_domain_modeling \
  --skill communication-policy
```

个人级安装可供多个项目使用：

```sh
npx skills add hancuke/minimal_domain_modeling \
  --skill communication-policy --global
```

安装时选择目标 Agent。安装不等于启用，全局安装也不保证每次任务自动加载。偶尔使用时，按目标 Agent 的调用方式加载；需要持续生效时，将以下示例合并到目标项目的 `AGENTS.md`，前提是该 Agent 支持并会读取它：

```markdown
## Communication Policy

Before composing user-facing responses or documents, load and apply the
installed communication-policy skill if it is not already loaded in this
session. Reload it if its content changes.

Apply it alongside task-specific skills. Preserve required content, formats,
verification requirements, and approval gates. Do not impose one universal outline.

If the skill is unavailable, report the missing dependency and do not claim
that the policy has been applied.
```

此示例仅用于安装后的启用配置，本仓库不创建实际的 `AGENTS.md`。安装命令不会替你写入上述常驻指令。其他 Agent 应将入口放在其支持的常驻指令文件中，并根据实际安装位置配置可读取的 Skill 路径；个人级持续启用也需要相应的个人级指令。不要依赖非标准的 `alwaysApply` 字段承诺跨 Agent 生效。

配置后在新会话中确认 Agent 能找到并读取 Policy，同时仍遵守任务 Skill 的交付要求；不能仅凭安装成功就认为规范已经生效。

### 手动复制

将本项目复制到目标机器，并在本仓库根目录执行以下个人安装命令。目标目录已存在时先比较差异，避免覆盖当地修改。

Claude Code 个人安装：

```sh
mkdir -p "$HOME/.claude/skills"
test ! -e "$HOME/.claude/skills/minimal-domain-modeling" &&
  cp -R skills/minimal-domain-modeling "$HOME/.claude/skills/"
```

安装技术设计、实施 Skill 及共享参考，使用同一父目录：

```sh
mkdir -p "$HOME/.claude/skills"
test ! -e "$HOME/.claude/skills/code-structure-review" &&
  test ! -e "$HOME/.claude/skills/software-design" &&
  test ! -e "$HOME/.claude/skills/implement" &&
  cp -R skills/code-structure-review skills/software-design skills/implement "$HOME/.claude/skills/"
```

Claude Code 项目安装，在目标业务项目根目录执行；将源路径替换为本项目的实际路径：

```sh
mkdir -p .claude/skills
test ! -e .claude/skills/minimal-domain-modeling &&
  cp -R /path/to/minimal_domain_modeling/skills/minimal-domain-modeling .claude/skills/
```

项目级安装技术设计与实施 Skill 时，同样将三个完整目录放在 `.claude/skills/` 下。
以上命令在已有同名目录时停止复制；它们不是更新命令。
启动新会话后，用 `/minimal-domain-modeling` 显式调用，也可通过相关建模请求触发。
结构评审入口统一为 `code-structure-review`，替代旧名 `module-deepening`（更早为 `codebase-design`）。更新旧安装时先核对本地修改，确认新入口及共享参考可用，再移除旧入口，避免同时发现多套自动触发规则。旧 `references/deepening.md` 已改名为 `references/dependencies-and-verification.md`，同步更新本地自定义引用；不保留旧名兼容入口。
软件设计入口由 `implementation-design` 改名为 `software-design`；更新时先确认新入口及共享参考可用，再移除旧安装，不保留两个自动触发入口。
其他支持 Agent Skills 的工具可复制同一目录到其规定的 Skill 搜索路径；安装位置和显式调用语法以目标工具文档为准。

安装方式参考 [Claude Code Skills 文档](https://code.claude.com/docs/en/skills)。
本包只使用标准 `name` 和 `description` 元数据，不使用平台专属执行扩展。
 
## 编写依据

- [Anthropic Skill authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)
- [mgechev/skills-best-practices](https://github.com/mgechev/skills-best-practices)

正文保留英文以便跨工具使用；此文档面向中文维护者。仓库级说明与运行时材料分离，避免把人类安装文档混入 Skill。


## reference
- https://github.com/wonsukchoi/domain-experts/blob/main/roles/financial-analyst/
- https://www.multicians.org/thvv/proverbs.html
- https://www.thoughtworks.com/en-us/insights/blog/agile-project-management/domain-modeling-what-you-need-to-know-before-coding
- https://www.cosmicpython.com/book/part1
- https://www.cs.unc.edu/~stotts/COMP590-059-f24/robsrules.html
- https://mcpmarket.com/zh/tools/skills/rob-pike-code-philosophy
- https://ubos.tech/news/rob-pikes-five-programming-rules-timeless-guidance-for-modern-developers/
