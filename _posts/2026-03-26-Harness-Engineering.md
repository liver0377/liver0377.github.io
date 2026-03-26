---
layout: post
title: "Harness-Engineering"
date: 2026-03-26
categories: [blog]
---

# 定义

harness, 在"harness engineering"这个词的语境中表示驾驭，利用，下文均不进行翻译，直接称之为**harness**



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

### Prompt Engineering

Prompt Engineering关注的是如何写好一个prompt, 进而让大模型能够生成出最好的回复

具体来说，其关注的是“怎么写system prompt", "怎么写few-shot", "怎么组织prompt 的语言风格"





### Context Engineering

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



### Harness Engineering

Harness Engineerign关注的是，Agent在什么样的环境里做事, 包括

- 什么时候可以执行
- 执行时有哪些工具
- 工具是否需要审批
- 输出太长怎么办
- 会话如何恢复
- 中间产物如何持久化
- ...



Harness工程的本质，是让Agent从“会调用工具的模型”， 变成“能够在约束中稳定完成任务的系统”



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







