# UX Design

harness 里**横跨整个项目的交互设计师** skill。守护项目的交互事实源——一份活的
`UX-MAP.md`(屏幕与状态地图 / 每屏交互状态 / 输入·导航·反馈约定与不变量)。feature-by-feature
推进时常只顾把玩法跑起来,菜单/暂停/结算等界面从没设计、游戏直接开打;ux-design 通读代码 +
harness 进度文件诊断根因,产出**交互调整方案**交 Planner 落地。它**只定交互结构与流程**,不画
像素/资源(那是 Art Spec)、不定玩法手感/规则(那是 Game Designer)、不写代码(那是 Planner)、
也不实现底层流程状态机(那是 state-machine-master,UX 给它屏幕地图去实现)。

## 解决的根因

feature-by-feature 没把界面流程当整体设计过 → "直接开打、没有任何界面交互"。活的 `UX-MAP.md`
让 Planner/Game Designer 开新功能**之前**就能对照玩家会经过哪些屏、每屏有哪些交互状态,把缺口提前拦下。

## 两种模式 + 一个前置

- **A 开局交互设计**(全新项目):交互式讨论,和你一起定屏幕地图/导航/每屏状态/输入约定,产出 `UX-MAP.md` v1。
- **B 交互顾问**(已有 `UX-MAP.md` 的进行中项目):诊断交互缺口/冲突根因,产出
  `harness/ux/UX-CHANGE-<NN>-<slug>.md`,并把 `UX-MAP.md` 更新到目标形态,交 Planner。
- **逆推前置**(已在开发、但从没建过 `UX-MAP.md`):先通读代码逆推出反映现状的 `UX-MAP.md` v1
  (把"直接开打、缺菜单"记进已知交互债),再据此进入模式 B(若要补界面)或停在补档。
  这条对应裸跑起来、事后才发现界面全缺的常见场景。

## 在管线里的位置

```
新项目:  /ux-design → UX-MAP.md（交互事实源,常驻）
补界面:  缺菜单/缺状态 → /ux-design <slug> → UX-CHANGE-<NN>-<slug>.md + 更新 UX-MAP.md
                → /role-planner <slug> 落成 PLAN → /role-implementer …(视觉细节转 /role-art-spec)
预防:    Planner / Game Designer 开功能前对照 UX-MAP.md,缺界面/冲突则 STOP 转 /ux-design
```

- 上游:`project-context.md`、`FEATURE-DESIGN.md`、`BACKLOG.md`、`STATE-MACHINES.md`、真实代码 `src/`。
- 产物:`harness/UX-MAP.md`(常驻)+ `harness/ux/UX-CHANGE-<NN>-<slug>.md`(frontmatter `next: Planner`)。
- 下游:Planner 落成 PLAN;Art Spec 出视觉;state-machine-master 把 §2 屏幕地图实现成流程 FSM。

## 产物结构

`UX-MAP.md`(六节):交互一句话(+主输入设备)/ 屏幕与状态地图(核心)/ 每屏职责与交互状态 /
输入·导航·反馈约定与不变量 / 信息架构·HUD·可达性 / 已知交互债。

`UX-CHANGE-<NN>-<slug>.md`(七节):触发 / 现状诊断(根因)/ 目标形态(对 UX-MAP 的 delta)/
调整策略(按依赖序、策略级)/ 影响面(含 STATE-MACHINES/Art Spec 要补的)/ 风险与被否选项 / 交接。

## 与相邻 skill 的边界

- **Art Spec**:UX 定"这里要一个确认态",Art Spec 画它长什么样(像素/字体/配色/资源)。
- **Game Designer**:UX 定"技能选择屏怎么导航",Game Designer 定技能本身强不强。
- **state-machine-master**:UX 画玩家会看到的屏与跳转,state-machine-master 实现成 app/game
  流程 FSM(Boot→Title→Game→Pause→GameOver)。两份文档互相引用、保持一致。

## 文件(原生 Skill 格式,自包含)

```
ux-design/
  README.md       # 本文件
  SKILL.md        # skill 本体:frontmatter(name/description)+ 完整契约,正文内联、无绝对路径
```

## 怎么用

```
/ux-design                          # 新项目:交互式定流程,产出 UX-MAP.md
/ux-design 03-pause-menu 补暂停与结算界面   # 进行中:诊断缺口,产 UX-CHANGE 文档交 Planner
```

## 安装(让 /ux-design 可用)

```
~/.claude/skills/ux-design/SKILL.md
```

把本目录拷过去即可;之后 `/ux-design` 自动可用。

## 与 harness 的关系

`UX-MAP.md` 是给 `../../Claude_Roles/` 管线用的交互事实源。预防回路需要在
`roles/direction/game-designer.md`(及 `roles/production/art-spec.md`)里把 UX-MAP.md 列为
必读输入、并加"缺界面/冲突则转 /ux-design"的 escalation——这部分在 Claude_Roles 仓库里改。
