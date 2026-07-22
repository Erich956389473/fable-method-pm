# Fable Method for PMs — 项目配置

## 项目概述

Claude Code Skills 项目，为产品管理者提供自适应 AI 思考框架（Think/Act/Prove）。

## 技能结构

- `skills/pm-method.md` — 思考模块（中文）
- `skills/pm-loop.md` — 执行模块（中文）
- `skills/pm-judge.md` — 验证模块（中文）
- `skills/en/` — 英文版

## 数据流

```
pm-method → pm-loop → pm-judge
                ↑           │
                └───────────┘  (失败回退)
```

## 开发规范

- 所有内容使用中文（英文版在 `skills/en/` 目录）
- 技能文件修改时，中英文版需同步更新
- 参数格式统一为：`（可选，默认: X，可选值: a / b / c）`
- 每个技能必须包含：输入参数、参数校验规则、自适应逻辑、输出格式
