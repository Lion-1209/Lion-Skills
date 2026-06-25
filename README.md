# Lion-Skills

一套个人自建的通用开发工作流 skills 套件，面向所有支持 Agent Skills 的工具（Claude Code、ZCode、Codex 等）。

## 这是什么

Lion-Skills 把高频的软件开发工作流沉淀成可复用的 skill：每个 skill 是一份"操作手册"，AI 在合适的时机触发它，照着做出更靠谱的结果。

**核心载体是标准 `SKILL.md`（Anthropic Agent Skills 规范）——这是跨工具通用标准，三个主流工具都原生支持。** 各工具的差异只在"装到哪个目录 / 用哪种 plugin manifest"，skill 内容一份不动。

## 包含的 skill

| Skill | 用途 |
|-------|------|
| `clarifying-questions` | 动手前澄清模糊、藏假设、X-Y problem 的需求 |
| `spec-writing` | 写可评审、可追溯、可验收的设计文档 |
| `task-breakdown` | 把需求拆成可验证的端到端任务 |
| `testing` | 写能抓住 bug 且不脆弱的测试 |
| `debugging` | 用科学方法定位 bug 根因，而非凭直觉乱改 |
| `code-review` | 有重点、分层次的代码审查 |
| `verify-and-fix` | 把"声称完成"变成"经验证完成"，修病因不修症状 |
| `commit-message` | 把 diff 提炼成规范的提交信息 |
| `onboarding-unknown-codebase` | 系统化上手陌生代码库 |
| `naming` | 命名决策 |
| `error-handling` | 错误处理与日志设计 |
| `lion-writing-skills` | 教你怎么加新 skill（元 skill） |

## 怎么安装

先 clone 仓库到本地（以下都假设 clone 到 `/path/to/Lion-Skills`）：

```bash
git clone https://github.com/Lion-1209/Lion-Skills.git
```

然后按你用的工具选一种装法。**推荐用 symlink**——源仓库改了（`git pull`）各工具自动生效，单一来源、无需重装。

### Codex（OpenAI）

Codex 原生支持 Agent Skills，技能目录是 `~/.agents/skills/`（用户级）。

```bash
# macOS/Linux：symlink 每个 skill
for s in /path/to/Lion-Skills/skills/*/; do
  ln -s "$s" "$HOME/.agents/skills/$(basename "$s")"
done
```

```powershell
# Windows PowerShell（需开发者模式或管理员；不想用符号链接就改用 Copy-Item）
$skills = Get-ChildItem "E:\path\to\Lion-Skills\skills" -Directory
foreach ($s in $skills) {
    New-Item -ItemType SymbolicLink -Path "$HOME\.agents\skills\$($s.Name)" -Target $s.FullName
}
```

装完用 `/skills` 或在 prompt 里 `$skill名` 触发。

### ZCode

两种方式：

**方式 A：当 plugin 装**（仓库已配 `.zcode-plugin/plugin.json`）。在 ZCode 里 `Agents Settings > Plugins > Discover` 安装，或按 ZCode plugin 文档操作。

**方式 B：symlink 到用户级 skills 目录** `~/.zcode/skills/`：

```bash
# macOS/Linux
for s in /path/to/Lion-Skills/skills/*/; do
  ln -s "$s" "$HOME/.zcode/skills/$(basename "$s")"
done
```

```powershell
# Windows PowerShell
$skills = Get-ChildItem "E:\path\to\Lion-Skills\skills" -Directory
foreach ($s in $skills) {
    New-Item -ItemType SymbolicLink -Path "$HOME\.zcode\skills\$($s.Name)" -Target $s.FullName
}
```

> 注：plugin 方式能否被 ZCode CLI 正确加载、marketplace 注册细节，建议以 [ZCode 官方 plugin 文档](https://zcode.z.ai/en/docs/plugin) 为准——本仓库 manifest 按其格式配置，但各版本行为可能不同。

### Claude Code

两种方式：

**方式 A：当 plugin 装**（仓库已配 `.claude-plugin/`）：

```
/plugin marketplace add Lion-1209/Lion-Skills
/plugin install lion-skills@lion-skills
```

**方式 B：symlink 到用户级 skills 目录** `~/.claude/skills/`：

```bash
# macOS/Linux
for s in /path/to/Lion-Skills/skills/*/; do
  ln -s "$s" "$HOME/.claude/skills/$(basename "$s")"
done
```

```powershell
# Windows PowerShell
$skills = Get-ChildItem "E:\path\to\Lion-Skills\skills" -Directory
foreach ($s in $skills) {
    New-Item -ItemType SymbolicLink -Path "$HOME\.claude\skills\$($s.Name)" -Target $s.FullName
}
```

### 装完的验证

任意工具新开会话，自然语言触发一个 skill 试试，例如：
- 「这段 `try{fetch(url)}catch(e){console.log(e)}` 怎么改进」→ 应触发 `verify-and-fix` 或 `error-handling`
- 「帮我写个登录功能的 spec」→ 应触发 `spec-writing`

各 skill 的标识可能带 plugin 前缀（如 `lion-skills:commit-message`），日常靠自然语言触发即可，无需关心前缀。

## 怎么加新 skill

见 `skills/lion-writing-skills/SKILL.md`，或复制 `template/SKILL.md` 开始。

## 设计依据

- 套件架构与 skill 间协作：`docs/specs/2026-06-22-skill-suite-architecture.md`
- 初始设计意图：`docs/specs/2026-06-16-lion-skills-design.md`
