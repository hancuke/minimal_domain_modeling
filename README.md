# Minimal Domain Modeling

把业务需求转化为最简充分领域模型的可移植 Agent Skill。也适用于评估、简化已有模型的概念边界、职责和生命周期，不代替技术架构或实现设计。

## Skills 与职责

| Skill | 适用任务 | 职责边界 |
|---|---|---|
| `minimal-domain-modeling` | 从业务需求建立领域模型，或评估、简化已有业务边界 | 概念、职责、规则、关系、生命周期和信息保留要求；不负责技术架构或 UI 设计 |
| `module-deepening` | 独立诊断现有代码的重复知识、调用者负担、修改牵连和深化机会 | 提供代码证据、候选优先级和接口方向；允许结论“不重构”，不默认实施 |
| `implementation-design` | 设计项目架构、规划功能实现，或落地选定的技术变更 | 基于明确目标设计接口、依赖、状态、数据流、失败处理和交付路径；仅在要求代码变更时实施验证 |
| `ui-design` | 设计界面、评审渲染结果，或实现页面、组件和样式 | 保留业务行为和数据契约；设计与评审不修改代码，实现需检查渲染及相关交互 |

按任务选择所需 Skill，不要求每次依次运行全部。业务规则未明确时先澄清或建模；技术设计与 UI 设计使用已确定的业务行为作为输入。新功能可直接使用 `implementation-design`，无需先运行深化诊断。

`module-deepening` 负责“现有复杂度在哪里，是否值得集中”，`implementation-design` 负责“明确目标怎样实现并安全交付”。用户要求落实诊断建议时，前者交接选定候选、证据、约束和验证需求，后者核对关键假设，不重复整个诊断。两个流程共享判断依据，不共享默认执行流程。
 

## 文件与安装边界

```text
minimal_domain_modeling/
├── README.md
├── skills/
│   ├── minimal-domain-modeling/
│   │   ├── SKILL.md
│   │   ├── references/
│   │   │   └── information-and-retention.md
│   │   └── assets/
│   │       └── model-template.md
│   ├── module-deepening/
│   │   ├── SKILL.md
│   │   └── references/
│   │       ├── design-principles.md
│   │       ├── deepening.md
│   │       └── alternatives.md
│   ├── implementation-design/
│   │   └── SKILL.md
│   └── ui-design/
│       └── SKILL.md
└── evaluations/
    ├── cases.json
    └── design-cases.json
```

`skills/` 下每个子目录定义一个 Skill。Skill 可以协作或共享参考材料；存在必需依赖时，应说明依赖对象与读取条件，安装时确保所需 Skill 和参考文件可用。根目录的 README 和评估场景面向维护者，不属于运行时 Skill，也不需要加载到 Agent 上下文。

`minimal-domain-modeling` 正文负责流程、澄清门槛和完成标准；保留要求的专项判断放在 reference 中；完整文档的展示结构放在 template 中。其文件引用均相对 Skill 目录解析，不依赖此仓库的绝对路径、脚本或网络。

各正文保留任务特有原则、示例和流程。共享术语与架构约束在 `module-deepening/references/design-principles.md`；依赖语义、seam 放置和测试迁移细则在 `deepening.md`；方案比较方法在 `alternatives.md`。

`module-deepening` 评估候选前读取共享参考。`implementation-design` 仅在模块边界、依赖隔离或重要架构取舍时读取它；简单局部改动不加载参考。依赖替代、seam 调整或测试迁移时读取 `deepening.md`；有实质性方案差异或明确要求替代方案时读取 `alternatives.md`。读取参考不启动诊断流程。

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

将 `--skill` 的值替换为 `module-deepening` 或 `ui-design` 可安装其他 Skill。安装 `implementation-design` 时也安装其参考依赖：

```sh
npx skills add hancuke/minimal_domain_modeling --skill module-deepening
npx skills add hancuke/minimal_domain_modeling --skill implementation-design
```

两次安装选择相同 Agent 和安装范围，使两个目录处于同一 Skill 父目录。不要假设安装工具会自动解析跨 Skill 文件依赖；安装后检查 `implementation-design/SKILL.md` 中的三个相对引用均可读取。缺失依赖应明确报告，不能把未加载的规则视为已应用。

`npm install skills` 只安装 Skills CLI，不会安装本仓库的 Skill；无需将本仓库发布到 npm。

GitHub 安装读取已推送的文件；私有仓库需要目标机器配置有访问权限的 Git 凭据。更新已有安装前先比较版本与本地修改，避免覆盖。

安装方式参考 [Skills CLI 文档](https://github.com/vercel-labs/skills/blob/main/docs/commands.md)。

### 手动复制

将本项目复制到目标机器，并在本仓库根目录执行以下个人安装命令。目标目录已存在时先比较差异，避免覆盖当地修改。

Claude Code 个人安装：

```sh
mkdir -p "$HOME/.claude/skills"
test ! -e "$HOME/.claude/skills/minimal-domain-modeling" &&
  cp -R skills/minimal-domain-modeling "$HOME/.claude/skills/"
```

安装两个技术设计 Skill 及共享参考，使用同一父目录：

```sh
mkdir -p "$HOME/.claude/skills"
test ! -e "$HOME/.claude/skills/module-deepening" &&
  test ! -e "$HOME/.claude/skills/implementation-design" &&
  cp -R skills/module-deepening skills/implementation-design "$HOME/.claude/skills/"
```

Claude Code 项目安装，在目标业务项目根目录执行；将源路径替换为本项目的实际路径：

```sh
mkdir -p .claude/skills
test ! -e .claude/skills/minimal-domain-modeling &&
  cp -R /path/to/minimal_domain_modeling/skills/minimal-domain-modeling .claude/skills/
```

项目级安装技术设计 Skill 时，同样将两个完整目录放在 `.claude/skills/` 下。
以上命令在已有同名目录时停止复制；它们不是更新命令。
启动新会话后，用 `/minimal-domain-modeling` 显式调用，也可通过相关建模请求触发。
深化诊断入口由 `codebase-design` 改名为 `module-deepening`；更新旧安装时核对本地修改，确认新目录可用后再移除旧入口，避免同时发现两套规则。
其他支持 Agent Skills 的工具可复制同一目录到其规定的 Skill 搜索路径；安装位置和显式调用语法以目标工具文档为准。

安装方式参考 [Claude Code Skills 文档](https://code.claude.com/docs/en/skills)。
本包只使用标准 `name` 和 `description` 元数据，不使用平台专属执行扩展。

## 维护规则

- 每个 Skill 放在 `skills/<name>/`，其 `SKILL.md` 包含必需的 `name` 和 `description`，名称唯一且与目录名一致。
- 每条规则有一个权威归属；正文用任务特有原则和示例指导应用，reference 提供定义及分支细则。修改技术设计或 UI 行为时，更新 `evaluations/design-cases.json`。
- 发布时检查 Skill 文件、引用材料及声明的必需依赖在安装后可用。更新安装前比较版本与本地修改，不直接覆盖。
- 以下正文、reference、template 和评估规则针对 `minimal-domain-modeling`。
- 修改行为时，先确定规则的唯一归属：执行与完成标准在正文，信息保留判断在 reference，展示结构在 template。
- 正文只直接引用上述两个文件，并说明读取条件。新增参考文件应对应实际分支，不为目录完整性拆文件。
- 共用的必要规则留在正文；仅部分任务需要的细节按需加载。避免让 Agent 为一次简单建模读取整套材料。
- 业务示例用于说明判断，不作为未经确认的业务默认值。
- 新增或修复行为时，更新 `evaluations/cases.json` 中对应的场景，避免重复测试相同分支。

## 回归验证

`evaluations/cases.json` 是可重复使用的评估输入与验收标准，不是已通过的测试报告，也不要求特定评估框架。

1. **触发测试：** 在新会话中只提供 `SKILL.md` 的元数据和一个 discovery 请求，判断是否应该加载；与 `should_trigger` 比较。再在目标工具实际安装后检查是否加载了 Skill。
2. **执行测试：** 为每个 execution 场景开启独立会话，加载 Skill 并发送请求。逐项检查 `checks`；需要澄清的场景先观察提问，不提供答案时再检查暂定结果。
3. **披露测试：** 检查文件读取记录。信息保留任务应读取 reference；明确要求完整模型文档时应读取 template；简短建模不应默认读取 template。
4. **效果比较：** 用相同请求比较无 Skill、上一版本与当前版本的可观察产物，包括误触发、编造规则、遗漏场景和不必要复杂度。涉及模型或工具更换时重跑。

记录实际使用的模型、工具版本、Skill 版本、通过项及失败证据。只检查可观察的调用与输出，不依赖模型内部思考过程。

建议初次发布至少运行全部场景；局部修改先运行相关场景，发布前再运行全部。静态结构检查不能代替实际 Agent 评估。

### 技术设计与 UI 评估

`evaluations/design-cases.json` 同样是输入与验收标准，不是已通过的报告：

- `discovery` 只加载四个入口的元数据，将实际选择与逐 Skill 的 `should_trigger` 比较，不预先透露期望值。组合请求检查所需入口，不要求一次执行全部流程。
- `execution` 每个场景使用独立会话，加载指定 `skill` 并执行 `prompt`。代码片段中的路径是场景输入，不代表当前仓库文件；记录回答与实际工具行为，逐项核对 `checks`。
- `disclosure` 检查读取记录与缺失依赖处理。简单局部改动不加载设计参考；架构分支按条件读取，且不触发诊断流程。
- UI 评估使用一次性 HTML 副本和实际截图；设计、评审场景检查未修改文件。无渲染工具的场景检查是否明确区分代码风险与已观察问题。缺少截图时，渲染评审场景标记未运行。
- 安装验证在临时目录放置两个完整 Skill 目录，检查所有相对引用可达；目标工具实际安装后的发现和读取仍需另行验证。

分别记录静态检查、实际执行的场景、失败证据与未运行项。新旧版本效果比较以及目标工具安装验证不能由静态检查或少量代表性场景代替。

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
