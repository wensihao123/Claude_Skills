# State Machine Master

harness 里**横跨整个项目的状态管理师** skill,与 `../arch-guard/` 配套。守护项目的状态机事实源
——一份活的 `STATE-MACHINES.md`(状态机清单 / 每台机的状态与转移 / FSM 基础设施与约定 / 不变量)。
feature-by-feature 推进时,战斗流程/敌人 AI/玩家控制/界面流程的状态管理常散成一堆布尔标志、互相
打架、加一个状态要改十处;state-machine-master 通读代码 + harness 进度文件诊断根因,产出**状态机
调整方案**交 Planner 落地。它**只定动态行为与状态结构**,不写代码(那是 Planner)、不定数据模型/
模块边界(那是 arch-guard)、不画玩家界面(那是 ux-design)。

## 解决的根因

feature-by-feature 把"现在处于什么状态、能转到哪"用散落的布尔变量隐式表达 → 状态爆炸、非法组合、
扩充时步步踩雷。活的 `STATE-MACHINES.md` 把每台机的状态/事件/转移显式列出,让 Planner 开新功能
**之前**就能对照该往哪台机加哪个状态,把混乱提前拦下。

## 与 arch-guard 的边界(配套不重叠)

- **arch-guard 管静态结构**:数据模型、模块边界、依赖方向(系统**是**什么、谁依赖谁)。
- **state-machine-master 管动态行为**:状态、事件、转移、守卫(系统在时间里**怎么变**)。
- 当一台状态机需要**新模块/新数据结构**才能装下,state-machine-master **STOP 转 /arch-guard**
  定结构,再回来定转移。

## 与 ux-design 的关系

`../ux-design/` 画玩家会看到的屏与跳转(UX-MAP.md §2);state-machine-master 把它实现成
**app/game 流程 FSM**(Boot→Title→Game→Pause→GameOver),并额外负责所有**玩法状态机**(战斗
阶段、玩家控制器、敌人 AI)。两份文档互相引用、保持一致。

## 两种模式 + 一个前置

- **A 开局状态机设计**(全新项目):交互式讨论,和你一起定有哪几台机、各自状态与转移、FSM 范式,产出 `STATE-MACHINES.md` v1。
- **B 状态顾问**(已有 `STATE-MACHINES.md` 的进行中项目):诊断状态混乱根因,产出
  `harness/state/STATE-CHANGE-<NN>-<slug>.md`,并把 `STATE-MACHINES.md` 更新到目标形态,交 Planner。
- **逆推前置**(已在开发、但从没建过 `STATE-MACHINES.md`):先通读代码逆推出反映现状的
  `STATE-MACHINES.md` v1(标注"逆推自现有代码",把"靠布尔标志硬撑"记进已知状态债),再据此进入模式 B 或停在补档。

## 在管线里的位置

```
新项目:  /state-machine-master → STATE-MACHINES.md（状态机事实源,常驻）
重构时:  状态乱/难扩充 → /state-machine-master <slug> → STATE-CHANGE-<NN>-<slug>.md + 更新 STATE-MACHINES.md
                → /role-planner <slug> 落成具体 PLAN → /role-implementer …（需新结构先转 /arch-guard）
预防:    Planner 开功能前对照 STATE-MACHINES.md,状态冲突则 STOP 转 /state-machine-master
```

- 上游:`project-context.md`、`ARCHITECTURE.md`、`UX-MAP.md`、各 feature 的 `PLAN/HANDOFF/CHANGES`、真实代码 `src/`。
- 产物:`harness/STATE-MACHINES.md`(常驻)+ `harness/state/STATE-CHANGE-<NN>-<slug>.md`(frontmatter `next: Planner`)。
- 下游:Planner 落成有序、可验证、到文件级的 PLAN;需新模块/数据结构时 arch-guard 先动。

## 产物结构

`STATE-MACHINES.md`(六节):状态管理一句话 / 状态机清单 / 每台机的状态与转移(核心)/
FSM 基础设施与约定 / 关键不变量 / 已知状态债。

`STATE-CHANGE-<NN>-<slug>.md`(七节):触发 / 现状诊断(根因)/ 目标形态(对 STATE-MACHINES 的
delta)/ 调整策略(按依赖序、策略级)/ 影响面与迁移(含是否需 arch-guard 先动)/ 风险与被否选项 / 交接。

## 文件(原生 Skill 格式,自包含)

```
state-machine-master/
  README.md       # 本文件
  SKILL.md        # skill 本体:frontmatter(name/description)+ 完整契约,正文内联、无绝对路径
```

## 怎么用

```
/state-machine-master                              # 新项目:交互式定状态机,产出 STATE-MACHINES.md
/state-machine-master 04-combat 战斗靠布尔标志硬撑要抽离   # 进行中:诊断,产 STATE-CHANGE 文档交 Planner
```

## 安装(让 /state-machine-master 可用)

```
~/.claude/skills/state-machine-master/SKILL.md
```

把本目录拷过去即可;之后 `/state-machine-master` 自动可用。

## 与 harness 的关系

`STATE-MACHINES.md` 是给 `../../Claude_Roles/` 管线用的状态机事实源。预防回路需要在
`roles/engineering/planner.md` 里把 STATE-MACHINES.md 列为必读输入、并加"状态冲突则转
/state-machine-master"的 escalation——这部分在 Claude_Roles 仓库里改。
