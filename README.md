# Lion-Skills

一套个人自建的通用开发工作流 skills 套件，面向 Claude Code（也兼容其他支持 Agent Skills 的工具）。

## 这是什么

Lion-Skills 把高频的软件开发工作流沉淀成可复用的 skill：每个 skill 是一份"操作手册"，Claude 在合适的时机触发它，照着做出更靠谱的结果。

## 包含的 skill

| Skill | 用途 |
|-------|------|
| `commit-message` | 把 diff 提炼成规范的提交信息 |
| `onboarding-unknown-codebase` | 系统化上手陌生代码库 |
| `naming` | 命名决策 |
| `error-handling` | 错误处理与日志设计 |
| `lion-writing-skills` | 教你怎么加新 skill（元 skill） |

## 怎么安装

### 推荐：作为 Claude Code plugin 安装

在 Claude Code 里两步：

```
/plugin marketplace add Lion-1209/Lion-Skills
/plugin install lion-skills@lion-skills
```

装完即用——5 个 skill 会在合适的时机自动触发。skill 标识带 plugin 前缀（如 `lion-skills:commit-message`），但日常靠自然语言触发即可，无需关心前缀。

### 备选：手动 clone + 链接

不想用 plugin，clone 后把需要的 skill 软链到 `~/.claude/skills/`：

Linux/macOS：
```bash
git clone https://github.com/Lion-1209/Lion-Skills.git
ln -s /path/to/Lion-Skills/skills/commit-message ~/.claude/skills/commit-message
```

Windows（PowerShell，符号链接需管理员权限或开发者模式，或直接拷贝）：
```powershell
git clone https://github.com/Lion-1209/Lion-Skills.git
Copy-Item -Recurse E:\999-Git\Lion-Skills\skills\commit-message $HOME\.claude\skills\
```

## 怎么加新 skill

见 `skills/lion-writing-skills/SKILL.md`，或复制 `template/SKILL.md` 开始。

## 设计依据

见 `docs/specs/2026-06-16-lion-skills-design.md`。
