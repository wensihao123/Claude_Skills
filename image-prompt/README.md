# Image Prompt

harness 里 **art-spec 的下游 prompt 编译器** skill。读 art-spec 产出的
`ASSET-SPEC.md`(每个 asset 的 prompt brief + 风格约束)和 `STYLE-BIBLE.md`,把每个
asset 编译成**可直接粘给 image2(GPT 生图模型)的英文成品 prompt**,产出
`IMAGE-PROMPTS.md`,交给人去生图、再交 Engine Integrator 导入。它**只做编译**——不发明
新美术方向(那是 art-spec),也不管引擎导入(那是 Engine Integrator)。

## 在管线里的位置

```
/role-art-spec <slug> → ASSET-SPEC.md(prompt brief)
        → /image-prompt <slug> → IMAGE-PROMPTS.md(成品 prompt)
        → 人去 image2 生图 → /role-engine-integrator-godot <slug> 导入(或 /role-art-spec 验收)
```

- 上游:`ASSET-SPEC.md`(**必需**,含 prompt brief)+ `STYLE-BIBLE.md`(风格锚)。
- 产物:`harness/features/<NN-slug>/IMAGE-PROMPTS.md`(frontmatter `next: Engine Integrator`)。
- 下游:人拿成品 prompt 去 image2 生图,回来交 Engine Integrator 导入。

## 文件(原生 Skill 格式,自包含)

```
image-prompt/
  README.md       # 本文件
  SKILL.md        # skill 本体:frontmatter(name/description)+ 完整契约,正文内联、无绝对路径
```

## 怎么用

```
/image-prompt 01-double-jump            # 编译该 feature 的所有 asset prompt
/image-prompt 01-double-jump 只编译 UI    # slug 后可跟自由指令
```

skill 会:读 `ASSET-SPEC.md`(缺失则 STOP,提示先开 `/role-art-spec`)+ `STYLE-BIBLE.md`
+ `style-basic-2d.md` + `project-context.md` + `IMAGE-PROMPT-PREFIX.md` → 逐个 asset
编译成「四段前缀 + 具体要求」的英文成品 prompt + 画布尺寸 + Post-gen 处理 → 写
`IMAGE-PROMPTS.md` + 更新 HANDOFF。

## 核心纪律:四段前缀(保证同项目风格统一)

每条 prompt 都是 **固定项目级四段前缀 + 本次具体生图要求**,前缀永远在前:

1. **风格锚定** — 项目风格要求(取自 STYLE-BIBLE.md)。
2. **参考锚定** — 项目参考的知名 IP/原型(大众熟知,给 image2 一个共同心智模型)。
3. **技术约束** — 输出规范(image2 固定画布、分辨率、aspect ratio;单 asset 可覆盖)。
4. **排除项**(很重要)— 内置「asset 大类 → 默认禁止项」表(道具→禁人物;技能特效→禁人物+背景…)
   + 该 asset 的具体排除项接在后面。image2 不吃 negative,故全用正面自然语言表达。

前 3 段在项目内**一模一样**,由 skill 首次运行时生成并锁进标准文件
`harness/IMAGE-PROMPT-PREFIX.md`(参考锚定缺失时问你一次),之后逐字复用——这就是"稳定"的来源。

## image2(gpt-image)硬约束(skill 会替你处理)

- 只出固定画布尺寸(1024×1024 / 1024×1536 / 1536×1024)→ 小图/像素图"大图生成→下采样"。
- 透明背景必须显式要求。
- 不吃权重语法、不吃 negative prompt → "要避免什么"用正面自然语言表达。
- 强于文字渲染与指令遵循 → 带字/UI asset 善用。

> ⚠️ 骨架阶段:SKILL.md 里 `<compile_rules>` 的具体编译范式(句式模板、风格词典、各类
> asset 取景套路、批量/变体策略)还是初稿,待继续细化;`<example>` 是占位块。

## 安装(让 /image-prompt 可用)

```
~/.claude/skills/image-prompt/SKILL.md
```

把本目录拷过去即可;之后 `/image-prompt` 自动可用。

## 与 harness 的关系

产出的 `IMAGE-PROMPTS.md` 接进 `../../Claude_Roles/` 的 role harness;它是
`roles/production/art-spec.md` 的明确下游(art-spec 出 brief,本 skill 出成品 prompt)。
