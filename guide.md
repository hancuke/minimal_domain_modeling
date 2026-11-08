
# Principle-driven Skill Design 

从一段复杂的知识、经验或讨论中，提炼出最少但足够指导行为的 Insights、Principles 和 Examples。它不是追求最短的文档，而是追求最高的信息密度和决策价值。

# skill 类型
Principle 擅长指导判断，但不一定擅长指导执行。

考虑三个不同类型的 Skill：

| Skill 类型 | 最重要的描述方式                              |
| -------- | ------------------------------------- |
| 思维方法型    | Insight + Principle + Examples        |
| 设计规范型    | Principle + Good / Avoid              |
| 工作流程型    | Steps + Decision Rules + Verification |


## 一种更精简的 Skill 描述模型

设计成三个层次：

Layer 1 · Understanding

Insight

解释这个 Skill 背后的核心认知：为什么需要它？它试图解决什么根本问题？

Layer 2 · Judgment

Principles

定义 Agent 的判断标准：什么是好的？什么是不好的？遇到冲突时优先考虑什么？

Layer 3 · Execution

Examples / Workflow

用正反例解释原则如何落地；如果是程序性任务，则定义必要步骤、决策分支和验证条件。

这三个层次有一个重要的逻辑关系：

- Insight 提供理解。
- Principle 提供判断。
- Example / Workflow 提供行为约束。

不是每个 Skill 都需要完整的三个层次。例如，某些 Skill 可能只需要一条 Insight 和三条 Principles。


| 场景              | 最合适的表达       |
| --------------- | ------------ |
| Agent 容易误解问题    | Insight      |
| Agent 容易做出错误选择  | Principle    |
| Agent 不理解原则如何应用 | Examples     |
| Agent 容易遗漏步骤    | Workflow     |
| Agent 容易违反边界    | Constraints  |
| Agent 必须保证结果正确  | Verification |

五条设计准则：

1. Insight over Introduction：不要花篇幅介绍背景，直接表达核心认知。
2. Principles over Generic Instructions：用判断原则替代空泛的行为要求。
3. Examples over Repetition：用正反例展示原则，而不是反复解释原则。
4. Workflow when Necessary：只有需要可重复执行的任务才定义详细流程。
5. Evidence over Completeness：通过实际任务测试 Skill，而不是通过文档长度判断质量。


每条 Principle 必须能够指导一个实际决策。简短不等于清晰，抽象也不等于高级。

如果一个 Principle 需要 Agent 自己猜测具体含义，它就可能成为一个新的模糊指令。好的 Example 应该消除这种歧义，而不是仅仅装饰文档。

不够好的 Principle：
> Write clean and maintainable code.

更好的 Principle：
> Prefer explicit domain types over generic dictionaries when the data has stable business meaning.

前者几乎没有决策价值，后者可以直接影响 Agent 的代码设计。


<template_schema>

---
name: minimal-domain-model
description: ...
---

# Minimal Domain Model

## Insight

用一句话表达核心认知。

## Principles 

- Principle 1
- Principle 2
- Principle 3

## Good Examples
展示符合原则的具体做法。

## Avoid
展示典型错误及其原因。

## Execution
仅在需要重复执行的工作流程中定义。

</template_schema>
