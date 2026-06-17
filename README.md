# Skills — 独立 Claude Code skill 集

一组**自包含、可移植**的 Claude Code skill,每个 skill 一个子目录。
与 role harness(`../Claude_Roles/`)**解耦**:role 规范留在 harness 仓库,
通用/前置型 skill 收在这里,方便单独维护,后续整体迁进专门的 skills GitHub 仓库。

## 为什么单独放

- role 是「一个 session 扮一个固定 role、靠 artifact 协作」的重型规范,绑定 harness。
- skill 更轻、更通用,可能服务多个项目,甚至与 harness 无关。
- 拆开后:harness 仓库保持纯粹,skill 这边可以独立 git 化、独立分发(plugin marketplace)。

## 子项目目录

> 新增 skill 时在此登记一行(目录名 / 调用 / 一句话)。

| 子项目 | 调用 | 一句话 |
|--------|------|--------|
| [design-jam](design-jam/README.md) | `/design-jam` | 跟你对话把粗想法磨成 `IDEA.md`,交给 Game Designer |
| [image-prompt](image-prompt/README.md) | `/image-prompt` | 把 art-spec 的 `ASSET-SPEC.md` 编译成 image2 成品 prompt,产出 `IMAGE-PROMPTS.md` |

## 目录结构

```
Skills/
  README.md            # 本文件
  design-jam/          # 每个 skill 一个目录(原生 Skill 格式)
    README.md          # 该 skill 的说明
    SKILL.md           # skill 本体:frontmatter + 自包含正文
```

## 怎么注入 Claude Code(三种方式)

1. **原生 Skill**(可移植、推荐,design-jam 已采用)——`~/.claude/skills/<name>/SKILL.md`,
   带 `name`/`description` frontmatter,正文自包含,能被 Skill 工具自动发现。整个文件夹可原样拷走。
   本仓库里每个子目录就是一个这样的 skill,安装时拷到 `~/.claude/skills/` 即可。
2. **slash 命令**(最省事,但不可移植)——`~/.claude/commands/<name>.md`,`/name` 即可调用;
   适合一次性、只在本机用的小命令,不适合分发。
3. **Plugin Marketplace**(正式分发)——把本仓库做成 GitHub 仓库,加
   `.claude-plugin/marketplace.json` 清单,别人 `/plugin marketplace add <repo>` + `/plugin install` 安装。

## 路线

- [x] 从 harness 仓库拆出,落在 `Skills/`
- [x] design-jam 转成自包含 `SKILL.md`(去掉绝对路径依赖)
- [ ] 建独立 GitHub 仓库 + plugin marketplace 清单,支持 `/plugin install`
