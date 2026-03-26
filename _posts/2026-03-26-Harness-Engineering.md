---
layout: post
title: Harness Engineering
date: 2026-03-26
categories: [blog]
toc: true
---

## harness是什么？

**Harness是Agent运行时的"工程环境总和**

其至少包含以下几类东西：

- 任务如何被表达
- 上下文如何被组织
- 工具如何被暴露，治理，拦截
- 状态如何被保存，恢复，裁剪
- 反馈如何回流给模型
- 错误如何被分类、重试、修复
- 系统如何验证Agent真正完成了需求



如果将Agent比喻成汽车的引擎，那么Harness就是汽车的骨架，方向盘，传感器，甚至保险

所以**Harness = 让Agent能够被稳定驾驭的环境系统**



## Agent时代的软件工程

### Prompt Engineering（2023~2024）

Prompt Engineering关注的是如何写好一个prompt, 进而让大模型能够生成出最好的回复

具体来说，其关注的是“怎么写system prompt", "怎么写few-shot", "怎么组织prompt 的语言风格"





### Context Engineering（2025）

Context Engineering本质上还是在关注prompt, 但其在prompt engineering的基础上更近一步，它关注的是送给模型的整个上下文的组织，Karpathy将其总结为

>***[Context engineering is the] ”…delicate art and science of filling the context window with just the right information for the next step.”***

其衍生出了一系列技术

- RAG
- Tools
- 记忆
- 上下文压缩/修剪
- skills
- ...

> 参考资料: [Context Engineering for Agents](https://blog.langchain.com/context-engineering-for-agents)



### Harness Engineering（2026）

Harness Engineering是围绕AI模型构建“运行环境与管控基础设施”的工程实践，目的是让AI Agent在**长时、复杂、多步骤**的真实任务中稳定、可靠、高效地运行

Harness Engineering解决的核心问题是：

- Context Rot（上下文腐化）：模型在长任务中记忆窗口被垃圾信息淹没，丧失对原始指令的遵循
- Hallucinated Completion（幻觉完成）：模型不堪重负时伪造"任务完成"的假象
- Model Drift（模型漂移）：模型在多步执行中逐步偏离初始目标



包含以下技术点：

- 记忆

  使用文件系统代替模型的内部记忆

- 确定性验证性轨道

  Linter，类型检查，单元测试等刚性门禁，AI的输出必须通过测试才能被接受

- 原子任务+上下文刷新

  每个Agent只做一个小任务就销毁，新Agent从干净状态启动，彻底杜绝Context Rot

- 子Agent集群（Swarm)

  使用大量便宜快速的小模型并行工作，而非一个大模型顺序处理

- skills

  按需加载专用skills, Agent只看到与当前任务相关的工具和说明

- Guard Rails + Checkpoints

  在关键节点自动运行检查，防止Agent偏离轨道

- Handoffs

  Session之间通过进度文件、git commit传递状态，实现跨session 连续性

- Human-in-the-loop

  在关键阶段设置人工审核断点，来弥补AI自验证的不足

- 垃圾回收Agent

  定期扫描、修复代码库与文档的不一致性，对抗系统的熵增





这三者是逐步递进的关系，Harness 内部的每个session仍然需要Context Engineering，每次 prompt仍然需要Prompt Engineering









## 工程实践

**OpenAI是怎么做的？**

OpenAI为了避免超长上下文带来的腐烂问题，将文档进行了组织，以进行渐进式提示词披露

```txt
AGENTS.md
ARCHITECTURE.md
docs/
├── design-docs/
│   ├── index.md
│   ├── core-beliefs.md
│   └── ...
├── exec-plans/
│   ├── active/
│   ├── completed/
│   └── tech-debt-tracker.md
├── generated/
│   └── db-schema.md
├── product-specs/
│   ├── index.md
│   ├── new-user-onboarding.md
│   └── ...
├── references/
│   ├── design-system-reference-llms.txt
│   ├── nixpacks-llms.txt
│   ├── uv-llms.txt
│   └── ...
├── DESIGN.md
├── FRONTEND.md
├── PLANS.md
├── PRODUCT_SENSE.md
├── QUALITY_SCORE.md
├── RELIABILITY.md
└── SECURITY.md
```

`ARCHITECTURE.md`是整个文档组织的目录



具体可以看openai的blog：[Harness Engineering: Leveraging Codex in an agent-first world]([Harness engineering: leveraging Codex in an agent-first world | OpenAI](https://openai.com/index/harness-engineering/))



## Anthropic

官方blog: [Effecitve harness for long-running Agent]([Effective harnesses for long-running agents \ Anthropic](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents))





## Q&A

**什么是Harness Engineering?**

Harness Engineering 是围绕 AI 模型构建运行环境、调度机制和管控基础设施的一种工程实践。它的目标不是单纯让模型“更聪明”，而是让 AI Agent 在长时、复杂、多步骤的真实任务中，能够稳定、可靠、可控地持续运行。

如果用一句更通俗的话来说，Harness Engineering 做的事情，就是不再把 AI 当成一个一次性回答问题的黑盒，而是把它放进一个被设计好的工作系统里，让它按规则做事。

它背后的设计哲学也很重要：模型只是原材料，环境才决定结果能不能落地。

再强的模型，如果直接裸跑长任务，也会遇到上下文腐化、失忆、幻觉完成、任务漂移等问题。所以 Harness Engineering 的核心，不是继续在模型上堆能力，而是通过外部记忆、任务拆解、测试门禁、状态交接和人工审核，把 AI 的执行过程工程化。



**Harness Engineering想解决的问题是什么？**

它主要解决的是 AI 在长时任务里的可靠性问题。

第一个问题是 Context Rot，也就是上下文腐化。模型做长任务时，上下文窗口会不断被历史对话、错误尝试和无关信息填满，最后导致它丧失对原始目标的把握。

第二个问题是 Hallucinated Completion，也就是幻觉完成。模型在复杂任务中迷失后，往往不会老实说“我不会了”，而是会输出一个看起来完成、实际上并不正确的结果。

第三个问题是 Model Drift，也就是模型漂移。多步执行中，模型会逐渐偏离最初目标，最后做出来的东西方向就歪了。

所以我理解 Harness Engineering 的价值，不是让模型在单轮回答里更强，而是让它在几十步、几百步的连续工作流里，依然能沿着正确轨道往前走。





**Harnesss Engineering与Prompt Engineering, Context Engineering的关系是什么？**

我会把它理解成一个演进关系，而不是替代关系。

Prompt Engineering 主要解决的是“对 AI 说什么”，也就是如何设计指令，让单次输出更好。

Context Engineering 进一步解决的是“让 AI 知道什么”，也就是在一次 session 或上下文窗口里，给它什么信息、给多少信息、怎么组织信息。

而 Harness Engineering 更进一步，它解决的是“让 AI 在什么环境里做事”。

也就是说，Harness Engineering 并没有取代前两者，而是把它们包进了一个更大的系统里。Harness 内部的每个 session 还是要做 Context Engineering，每一次给 agent 的具体任务描述还是要做 Prompt Engineering。只是现在关注点从“优化一次对话”升级成了“设计一整个运行体系”。 



**Harness Engineering的最佳实践是什么？**

第一，状态外部化。不要把连续性寄托在模型记不记得，而要把状态写进文件、Git 或数据库，让系统本身有记忆。

第二，任务原子化。不要让一个 agent 一口气做一个大需求，而是拆成足够小、足够明确的原子任务，每次只做一件事。

第三，上下文刷新。很多先进的 harness 架构都会在一个小任务完成后销毁当前 agent，再启动一个全新的 agent 来继续做下一轮。这么做的目的，就是从根本上避免上下文腐化。

第四，验证优先。AI 的输出必须经过测试和规则检查，而不是靠它自我声明“完成了”。这是把模型主观判断变成工程确定性的关键。

第五，按需暴露工具和技能。不要一次给模型上百个工具，而应该通过 skills、toolkits 或 task routing，让它按需加载当前任务相关的工具和说明。

第六，在关键点加入 Human-in-the-loop。尤其是高价值任务或者多步骤流程里，人工审核断点很重要，它不是系统失败的表现，而是提高整体可靠性的工程设计。

第七，保持轻量、模块化、模型无关。因为模型变化很快，如果 Harness 和某个模型耦合太深，很快就会过时。所以好的 Harness 应该能在尽量少改动的情况下替换模型、替换技能、替换验证逻辑。



**如果让你用Harness Engineering来实现一个项目，你怎么设计？**

如果让我来落地一个 Harness，从一个 Initializer + Coding Loop 的最小闭环开始，而不是一开始就做复杂的多 agent 平台。

第一步，我会先准备一份清晰的 App Spec / PRD，把产品目标、功能范围和验收标准定义清楚。因为在 Anthropic 的架构里，系统起点不是直接写代码，而是先把需求整理成后续 agent 可以反复执行的任务蓝图。

第二步，我会实现一个 Initializer Agent。它的职责不是写业务代码，而是读取需求文档，把大任务拆成一组细粒度、可执行、可验证的 feature，并生成类似 `feature_list.json` 这样的状态文件。同时它还会初始化项目骨架、代码仓库，以及后续循环执行所需的基础环境。这样系统就先有了“任务清单”和“状态来源”。

第三步，我会实现一个 Coding Loop。每一轮都启动一个全新的 coding agent，让它只读取当前状态文件、进度记录和代码库信息，只完成一个明确的小任务。任务完成后，不是让 agent 自己宣布成功，而是必须经过测试、lint 或类型检查等确定性验证；只有验证通过，才更新状态文件、写入交接记录并提交代码。然后这个 agent 立即退出，下一轮由新的 agent 接力。

第四步，我会把 外部记忆 做成系统核心，而不是把连续性寄托在模型上下文里。也就是说，我会依赖 feature list、progress file、Git log 这些可持久化信息，让每个新 agent 启动时都能快速接手，而不是依赖前一轮 session 的“记忆”。

第五步，我会在关键节点加入 Human-in-the-loop。Anthropic 的方法虽然高度自动化，但并不是完全无人值守。所以在高风险功能、关键界面变更或者多轮失败之后，我会设计人工确认断点，确保系统不会沿着错误方向持续推进。

整体上，我不会把重点放在“怎么让一个模型一次做完所有事”，而是把重点放在：先拆解、再循环执行、靠外部状态保持连续性、靠确定性验证保证质量。我认为这是一种更可落地，也更适合真实工程环境的 Harness 实现方式。



