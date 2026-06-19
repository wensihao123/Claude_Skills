# Skills — 独立 Claude Code skill 集

一组**自包含、可移植**的 Claude Code skill,每个 skill 一个子目录(`README.md` + `SKILL.md`)。
与 role harness(`../Claude_Roles/`)**解耦**:role 规范留在 harness 仓库,
通用/前置型 + 项目级事实源守护型 skill 收在这里,方便单独维护,后续整体迁进专门的 skills GitHub 仓库。

## 为什么单独放

- role 是「一个 session 扮一个固定 role、靠 artifact 协作」的重型规范,绑定 harness。
- skill 更轻、更通用,可能服务多个项目,甚至与 harness 无关。
- 拆开后:harness 仓库保持纯粹,skill 这边可以独立 git 化、独立分发(plugin marketplace)。

## 两类 skill

- **前置 / 编译型**(design-jam、image-prompt):在管线某个接缝上做一次性转换——把粗想法磨成
  `IDEA.md`、把 `ASSET-SPEC.md` 编译成成品生图 prompt。轻、无常驻状态。
- **项目级事实源守护型**(arch-guard、num-smith、ux-design、state-machine-master):各自守护一份
  **活的标准文档**(`ARCHITECTURE.md` / `BALANCE.md` / `UX-MAP.md` / `STATE-MACHINES.md`),共用同一套
  范式——**两种模式 + 一个前置**:A 开局交互式设计、B 顾问诊断产出 `<KIND>-CHANGE-<NN>.md` 调整方案、
  逆推前置(存量项目无文档时先通读代码逆推出反映现状的 v1)。它们还接进 harness 的**预防回路**:
  Game Designer / Planner / Art Spec 开功能前先对照这些事实源,冲突就 STOP 转对应 skill,把问题在
  开发前拦下(接线在 `../Claude_Roles/roles/` 里)。

## 子项目目录

> 新增 skill 时在此登记一行(目录名 / 调用 / 类型 / 一句话)。

| 子项目 | 调用 | 类型 | 一句话 |
|--------|------|------|--------|
| [design-jam](design-jam/README.md) | `/design-jam` | 前置/编译 | 跟你对话把粗想法磨成 `IDEA.md`,交给 Game Designer |
| [image-prompt](image-prompt/README.md) | `/image-prompt` | 前置/编译 | 把 art-spec 的 `ASSET-SPEC.md` 编译成 image2 成品 prompt,产出 `IMAGE-PROMPTS.md` |
| [arch-guard](arch-guard/README.md) | `/arch-guard` | 事实源守护 | 维护活的 `ARCHITECTURE.md`,诊断不兼容并产出 `REFACTOR-<NN>.md` 重构方案交 Planner |
| [num-smith](num-smith/README.md) | `/num-smith` | 事实源守护 | 维护活的 `BALANCE.md`,诊断数值失衡并产出 `BALANCE-CHANGE-<NN>.md` 调整方案交 Planner/Implementer |
| [ux-design](ux-design/README.md) | `/ux-design` | 事实源守护 | 维护活的 `UX-MAP.md`(屏幕与状态地图),诊断交互缺口并产出 `UX-CHANGE-<NN>.md` 方案交 Planner(视觉转 Art Spec) |
| [state-machine-master](state-machine-master/README.md) | `/state-machine-master` | 事实源守护 | 维护活的 `STATE-MACHINES.md`,诊断状态混乱并产出 `STATE-CHANGE-<NN>.md` 方案交 Planner(需新结构转 arch-guard) |

## 目录结构

每个 skill 一个目录,统一是原生 Skill 格式(`README.md` 说明 + `SKILL.md` 本体):

```
Skills/
  README.md                  # 本文件
  design-jam/                # 前置/编译型
  image-prompt/              # 前置/编译型
  arch-guard/                # 事实源守护型(ARCHITECTURE.md)
  num-smith/                 # 事实源守护型(BALANCE.md)
  ux-design/                 # 事实源守护型(UX-MAP.md)
  state-machine-master/      # 事实源守护型(STATE-MACHINES.md)
    README.md                # 该 skill 的说明
    SKILL.md                 # skill 本体:frontmatter(name/description)+ 自包含正文
```

## 怎么注入 Claude Code(三种方式)

1. **原生 Skill**(可移植、推荐,本仓库全部 skill 均采用)——`~/.claude/skills/<name>/SKILL.md`,
   带 `name`/`description` frontmatter,正文自包含、无绝对路径,能被 Skill 工具自动发现。整个文件夹可原样拷走。
   本仓库里每个子目录就是一个这样的 skill,安装时拷到 `~/.claude/skills/` 即可:

   ```
   cp -r <name>/ ~/.claude/skills/<name>/        # 之后 /<name> 自动可用
   diff -q <name>/SKILL.md ~/.claude/skills/<name>/SKILL.md   # 改动后校验已同步
   ```

   注意:`~/.claude/skills/` 是**安装副本**,本仓库才是源;改了源记得重拷一遍再 `diff -q` 核对。
2. **slash 命令**(最省事,但不可移植)——`~/.claude/commands/<name>.md`,`/name` 即可调用;
   适合一次性、只在本机用的小命令,不适合分发。
3. **Plugin Marketplace**(正式分发)——把本仓库做成 GitHub 仓库,加
   `.claude-plugin/marketplace.json` 清单,别人 `/plugin marketplace add <repo>` + `/plugin install` 安装。

## 路线

- [x] 从 harness 仓库拆出,落在 `Skills/`
- [x] 全部 skill 转成自包含 `SKILL.md`(去掉绝对路径依赖)
- [x] 事实源守护型 skill(arch-guard / num-smith / ux-design / state-machine-master)接进 harness 预防回路
- [ ] 建独立 GitHub 仓库 + plugin marketplace 清单,支持 `/plugin install`
