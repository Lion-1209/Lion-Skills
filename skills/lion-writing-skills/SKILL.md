---
name: lion-writing-skills
description: 当用户想在 Lion-Skills 仓库里创建新 skill、修改现有 skill、或检查 skill 是否符合本仓库规范时使用。典型信号：用户说"帮我加个新 skill""写个 skill""创建一个 skill""这个 skill 怎么改/优化""检查这个 skill 写得对不对"、想把某个重复的工作流沉淀成 skill、想给 Lion-Skills 套件加成员。涉及关键词：创建 skill、写 skill、新 skill、lion-skills、SKILL.md、skill 模板、skill 规范、验证 skill。
---

# Lion Writing Skills

## 概述

Lion-Skills 的"造 skill 流水线"：按本仓库规范，把一个重复的工作流沉淀成可复制、经验证的 skill。

核心原则：skill 是给未来 Claude 读的"操作手册"——它的价值不在于你写了什么，而在于未来的 Claude 在触发它时，能照着做出正确的事。

## 何时使用

- 用户想新增一个 skill（"帮我加个 X 的 skill"）
- 用户想修改/优化现有 skill
- 用户想检查某个 skill 写得是否符合规范
- 想把对话里反复出现的工作流沉淀成 skill

**不该做成 skill 的情况**：一次性的、项目特定的约定（放 CLAUDE.md）、能用简单校验自动化的（写成校验脚本而不是文档）。

## 核心内容

### 第 1 步：判断值不值得做

回答四个问题（都"是"才值得做）：
1. 高频吗？（反复出现，不是一次性的）
2. 跨项目吗？（不止一个代码库用得到）
3. 有判断空间吗？（需要决策/经验，不是机械操作——机械操作写成脚本）
4. 未来 Claude 不读这个 skill，会做错或做差吗？

任何一个答"否"，就别做成独立 skill：项目特定的约定放 CLAUDE.md，能机械自动化的写成校验脚本，一次性的直接在对话里处理。

### 第 2 步：复制模板，填 frontmatter

新 skill 建在 `skills/<name>/SKILL.md`（同名目录，里面放 `SKILL.md`，可选再放 `evals/` 存验证用例）。复制 `template/SKILL.md` 过去，填两个字段：
- `name`：全小写、连字符，用 skill 的核心动作或概念命名（如 `commit-message`、`error-handling`、`onboarding-unknown-codebase`）。不要下划线或驼峰（`commit_message`、`CommitMessage` 都错）
- `description`：**只写"何时触发"**，第三人称，中文，把用户可能说的各种说法都列进去（上限 1024 字符）

> 关键：description 绝不能写工作流（"先做 A 再做 B"）。原因——测试发现 Claude 会偷懒只读 description 跳过正文。把流程留给正文。

### 第 3 步：写正文

按这个骨架：概述 / 何时使用 / 核心内容 / 常见错误（参考 `template/SKILL.md`）。

写作约定：
- **解释 why**，少用全大写 MUST（讲道理比下命令管用）
- **一个优秀例子** > 多语言版本
- 正文 < 500 行；超了拆 `references/`
- 中文为主，工具名/术语保留英文

### 第 4 步：轻量验证

写 2–3 个真实测试 prompt（像真实用户会说的，带具体细节），用 subagent 带着新 skill 跑。

**范式**：派一个 general-purpose subagent，prompt 写「读取 `skills/<name>/SKILL.md`，然后按其指引处理这个需求：<测试 prompt>。完成后报告：你走了哪些步骤、产出什么、哪里卡壳」。

**通过标准**：subagent 的产出解决了用户问题、且遵循 skill 的设计意图（该走流程的走了流程、该遵循的约定遵循了）。卡壳或跑偏就改正文重跑——这一步最容易被跳过，但也最能发现问题，别省。

### 第 5 步：迭代

输出不对就改正文，重跑。重点改：
- 触发不准 → 改 description（补关键词、加症状）
- 做错了 → 正文讲清 why 或加常见错误
- 啰嗦/跑偏 → 删冗余

## 常见错误

| 问题 | 后果 | 修法 |
|------|------|------|
| description 写了工作流 | Claude 只读 description 跳过正文 | description 只留触发条件 |
| 正文超 500 行没拆 | 加载慢、难维护 | 大段参考拆到 `references/` |
| 堆 MUST 不讲 why | 死板、难泛化 | 解释原因 |
| 一个例子写多语言 | 平庸、难维护 | 一个优秀例子 |
| name 用下划线/驼峰 | 不规范 | 全小写连字符 |
