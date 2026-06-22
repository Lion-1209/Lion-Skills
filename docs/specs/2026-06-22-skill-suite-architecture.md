# Lion-Skills 套件架构

- **日期**：2026-06-22
- **状态**：已实现 + 已验证
- **范围**：记录 8 个 skill 的流水线关系、协作机制、已验证的协作证据
- **目的**：让后续读者（包括未来的 Claude）一眼看清 skill 之间如何协作，避免把每个 skill 当孤立工具用

> **文档定位**：这是套件级架构索引，不是单个 skill 的设计文档。各 skill 的内部设计见各自的 `SKILL.md`，初始设计意图见 `2026-06-16-lion-skills-design.md`。

---

## 1. 流水线：需求到交付的主链

三个 skill 构成"需求 → 交付"的主链，按顺序协作：

```
clarifying-questions → spec-writing → task-breakdown → 执行
   (澄清需求)            (写设计文档)    (拆任务)
```

| 环节 | skill | 产出 | 何时进入下一环 |
|------|-------|------|---------------|
| 澄清 | `clarifying-questions` | 双方对齐的需求理解（含已明决策 + 默认假设 + 未答未知） | 核心方向已明、未答未知不再阻塞方向 |
| 设计 | `spec-writing` | 可评审的 spec（决策 + 范围 + 验收标准 + TBD） | spec 定稿（阻塞澄清已答、关键 spike 有结论） |
| 拆解 | `task-breakdown` | 可执行任务清单（垂直切片 + 完成定义 + 研究/执行分离） | 任务可分配/可执行 |

**关键衔接契约**（上游产出必须满足的、下游消费的）：

- clarifying → spec：clarifying 的"硬问题"被 spec 继承为阻塞澄清项；clarifying 的"默认假设"被 spec 作为候选决策；clarifying 的"未答未知"被 spec 显式列为 TBD。
- spec → task：spec 的"验收标准"被 task 一一映射到各切片的"完成定义"；spec 的 TBD 对应到 task 的 spike；spec 的范围决定 task 拆什么不拆什么。

---

## 2. 横切 skill：贯穿开发全程

四个 skill 不在主链上，而是在开发全程按需触发：

| skill | 触发时机 | 与主链的关系 |
|-------|---------|-------------|
| `commit-message` | 任何提交前 | 把 task-breakdown 拆出的任务做完后，提炼成规范提交信息 |
| `naming` | 任何命名决策 | 写 spec / 拆任务 / 写代码时随时可能触发 |
| `error-handling` | 设计错误处理时 | spec 的"设计"节或实现期，决定错误三态（抛/处理/重试） |
| `onboarding-unknown-codebase` | 接手陌生代码库时 | 与主链解耦，是独立的"上手"流程 |

---

## 3. 元 skill：自我繁衍

`lion-writing-skills` 是**元 skill**——它教怎么造新 skill、怎么验证。本仓库所有 skill 都是按它的规范产出的。它不在任何流水线上，但**约束所有其它 skill 的形态**（frontmatter 规范、正文骨架、轻量验证流程）。

---

## 4. 共同的方法论 DNA

8 个 skill 各自独立，但共享几条贯穿的设计原则（这是设计决策，不是巧合）：

- **决策重于知识**：每个 skill 都把篇幅让给"为什么这么定"，而不是堆背景（spec-writing 显式提出，其余 skill 遵循）。
- **显式标注不确定性**：TBD（spec-writing）、spike（task-breakdown / spec-writing）、默认假设推进（clarifying-questions）——不同 skill 用不同术语，本质都是"别把没定的包装成已定"。
- **少而准**：澄清少问关键问题、spec 精简不膨胀、task 不拆过细——共同反对"多而全"。
- **每步可验证**：spec 有验收标准、task 有完成定义、skill 自身有 evals——形成多层验证。

---

## 5. 已验证的协作证据

跨 skill 协作经过两类验证：

**A. 一致性验证**（提交 `04726c1`）：修复了 4 处跨 skill 问题——流水线相邻 skill 加上下游 mention、spike 术语自包含定义、澄清方法论互补引用、术语对齐。

**B. 流水线串跑验证**：

- **两环**（spec → task，提交前的验证）：场景"文件上传 + 病毒扫描（服务未选型）"。验证 spec 的验收标准落到 task 完成定义、TBD 对应 spike、依赖方向单向。5 项衔接验证全过。
- **三环**（clarifying → spec → task，2026-06-22）：场景"内部知识库"。验证 clarifying 的硬问题被 spec 继承、spec 的验收标准被 task 映射、三环方向零漂移。5 项衔接验证全过。

两次串跑共同证明：三个 skill 的时机判断逻辑互相一致（都正确识别"现在不该硬写 spec / 不该硬拆"），衔接契约成立。

---

## 6. 何时该用单个 skill，何时该走流水线

不是所有任务都要走完整三环。判断：

- **小改动 / 明确需求** → 直接做，最多用 commit-message。
- **中等需求（方向明但有设计决策）** → spec-writing 单独用，或 spec-writing → task-breakdown 两环。
- **大需求 / 模糊需求 / 高风险方案** → 完整三环 clarifying → spec → task。
- **接手陌生代码库** → onboarding-unknown-codebase，与主链解耦。

误用流水线和误用单个 skill 一样有害——小需求套三环是流程负担，大需求跳过澄清是返工源头。

---

## 7. 验收标准（这份文档自身）

按 spec-writing 规范，定义"这份文档怎样算成功"：

- 读者能在 5 分钟内看清 8 个 skill 的关系，不必读完 8 个 SKILL.md 才理解协作
- 流水线主链（3 skill）+ 横切（4 skill）+ 元（1 skill）的分层清晰
- 衔接契约（上下游产出对齐）显式写出，不是隐含
- 已验证的协作有证据引用（提交 hash + 验证场景）
- 文档自身符合 spec-writing 的精简原则（决策为主、不堆背景）

---

## 8. 后续

- **新增 skill 时**：按本文档第 1/2 节的分类判断它属于主链 / 横切 / 元，并更新对应分类表。
- **新增 skill 后**：跑 `lion-writing-skills` 的轻量验证流程（至少 3 轮），若属主链/横切与已有 skill 有衔接，需跑跨 skill 一致性检查 + 必要时流水线串跑。
- **本文档滞后时**：以各 SKILL.md 为真相（与 `2026-06-16-lion-skills-design.md` §9 的处理策略一致），本文档回填。
