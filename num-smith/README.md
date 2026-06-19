# Num Smith

harness 里**横跨整个项目的数值策划/平衡师** skill。守护项目的数值事实源——一份活的
`BALANCE.md`(数值哲学 / 核心属性与单位 / 公式与曲线 / 经济收支 / 关键常量与不变量)。
feature-by-feature 推进时,数字越堆越多、彼此失配,失衡常到玩起来才暴露;num-smith 通读代码 +
harness 进度文件诊断根因,产出**数值调整方案**交 Planner/Implementer 落地。它**只定数值与策略**,
不写代码、不写逐行计划(那是 Planner/Implementer),也不定玩家意图(那是 Game Designer)。

## 解决的根因

feature-by-feature 没有全局数值事实源 → 失衡要到玩起来才暴露。活的 `BALANCE.md` 让
Game Designer/Planner 开新功能**之前**就能对照,把破坏平衡的数值提前拦下。

## 和 Game Designer 的分工

Game Designer 出**意图**(「这里要爽」「成长要有回报」「Boss 要有压迫感」);num-smith 拥有
**全部具体数字**——把意图翻成公式 / 曲线 / 经济 / 常量,并守护全局平衡一致性。GD 碰到数字问题
escalate 给 num-smith。

## 两种模式 + 一个前置

- **A 开局数值框架**(全新项目):交互式讨论,和你一起定数值哲学/属性/公式形态/经济结构/不变量,
  产出 `BALANCE.md` v1。不给还没做的内容硬定具体值。
- **B 平衡顾问**(已有 `BALANCE.md` 的进行中项目):诊断失衡根因,产出
  `harness/balance/BALANCE-CHANGE-<NN>-<slug>.md`,并把 `BALANCE.md` 更新到目标形态,交下游。
- **逆推前置**(已在开发、但从没建过 `BALANCE.md`):先通读代码 + 文档逆推出反映现状的
  `BALANCE.md` v1(标注"逆推自现有代码"),再据此进入模式 B(若要调平衡)或停在补档。
  这条对应 feature-by-feature 裸跑起来、事后才想要数值事实源的常见场景。

## 在管线里的位置

```
新项目:  /num-smith → BALANCE.md（事实源,常驻）
失衡时:  数值撞车/失衡 → /num-smith <slug> → BALANCE-CHANGE-<NN>-<slug>.md + 更新 BALANCE.md
                → /role-planner <slug>（结构性）或 /role-implementer <slug>（纯微调）落地
预防:    Game Designer / Planner 开功能前对照 BALANCE.md,破坏不变量则 STOP 转 /num-smith
```

- 上游:`project-context.md`、`BACKLOG.md`、各 feature 的 `FEATURE-DESIGN/PLAN/HANDOFF/CHANGES`、真实代码 `src/`。
- 产物:`harness/BALANCE.md`(常驻)+ `harness/balance/BALANCE-CHANGE-<NN>-<slug>.md`(frontmatter `next: Planner` 或 `Implementer`)。
- 下游:结构性数值改动交 Planner 落成有序可验证 PLAN;纯常量微调可直接交 Implementer。

## 产物结构

`BALANCE.md`(六节):数值一句话 / 核心属性与单位 / 公式与曲线 / 经济收支 / 关键调参常量与平衡
不变量 / 已知失衡与债。**系统级框架 + 关键常量/不变量**;每个实体的具体数值表留在 per-feature。

`BALANCE-CHANGE-<NN>-<slug>.md`(七节):触发 / 现状诊断(根因)/ 目标数值(对 BALANCE 的 delta)/
调整策略(按依赖序:锚→派生)/ 影响面与迁移 / 风险与被否选项 / 交接。

## 文件(原生 Skill 格式,自包含)

```
num-smith/
  README.md       # 本文件
  SKILL.md        # skill 本体:frontmatter(name/description)+ 完整契约,正文内联、无绝对路径
```

## 怎么用

```
/num-smith                              # 新项目:交互式定数值框架,产出 BALANCE.md
/num-smith 07-boss-rush 后期 Boss 被秒    # 进行中:诊断失衡,产 BALANCE-CHANGE 文档交下游
```

## 安装(让 /num-smith 可用)

```
~/.claude/skills/num-smith/SKILL.md
```

把本目录拷过去即可;之后 `/num-smith` 自动可用。

## 与 harness 的关系

`BALANCE.md` 是给 `../../Claude_Roles/` 管线用的数值事实源,和 arch-guard 的 `ARCHITECTURE.md`
同构(一个守结构,一个守数值平衡)。预防回路已接:`roles/direction/game-designer.md` 和
`roles/engineering/planner.md` 都把 `BALANCE.md` 列为必读输入(若存在),并在 Check 步 + escalation
里加了"功能/目标破坏平衡不变量或数值哲学时 STOP 转 /num-smith"。这部分在 Claude_Roles 仓库里。
