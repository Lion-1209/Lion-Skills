---
name: naming
description: 当用户需要为代码标识符决定名字时使用——变量、函数、方法、类、文件、模块、API、数据库字段、配置项等。典型信号：用户说"这个变量/函数叫啥名好""这个名字合适吗""帮我起个名""命名规范""这个 API 怎么命名"、写代码时卡在取名、觉得现有命名不准或误导想改、想统一一个模块的命名风格。涉及关键词：命名、取名、起名、变量名、函数名、类名、文件名、API 命名、identifier、naming convention、命名约定、命名风格。
---

# Naming

## 概述

名字要回答"它是什么/它做什么"，而不是"它在哪/什么类型"。好名字让代码自解释，坏名字逼读者来回跳转确认含义。

## 何时使用

- 写代码时卡在取某个名字上
- 觉得现有名字不准、误导，想改
- 设计 API/接口，定资源和字段名
- 想统一一个模块的命名风格

**不该用**：已在用团队既定规范（遵循即可）；纯格式问题（用 linter）。

## 核心内容

### 决策四问

取名前问：
1. **它代表什么？**（数据/实体）→ 名词
2. **它做什么？**（动作）→ 动词
3. **谁会用它？**（读者上下文）→ 用领域熟悉的词
4. **有歧义吗？**（同名异义/异名同义）→ 消除

### 快速规则

- **函数/方法用动词**：`fetchUser`、`validateEmail`
- **变量/属性用名词**：`user`、`emailAddress`
- **布尔用 is/has/can/should**：`isLoggedIn`、`hasPermission`
- **避免否定**：`isEmpty` 优于 `isNotEmpty`
- **避免无意义词**：`data`、`info`、`helper`、`manager`、`util`（说不清是啥）
- **少缩写**：除非领域通用（`id`、`url`、`http`，以及 `csv`、`json`、`xml` 这类格式名）；`usr`、`cfg`、`tmp` 别用
- **范围越窄越具体**：模块内可简短，全局/API 要自解释
- **一致**：同一概念全项目用同一个词（别 `user`/`account`/`member` 混用）

### before / after

| 不好 | 好 | 原因 |
|------|-----|------|
| `processData(d)` | `parseCsvToRecords(text)` | 说清做什么、入参是啥 |
| `flag` | `isLoggedIn` | 布尔用 is，表意明确 |
| `userList` | `users` | 复数即集合，去冗余类型后缀 |
| `strName` | `name` | 去掉匈牙利记法前缀 |
| `handleStuff()` | `sendInvoice()` | 动词具体，不抽象 |
| `UserInfoManager` | `UserProfile` | 去掉无意义 Manager |

### API 命名

- 资源用复数名词：`/users`、`/orders`
- 嵌套表达从属：`/users/{id}/orders/{oid}/items`
- 字段 case 跟随语言：JS camelCase、Python/DB snake_case
- 动作用 HTTP 方法表达，别塞进 URL：`DELETE /users/123` 而非 `/users/123/delete`

## 常见错误

| 问题 | 修法 |
|------|------|
| 缩写成谜（`usrCfg`） | 写全，除非领域通用缩写 |
| 同概念多名（user/account/member） | 全项目统一一个 |
| 匈牙利记法（`strName`、`intAge`） | 去掉类型前缀 |
| 过度抽象（Manager/Handler/Util） | 用具体名词 |
| 否定布尔（isNotValid） | 改肯定（isValid） |
