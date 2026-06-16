# Design Jam

harness 的**入口前门** skill。跟你一问一答,把一个粗想法("如果玩家能……")
磨成一份粗粒度的 `IDEA.md`,交给 Game Designer 细化。它负责**打开设计空间**、
把没想清的部分诚实地留成 Open threads,而不是逼自己当场定稿。

## 在管线里的位置

```
/design-jam → IDEA.md → /role-game-designer <slug> → FEATURE-DESIGN.md → /role-planner …
```

- 上游:你脑子里的一个 spark。
- 产物:`harness/features/<NN-slug>/IDEA.md`(frontmatter `next: Game Designer`)。
- 下游:Game Designer 读 `IDEA.md`,逐条收敛 Open threads,产出 `FEATURE-DESIGN.md`。
- IDEA.md 是**可选**上游——Game Designer 是项目入口,小功能也可以不经 design-jam 直接跟它谈。

## 文件(原生 Skill 格式,自包含)

```
design-jam/
  README.md       # 本文件
  SKILL.md        # skill 本体:frontmatter(name/description)+ 完整契约,正文内联、无绝对路径
```

`SKILL.md` 自包含:不再依赖外部规范文件或命令壳,可原样拷进任何机器 / GitHub 仓库。

## 怎么用

```
/design-jam                      # 直接开始 jam,过程中和你商定 slug
/design-jam 03-wall-jump 蹬墙跳   # 带 slug + 起始 spark
```

skill 会:读 `harness/project-context.md`(对齐支柱,缺失则照常 jam 但 flag)→
交互式 jam(一次问一个重点)→ 分清「已定」与「未决」→ 写 `IDEA.md` +
seed/更新该功能 `HANDOFF.md`,把"下一步"设为 `/role-game-designer <slug>`。

## 产物:IDEA.md 八节

1. 一句话 / One-liner
2. 玩家体验 / Fantasy
3. 核心循环(初稿) / Core loop (draft)
4. 关键规则与状态 / Key rules & states
5. 反馈 / juice 意图
6. 最小版本 / Minimal version
7. 与游戏支柱的关系 / Pillar fit
8. **Open threads / 未决问题** ← 故意留给 Game Designer 收敛的洞

> 设计原则:粗,不要定稿;一定要留诚实的 Open threads——一份「什么都解决了」的 jam 是坏味道。

## 安装(让 /design-jam 可用)

`SKILL.md` 要被 Claude Code 发现,需放到全局 skills 目录:

```
~/.claude/skills/design-jam/SKILL.md
```

把本目录拷过去(或建符号链接)即可;之后 `/design-jam` 自动可用。
将来本仓库做成 plugin marketplace 后,改用 `/plugin install` 安装,无需手工拷贝。

## 与 harness 的关系

产出的 `IDEA.md` 接进 `../../Claude_Roles/` 的 role harness;seed HANDOFF 时引用的是
**游戏项目内**的 `harness/templates/HANDOFF.md`,不依赖本 skill 仓库。
