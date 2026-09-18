# 漫画转真人·第一步（中文描述 JSON）

一个面向 Codex / ChatGPT 桌面端的个人插件：只读取当前上传的动漫、漫画、插画或虚拟角色参考图，先建立严格的视觉事实层，再输出可交给后续真人化步骤使用的 JSON。

它不生成图片，也不会从角色百科、历史图片、旧提示词或其他对话中补充当前画面。

## 核心逻辑

固定权限链：

```text
当前原图 > 视觉事实 > 真人化解释 > 最终生成
```

后级不得推翻、删除、美化或改写前级已经确认的事实。

视觉证据分为三类：

- `clearly_visible`：当前图片直接可见。
- `high_confidence_inference`：有明确画面依据，但没有完全展示。
- `cannot_confirm`：遮挡、裁切、分辨率不足、歧义或画面未提供。

## 主要能力

- 将画面事实与真人化解释彻底分层。
- 锁定角色身份、脸部关系、Hair Design DNA、身体动作意图、服装、配饰及特殊生物结构。
- 二维发丝高光、赛璐璐阴影和块面明暗只记录为绘画语言，不直接变成染发、硬质假发或真实斑痕。
- 支持高虹膜/高黑眼球比例与强眼球湿润镜面反射作为角色 DNA，同时禁止字面动漫大眼。
- 真人化层使用真实解剖、肌肉动力学、头发物理、分区皮肤质感、结构光影与自然镜头表现。
- 保留旧字段以兼容现有 Step Two 工作流。
- 输出为严格 JSON：键名使用英文，视觉描述和解释使用中文，`negative_prompt` 保持英文。

## 不会做什么

- 不在第一步生成图片。
- 不补全背面、鞋底、被遮挡的耳朵、手或隐藏结构。
- 不自动年轻化、网红脸化、磨皮或按平均真人比例重做五官。
- 不擅自新增、删除或重设计服装、配饰、道具、肢体及非人结构。
- 不把历史参考、角色名或外部知识当成当前图片证据。

## 安装

按照 OpenAI 官方支持的 GitHub 市场方式添加本仓库：

```powershell
codex plugin marketplace add baoshuai741-ai/anime-to-real-step-one-en-json
codex plugin add anime-to-real-step-one-en-json@anime-to-real-step-one
```

然后确认：

```powershell
codex plugin list
```

状态应显示为 `installed, enabled`。安装或更新后请新建一个任务，以确保载入新版本。

## 使用方法

1. 在新任务中上传当前参考图。
2. 明确调用 `$anime-to-real-step-one-en-json`。
3. 多图时明确说明每张图片控制的字段，例如“图 1 控制脸和头发，图 2 只控制服装”。
4. 检查视觉事实、置信度、二维绘画伪影与无法确认项目。
5. 确认后，把当前原图和完整 JSON 一起交给第二步；不要只传 JSON。

示例：

```text
使用 $anime-to-real-step-one-en-json 分析我当前上传的参考图。
先建立视觉事实层并区分明确可见、高可信判断和无法确认，
然后输出严格 JSON。不要生成图片。
```

## 关键字段

| 字段 | 作用 |
|---|---|
| `authority_chain` | 固定原图、事实、解释、生成的权限顺序 |
| `evidence_confidence` | 三档证据置信度 |
| `visual_facts` | 脸、眼睛、头发、身体、服装、生物结构及镜头环境事实 |
| `anime_rendering_artifacts` | 二维高光、阴影块、描边等绘画语言 |
| `uncertain_or_occluded` | 未知、遮挡、裁切、歧义及禁止推断项 |
| `character_dna` | 身份关键特征索引 |
| `immutable_anchor` | 后续步骤不得修改的证据锚点 |
| `realism_translation_rules` | 从已确认设计到真实解剖、材质和光学的转译规则 |
| `high_risk_translation_items` | 最容易被错误正常化或错误物理化的项目 |
| `visual_translation_rules` | 兼容旧工作流的真人化规则字段 |

更完整的中文说明见 [`usage-guide-zh.md`](plugins/anime-to-real-step-one-en-json/skills/anime-to-real-step-one-en-json/references/usage-guide-zh.md)。

## 工作流边界

- Step One：视觉事实提取、角色 DNA 与真人化解释 JSON。
- Step Two：同时使用当前原图和完整 Step One JSON 生成真人图。
- Step Three：如有需要，使用已确认的真人化结果制作角色设定图。

本仓库只包含 Step One。

## 验证状态

- Skill 结构验证：通过。
- 插件结构验证：通过。
- JSON 模板解析：通过。
- 本地源文件与安装缓存 SHA-256：一致。
- 真实图片输出效果：需要使用者在新任务中以当前参考图验证；结构验证不等于最终视觉质量验证。

## 更新

```powershell
codex plugin marketplace upgrade anime-to-real-step-one
codex plugin add anime-to-real-step-one-en-json@anime-to-real-step-one
```

更新后新建任务再测试。

## 说明

本仓库未附带开源许可证。公开可见不自动授予复制、修改或再分发许可；如需开放授权，请由仓库所有者另行选择并添加许可证。

---

## English summary

This Codex plugin analyzes only the reference image(s) supplied in the current message. It first creates a strict visual-evidence layer, separates direct facts from high-confidence inferences and unknown/occluded information, and only then writes realism-translation rules in the same JSON.

It does not generate images. JSON keys are English, visual descriptions are Chinese, and the fixed `negative_prompt` entries remain English.

Install from GitHub:

```powershell
codex plugin marketplace add baoshuai741-ai/anime-to-real-step-one-en-json
codex plugin add anime-to-real-step-one-en-json@anime-to-real-step-one
```

Start a new task after installation or update so Codex loads the current plugin version.
