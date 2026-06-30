# Lion-Skills

> AI 能一秒写出几百行代码，但判断它靠不靠谱要靠你——除非你把判断标准装进了 skill。

## 问题：AI 写代码的「靠谱悖论」

| | 没有 skill 约束 | 有 Lion-Skills 约束 |
|---|---|---|
| **需求理解** | 直接动手，藏着的假设没人追问 | 先澄清，消除 X-Y problem |
| **方案设计** | 边写边想，缺乏决策记录 | 先写 spec，决策可评审、可追溯 |
| **任务拆解** | 大任务一步到位，漏边界 | 拆成端到端任务，每个可验证 |
| **代码质量** | 写完声称「没问题」 | 交付前用工具实际验证 |
| **Bug 排查** | 凭直觉乱改碰运气 | 科学方法缩小范围定位根因 |
| **代码审查** | 逐行看，漫无重点 | 有层次、分重点地审 |
| **提交信息** | `update` `fix` `tmp` | 规范 commit message，变更可追溯 |

**核心思路**：skill 是一份「操作手册」——在 AI 合适的时候自动触发，让它按最佳实践出活。你管方向，skill 管纪律。

## 工作流

```
需求进来 → 澄清问题 → 写设计文档 → 拆成任务 → 写代码 → 验证 → 修 bug → 审查 → 提交
   │          │         │           │        │       │      │       │      │
   │          │         │           │        │       │      │       │      └─ commit-message
   │          │         │           │        │       │      │       └─ code-review
   │          │         │           │        │       │      └─ debugging → verify-and-fix
   │          │         │           │        │       └─ testing
   │          │         │           │        └─ task-breakdown
   │          │         │           └─ spec-writing
   │          │         └─ (spec 定稿后)
   │          └─ clarifying-questions
   └─ 起始点
```

每个环节都有对应的 skill 兜底，不丢步骤、不跳验证。

## 12 个 skill

| Skill | 一句话 | 触发时机 |
|-------|--------|----------|
| `clarifying-questions` | 动手前把模糊需求澄清到位 | 需求描述含糊、有隐藏假设 |
| `spec-writing` | 写可评审、可追溯的设计文档 | 要做非平凡功能/重构，先定方案 |
| `task-breakdown` | 把需求拆成可验证的端到端任务 | 方案定了，要落地执行 |
| `testing` | 写能抓住 bug 且不脆弱的测试 | 写测试、补测试、修测试 |
| `debugging` | 科学方法定位 bug 根因 | 遇到报错/崩溃/行为异常 |
| `code-review` | 有重点、分层次的代码审查 | 审代码、做 CR、看 PR |
| `verify-and-fix` | 把「声称完成」变「经验证完成」 | 交付前确认、修 bug 修病因 |
| `commit-message` | 把 diff 提炼成规范提交信息 | 要 commit 了 |
| `onboarding-unknown-codebase` | 系统化上手陌生代码库 | 接手新项目/看陌生代码 |
| `naming` | 命名决策 | 变量/函数/类名拿不准 |
| `error-handling` | 错误处理与日志设计 | 设计错误处理、写日志 |
| `lion-writing-skills` | 教你怎么加新 skill | 想自定义自己的 skill |

## 安装

先 clone 仓库到本地：

```bash
git clone https://github.com/Lion-1209/Lion-Skills.git
```

推荐用 **symlink**——源仓库改了（`git pull`）各工具自动生效，单一来源、无需重装。

### Codex（OpenAI）

```bash
# macOS/Linux
for s in /path/to/Lion-Skills/skills/*/; do
  ln -s "$s" "$HOME/.agents/skills/$(basename "$s")"
done

# Windows PowerShell
$skills = Get-ChildItem "E:\path\to\Lion-Skills\skills" -Directory
foreach ($s in $skills) {
    New-Item -ItemType SymbolicLink -Path "$HOME\.agents\skills\$($s.Name)" -Target $s.FullName
}
```

### ZCode

```bash
# macOS/Linux
cp -r /path/to/Lion-Skills/skills/* ~/.zcode/skills/

# Windows PowerShell
Copy-Item -Recurse "E:\path\to\Lion-Skills\skills\*" "$HOME\.zcode\skills\"
```

> 拷贝是快照，`git pull` 后需重新拷贝。symlink 方式可自动同步。

### Claude Code（推荐 plugin 方式）

```
/plugin marketplace add Lion-1209/Lion-Skills
/plugin install lion-skills@lion-skills
```

> **不要同时用 plugin + skills 目录拷贝**——Claude Code 下用 plugin 一种方式即可，避免同名 skill 重复触发。

升级旧版（0.1.0 → 0.2.0）：
```
/plugin marketplace update lion-skills
/plugin install lion-skills@lion-skills
```

### 装完验证

新开会话，试试自然语言触发：

- 「这段 `try{fetch(url)}catch(e){console.log(e)}` 怎么改进」→ 触发 `verify-and-fix` 或 `error-handling`
- 「帮我写个登录功能的 spec」→ 触发 `spec-writing`
- 「这里报错怎么排查」→ 触发 `debugging`

## 怎么加新 skill

见 `skills/lion-writing-skills/SKILL.md`，或复制 `template/SKILL.md` 开始。

## 相关文章

- [AI Agent 开发工作流 Skills 合集详解（CSDN）](https://blog.csdn.net/qq_52935238/article/details/162311617)
- [AI Agent 开发工作流 Skills 合集详解（掘金）](https://juejin.cn/post/7655208364164694025)
- [AI Agent 开发工作流 Skills 合集详解（微信公众号）](https://mp.weixin.qq.com/s/oI1VdN2w-JCFN3mQkHBfCg)

## 设计依据

- 套件架构与 skill 间协作：[spec](docs/specs/2026-06-22-skill-suite-architecture.md)
- 初始设计意图：[spec](docs/specs/2026-06-16-lion-skills-design.md)

## License

MIT
