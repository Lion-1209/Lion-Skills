# Contributing to Lion-Skills

欢迎贡献！Lion-Skills 是一个开放的 skill 套件，你可以添加新 skill、改进现有 skill、或修正文档。

## 开发流程

1. Fork 本仓库
2. 创建功能分支：`git checkout -b feat/my-new-skill`
3. 按 `template/SKILL.md` 模板编写 skill
4. 在 `skills/lion-writing-skills/SKILL.md` 中查看 skill 编写规范
5. Commit：`git commit -m 'feat: add my-new-skill'`
6. Push：`git push origin feat/my-new-skill`
7. 开 Pull Request

## Skill 规范

每个 skill 是一个目录，包含：

```
skills/my-skill/
├── SKILL.md       # 必需的 skill 定义文件
└── README.md      # 可选的详细说明
```

### SKILL.md 必须包含

```yaml
---
name: my-skill
description: 一句话说明何时用这个 skill。涉及关键词：key1、key2、key3。
---

# My Skill

## 概述
简短说明这个 skill 做什么、为什么有用。

## 何时使用
- 适用场景 1
- 适用场景 2

**不该用**：不适用场景。

## 核心方法
（具体操作步骤）
```

### 命名约定

- `name`：小写 + 连字符，如 `code-review`、`spec-writing`
- `description`：精简到核心触发条件（参考现有 skill）
- 目录名与 `name` 一致

## 提交信息规范

本项目使用 [Conventional Commits](https://www.conventionalcommits.org/)：

- `feat:` 新 skill 或功能
- `fix:` 修正现有 skill
- `docs:` 文档变更
- `refactor:` 重构（不影响功能）
- `chore:` 工具链、配置等

## 行为准则

- 一个 skill 只解决一个问题
- 优先给用户决策框架，而非直接给答案
- skill 之间不重复——如果有重叠，考虑合并或明确衔接点
