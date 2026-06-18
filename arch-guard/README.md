# Arch Guard

harness 里**横跨整个项目的架构师/重构师** skill。守护项目的架构事实源——一份活的
`ARCHITECTURE.md`(数据模型 / 模块边界 / 不变量 / 扩展点)。feature-by-feature 推进时,
新功能常和旧数据结构/设计撞车被迫 refactor;arch-guard 通读代码 + harness 进度文件诊断
根因,产出**架构调整方案**交 Planner 落地。它**只定结构与策略**,不写代码、不写逐行计划
(那是 Planner),也不做单功能勘探(那是 Explorer)。

## 解决的根因

feature-by-feature 没有全局架构事实源 → 不兼容要到开发时才暴露。活的 `ARCHITECTURE.md`
让 Planner/Game Designer 开新功能**之前**就能对照,把撞车提前拦下。

## 两种模式 + 一个前置

- **A 开局架构设计**(全新项目):交互式讨论,和你一起定数据模型/边界/不变量,产出 `ARCHITECTURE.md` v1。
- **B 重构顾问**(已有 `ARCHITECTURE.md` 的进行中项目):诊断不兼容根因,产出
  `harness/arch/REFACTOR-<NN>-<slug>.md`,并把 `ARCHITECTURE.md` 更新到目标形态,交 Planner。
- **逆推前置**(已在开发、但从没建过 `ARCHITECTURE.md`):先通读代码 + 文档逆推出反映现状的
  `ARCHITECTURE.md` v1(标注"逆推自现有代码"),再据此进入模式 B(若要重构)或停在补档。
  这条对应 feature-by-feature 裸跑起来、事后才想要架构事实源的常见场景。

## 在管线里的位置

```
新项目:  /arch-guard → ARCHITECTURE.md（事实源,常驻）
重构时:  新功能装不下 → /arch-guard <slug> → REFACTOR-<NN>-<slug>.md + 更新 ARCHITECTURE.md
                → /role-planner <slug> 落成具体重构 PLAN → /role-implementer …
预防:    Planner / Game Designer 开功能前对照 ARCHITECTURE.md,冲突则 STOP 转 /arch-guard
```

- 上游:`project-context.md`、`BACKLOG.md`、各 feature 的 `PLAN/HANDOFF/CHANGES`、真实代码 `src/`。
- 产物:`harness/ARCHITECTURE.md`(常驻)+ `harness/arch/REFACTOR-<NN>-<slug>.md`(frontmatter `next: Planner`)。
- 下游:Planner 读 REFACTOR 文档落成有序、可验证、到文件级的重构 PLAN。

## 产物结构

`ARCHITECTURE.md`(六节):架构一句话 / 数据模型 / 模块边界与依赖 / 关键不变量与约定 /
扩展点 / 已知张力与债。

`REFACTOR-<NN>-<slug>.md`(七节):触发 / 现状诊断(根因)/ 目标形态(对 ARCHITECTURE 的
delta)/ 调整策略(按依赖序、策略级)/ 影响面与迁移 / 风险与被否选项 / 交接 Planner。

## 文件(原生 Skill 格式,自包含)

```
arch-guard/
  README.md       # 本文件
  SKILL.md        # skill 本体:frontmatter(name/description)+ 完整契约,正文内联、无绝对路径
```

## 怎么用

```
/arch-guard                          # 新项目:交互式定架构,产出 ARCHITECTURE.md
/arch-guard 05-inventory 可堆叠背包重构   # 进行中:诊断不兼容,产 REFACTOR 文档交 Planner
```

## 安装(让 /arch-guard 可用)

```
~/.claude/skills/arch-guard/SKILL.md
```

把本目录拷过去即可;之后 `/arch-guard` 自动可用。

## 与 harness 的关系

`ARCHITECTURE.md` 是给 `../../Claude_Roles/` 管线用的架构事实源。预防回路需要在
`roles/engineering/planner.md`(及 `roles/direction/game-designer.md`)里把 ARCHITECTURE.md
列为必读输入、并加"冲突则转 /arch-guard"的 escalation——这部分在 Claude_Roles 仓库里改。
