# Minimal Domain Modeling

把业务需求转化为最简充分领域模型的可移植 Agent Skill。也适用于评估、简化已有模型的概念边界、职责和生命周期，不代替技术架构或实现设计。

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
│   ├── shape/
│   │   └── SKILL.md
│   └── uidesign/
│       └── SKILL.md
└── evaluations/
    └── cases.json
```

`skills/` 下每个子目录是独立的 Skill，安装时复制所选 Skill 的整个目录，而不是整个仓库。根目录的 README 和评估场景面向维护者，不属于运行时 Skill，也不需要加载到 Agent 上下文。

`minimal-domain-modeling` 正文负责流程、澄清门槛和完成标准；保留要求的专项判断放在 reference 中；完整文档的展示结构放在 template 中。引用均相对 Skill 目录解析，不依赖此仓库的绝对路径、脚本、网络或其他 Skill。

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

将 `--skill` 的值替换为 `shape` 或 `uidesign` 可安装其他 Skill。`npm install skills` 只安装 Skills CLI，不会安装本仓库的 Skill；无需将本仓库发布到 npm。

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

Claude Code 项目安装，在目标业务项目根目录执行；将源路径替换为本项目的实际路径：

```sh
mkdir -p .claude/skills
test ! -e .claude/skills/minimal-domain-modeling &&
  cp -R /path/to/minimal_domain_modeling/skills/minimal-domain-modeling .claude/skills/
```

以上命令在已有同名目录时停止复制；它们不是更新命令。
启动新会话后，用 `/minimal-domain-modeling` 显式调用，也可通过相关建模请求触发。
其他支持 Agent Skills 的工具可复制同一目录到其规定的 Skill 搜索路径；安装位置和显式调用语法以目标工具文档为准。

安装方式参考 [Claude Code Skills 文档](https://code.claude.com/docs/en/skills)。
本包只使用标准 `name` 和 `description` 元数据，不使用平台专属执行扩展。

## 维护规则

- 每个 Skill 放在 `skills/<name>/`，其 `SKILL.md` 包含必需的 `name` 和 `description`，名称唯一且与目录名一致。
- 以下正文、reference、template 和评估规则针对 `minimal-domain-modeling`。
- 修改行为时，先确定规则的唯一归属：执行与完成标准在正文，信息保留判断在 reference，展示结构在 template。
- 正文只直接引用上述两个文件，并说明读取条件。新增参考文件应对应实际分支，不为目录完整性拆文件。
- 共用的必要规则留在正文；仅部分任务需要的细节按需加载。避免让 Agent 为一次简单建模读取整套材料。
- 业务示例用于说明判断，不作为未经确认的业务默认值。
- 新增或修复行为时，更新 `evaluations/cases.json` 中对应的场景，避免重复测试相同分支。
- 发布时复制整个 Skill 目录，检查引用文件随包交付。更新安装前比较版本与本地修改，不直接覆盖。

## 回归验证

`evaluations/cases.json` 是可重复使用的评估输入与验收标准，不是已通过的测试报告，也不要求特定评估框架。

1. **触发测试：** 在新会话中只提供 `SKILL.md` 的元数据和一个 discovery 请求，判断是否应该加载；与 `should_trigger` 比较。再在目标工具实际安装后检查是否加载了 Skill。
2. **执行测试：** 为每个 execution 场景开启独立会话，加载 Skill 并发送请求。逐项检查 `checks`；需要澄清的场景先观察提问，不提供答案时再检查暂定结果。
3. **披露测试：** 检查文件读取记录。信息保留任务应读取 reference；明确要求完整模型文档时应读取 template；简短建模不应默认读取 template。
4. **效果比较：** 用相同请求比较无 Skill、上一版本与当前版本的可观察产物，包括误触发、编造规则、遗漏场景和不必要复杂度。涉及模型或工具更换时重跑。

记录实际使用的模型、工具版本、Skill 版本、通过项及失败证据。只检查可观察的调用与输出，不依赖模型内部思考过程。

建议初次发布至少运行全部场景；局部修改先运行相关场景，发布前再运行全部。静态结构检查不能代替实际 Agent 评估。

## 编写依据

- [Anthropic Skill authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)
- [mgechev/skills-best-practices](https://github.com/mgechev/skills-best-practices)

正文保留英文以便跨工具使用；此文档面向中文维护者。仓库级说明与运行时材料分离，避免把人类安装文档混入 Skill。
