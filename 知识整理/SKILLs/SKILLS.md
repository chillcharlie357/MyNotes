---
aliases:
tags:
  - 技术分享
  - SKILL
  - Agent
categories:
sticky:
thumbnail:
cover:
excerpt: false
mathjax: true
comment: true
title: SKILLS
date: 2025-12-22 11:12
modified: 2025-12-26 20:12
---

# Overview


![](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image/2025/12/26/20-59-37-a6687ab9b8077d9eba2275d265bf3d93-%7Borigin%7D-pEGPmF.png)


# 技术架构

Claude Skill 的三层加载架构基于**渐进式披露（Progressive Disclosure）** 的设计原则。这种机制允许 Claude 在不浪费上下文窗口（Context Window）的情况下，管理几乎无限的程序化知识库。

## 第一层：元数据（Metadata）—— 启动时加载

- **内容**：包含在 `SKILL.md` 文件顶部的 **YAML 前件（Frontmatter）**，主要是技能的 `name`（名称）和 `description`（描述）。

- **触发机制**：在会话启动时，Claude 会**预加载所有已安装技能的元数据**。

- **作用与成本**：这是“发现”阶段。Claude 利用这些描述来判断技能是否与用户请求相关。其成本极低，每个技能约占用 **30-100 个 tokens**，因此用户可以同时安装上百个技能而不会导致上下文溢出。

## 第二层：指令（Instructions）—— 触发后加载

- **内容**：`SKILL.md` 的正文部分，包含具体的**工作流、步骤指南和最佳实践**。

- **触发机制**：仅当用户请求匹配到第一层的描述时，Claude 才会通过 bash 命令从文件系统中**读取完整的** **SKILL.md** **内容**进入上下文。

- **作用与成本**：提供技能的核心逻辑。建议正文保持在 **5,000 个 tokens（约 500 行）以内**，以确保响应的高效和精准。

## 第三层：资源与代码（Resources and Code）—— 按需加载

- **内容**：存储在技能目录子文件夹中的辅助文件，包括：
- **scripts/**：可执行的 Python 或 Bash 脚本。    ◦ **references/**：详细的 API 文档、模式或参考资料。    ◦ **assets/**：模板、配置或二进制资源。

- **触发机制**：只有当 `SKILL.md` 中的指令明确要求，或 Claude 在执行过程中认定有必要时，才会**选择性地调用**这些资源。

- **作用与成本**：  
   - **确定性操作**： 脚本通过 bash 执行，**其代码本身不进入上下文**，仅输出结果消耗 tokens，极大地节省了资源并提高了可靠性。  
   - **海量存储**：技能可以捆绑数 MB 的参考资料，但 Claude 每次只读取当前任务所需的特定片段

# 运行机制

## 纯推理驱动的选择逻辑

Agent Skills 的触发机制既不是基于嵌入（Embedding）的相似度搜索，也不是基于正则表达式或机器学习分类器的**算法路由**。

- **非算法化**：在代码层面，不存在自动化的技能选择或意图检测逻辑。
- **纯模型推理**：决策完全发生在 Claude 的 transformer 前向传播过程中。模型通过阅读其系统提示词（System Prompt）或工具数组（Tools Array）中的**文本描述**，自主决定是否调用某个技能。
- **声明式系统**：这是一种“声明式、基于提示词”的系统，模型根据提供的描述来匹配用户意图

## 元工具（Meta-tool）

- Claude的实现定义了一个`Skill`元工具，这个工具的描述字段包含了一个动态生成的、所有可用skill的格式化列表(`<available_skills>`)。
- skill的调用类似操作系统中的动态链接库概念，只用在使用到具体skill时才会将指令、资源和代码载入上下文，而不是像传统的mcp/tools一样把所有的工具参数、描述等全部放入上下文。

> When users send a request, Claude receives three things: user message, the available tools (Read, Write, Bash, etc.), and the Skill tool. The Skill tool’s description contains a formatted list of every available skill with their name , description , and other fields combined. Claude reads this list and uses its native language understanding to match your intent against the skill descriptions. If you say “help me create a skill for logs,” Claude sees the internal-comms skill’s description (“When user wants to write internal communications using format that his company likes to use”), recognizes the match, and invokes the Skill tool with command: "internal-comms" .

## 渐进式披露（Progressive Disclosure）

推理触发能够高效运行的关键原理是**渐进式披露架构**，它解决了“海量知识”与“有限上下文”之间的矛盾。

- **低成本触发（Tier 1）**：在启动时，Claude 仅加载每个技能的名称和简短描述（Metadata），每个技能仅消耗约 100 个 tokens。
- **按需加载（Tier 2 & 3）**：只有当模型通过推理决定触发该技能后，完整的 `SKILL.md` 指令、相关脚本和参考资源才会被读入上下文。
 - **设计哲学：先提示，后深挖（Hint, then dive）**：这种模式确保了基础上下文的精简，同时允许代理在需要时访问几乎无限的程序化知识库。

## 灵活性与不透明性

• **优势：灵活性与组合性**：由于是推理驱动，多个技能可以自动协调。Claude 可以根据一个自然语言请求，自动确定加载多个技能的顺序和依赖关系。  
• **挑战：触发的不透明性**：这种决策是模型内部的，开发者无法直接审查 Claude 为何选择（或不选择）某个技能。  
• **调试依赖试错**：触发的可靠性高度依赖于 `description` 字段的质量。开发者通常需要反复优化描述语，通过“试错”而非确定性的代码逻辑来确保触发。

# SKILL结构

一个规范的 Skill 必须是一个**自包含的目录**，其核心是位于根目录的 **SKILL.md** 文件。

- **YAML Frontmatter**：发现与调用的“元数据”
	- **name**：最大 64 字符，必须使用**小写字母、数字和连字符（kebab-case）**，禁止使用 XML 标签或保留字（如 "anthropic"）。
	- **description**：被视为**最关键的字段**，最大 1024 字符（UI 显示限制为 200 字符）。规范要求必须以**第三人称**编写，且必须明确说明**技能做什么**以及**何时使用**（触发条件），因为 Claude 完全依靠此描述通过 **LLM 推理**来决定是否调用该技能。
	- **可选配置**：包括 `allowed-tools`（限制技能可调用的工具权限）和 `model`（为特定任务请求更高能力的模型，如 `opus`）。
- **SKILL.md**: 
	- **字数限制规范**：为了防止上下文溢出（Context Bloat），规范建议正文应保持在 **500 行或 5,000 词以下**。如果内容超过此限制，必须通过**参考资料分片（Reference Sharding）**模式将细节转移到 `references/` 目录中。
	- **内容编写模式**：应使用**祈使句**（如“分析代码…”而非“你应该分析…”），并包含明确的工作流步骤、示例和错误处理逻辑。
- **标准目录布局**：为了实现资源的高效管理，推荐使用三目录模式：  
    - **scripts/**：存放可执行的 Python 或 Bash 脚本。其规范要求脚本应处理确定性操作，且**只有输出进入上下文**，以节省 Token 并提高可靠性。  
    - **references/**：存放详细的文档、API 模式或查询表。这些内容通过 `Read` 工具**按需加载**到上下文中。  
    - **assets/**：存放模板、图片或二进制资源。这些文件**仅通过路径引用，不消耗 Token**。

# OpenSkills

OpenSkills 的核心价值在于它是一个**通用的 AI 编码代理技能加载器**，它不仅支持 Claude Code，还扩展到了 Cursor、Windsurf 和 Aider 等其他主流 AI 代理。

- **跨代理互操作性**：它将原本局限于 Anthropic 生态的功能转化为一种通用的“知识包格式”。这使得开发者可以“一次编写，到处运行”，在不同的 AI 界面中共享相同的专业知识库。
- **去中心化分发**：与官方 marketplace 相比，OpenSkills 允许用户从**任何 GitHub 仓库、本地路径或私人 Git 仓库**安装技能。这体现了开源生态中对“知识所有权”和“分发自由”的最佳实践，避免了单一中心化市场的限制。

- **架构对齐**：它保留了 SKILL.md 格式、YAML 前件、分层加载模式以及 `references/` 和 `scripts/` 的目录结构。这种对齐确保了生态工具之间不存在迁移门槛。
- **触发机制的变体**：虽然底层逻辑仍基于 **LLM 推理触发**，但 OpenSkills 将官方的元工具（Skill tool）调用转化为 CLI 命令 `openskills read <name>`。这种转换在非 Anthropic 代理的环境中实现了等效的**渐进式披露**效果。

# 参考链接

1. [GitHub - numman-ali/openskills: Universal skills loader for AI coding agents - npm i -g openskills](https://github.com/numman-ali/openskills)
2. [Claude Agent Skills: A First Principles Deep Dive](https://leehanchung.github.io/blogs/2025/10/26/claude-skills-deep-dive/)
3. [Skill authoring best practices - Claude Docs](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)
4. [numman-ali/openskills | DeepWiki](https://deepwiki.com/numman-ali/openskills)