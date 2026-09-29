# 漫画转真人：第一步 + 第二步 / Anime to Real: Step One + Step Two

本仓库提供两个彼此独立、但必须按顺序交替使用的 Codex 插件：

- `anime-to-real-step-one-en-json`：读取当前原画，先建立视觉事实层，再输出中文描述 JSON。
- `anime-to-real-step-two`：同时读取同一张当前原画和已确认的第一步 JSON，生成写实真人图或最终生成提示词。

两个插件不会自动互相调用。它们保持独立入口，用户需要在每一轮转换中手动按“第一步 → 第二步”交替使用。

## 必须交替使用

```text
新原画或原画发生变化
        ↓
第一步：原画 → 视觉事实 + 角色 DNA + 真人化解释 JSON
        ↓ 用户检查并确认 JSON
第二步：同一张原画 + 完整已确认 JSON → 写实真人图
        ↓
下一张新原画或设计发生变化时，重新回到第一步
```

不能跳过第一步直接执行第二步，也不能让第二步读取旧对话中的图片或 JSON。每次参考图、角色设计、多图职责或关键约束发生变化，都应重新执行第一步，再执行第二步。

固定权限链：

```text
当前原图 > 视觉事实 > 真人化解释 > 最终生成
```

后级不得推翻、删除、美化或改写前级已经确认的事实。固定目标 `9 head body proportion` 是用户授权的身体比例例外，但源图可见比例仍需由第一步单独记录。

## 第一步插件：视觉事实与 JSON

第一步把证据分为：

- `clearly_visible`：当前图片直接可见。
- `high_confidence_inference`：有明确画面依据，但没有完全展示。
- `cannot_confirm`：遮挡、裁切、分辨率不足、歧义或画面未提供。

主要职责：

- 分离画面事实、二维绘画语言与真人化解释。
- 锁定角色身份、脸部关系、Hair Design DNA、身体动作意图、服装、配饰及特殊生物结构。
- 先建立可信真人颅面、软组织和眼眶基础，再映射完整 Face DNA，避免用通用美人比例覆盖角色身份。
- 分别记录皮肤区域差异与头发设计意图，禁止用统一毛孔、全脸锐化或逐条复制二维发束制造“真实感”。
- 把二维发丝高光、赛璐璐阴影和块面明暗记录为绘画语言，而非染发、硬质假发、皮肤痕迹或真实接缝。
- 保留高虹膜/高黑眼球比例、强眼球湿润镜面反射等有证据支持的角色 DNA，同时禁止字面动漫大眼。
- 只返回严格 JSON，不生成图片。

## 第二步插件：JSON 与原画生成真人图

第二步必须同时收到：

1. 当前消息中的原画；
2. 与该原画对应、已经检查确认的完整第一步 JSON；
3. 明确的生成请求，或只返回最终生成提示词的请求。

主要职责：

- 只把二维表现转换成真实三维解剖、真人皮肤、真实头发、可穿着材料、物理光影和真实相机成像。
- 按固定优先级锁定角色 DNA、人物身份、构图姿态、真实人体、真实头发、服装材质、物理光线、相机成像和 Anti-AI 抑制。
- 保留表情、视线、动作意图、姿势方向和轮廓；二维几何无法由真人完成时只作最小必要修正。
- 保留 Hair Design DNA，并以发根、密度、质量、重力、惯性及碰撞解释头发，不把标志性发型普通化。
- 抑制塑料皮肤、网红脸、CG 光泽、廉价 Cosplay、过度锐化、错误手脚、漂浮材质、不可能阴影、文字和水印。
- 生成后必须逐项验收；通用 AI 美人脸、同质光滑皮肤、设计漂移或未知补全均判定为失败，不能因成功出图就宣称通过。

如果当前消息缺少原画或 JSON，第二步应要求重新提供，不能从历史对话补取。

## 安装两个插件

按照 OpenAI 官方支持的 GitHub 市场方式添加本仓库：

```powershell
codex plugin marketplace add baoshuai741-ai/anime-to-real-step-one-en-json
codex plugin add anime-to-real-step-one-en-json@anime-to-real-step-one
codex plugin add anime-to-real-step-two@anime-to-real-step-one
```

然后确认：

```powershell
codex plugin list
```

两个插件均应显示为 `installed, enabled`。安装或更新后请新建任务，以确保载入新版本。

## 每轮操作方法

### A. 执行第一步

上传当前原画，然后明确调用：

```text
使用 $anime-to-real-step-one-en-json 分析我当前上传的参考图。
先建立视觉事实层并区分明确可见、高可信判断和无法确认，
然后输出严格 JSON。不要生成图片。
```

多图时必须明确每张图控制什么，例如“图 1 控制脸和头发，图 2 只控制服装”。检查并确认输出 JSON 后再进入第二步。

### B. 执行第二步

在当前请求中重新附上同一张原画和完整已确认 JSON，然后明确调用：

```text
使用 $anime-to-real-step-two，依据当前附上的原画和已确认的第一步 JSON，
生成写实真人图。不得读取旧图或旧 JSON，不得改变视觉事实和角色 DNA。
```

如果只需要提示词，应明确写“只返回最终生成提示词，不生成图片”。

### C. 下一轮重新交替

更换原画、改变角色设计、重新分配多图职责或修改关键约束时，不要沿用旧 JSON。重新执行第一步，确认后再执行第二步。

## 完整说明

- [第一步中文使用说明](plugins/anime-to-real-step-one-en-json/skills/anime-to-real-step-one-en-json/references/usage-guide-zh.md)
- [第二步中文使用说明](plugins/anime-to-real-step-two/USAGE.zh-CN.md)
- [真实参考图只读回归测试](REGRESSION_TEST.zh-CN.md)

## 验证状态

- 两个 Skill 的结构验证：通过。
- 两个插件的结构验证：通过。
- 第一步模板 JSON 解析、14 个顶层键、固定英文负面词和重复键检查：通过。
- 第一、第二步个人安装版本此前均验证为 `installed, enabled`。
- 2026-09-29 已使用一张新的本地参考图执行真实图片回归；测试图片和产物未提交到仓库。
- Step Two 已依据同一张原图和已确认 JSON 成功出图；构图、姿势、发型轮廓、主要服装结构、配饰和未知边界通过。
- 面部仍有通用 AI 美人脸倾向，皮肤仍过度平滑且区域差异不足，因此本轮真实图片回归总体为 **FAIL**，不能视为视觉质量验证通过。
- 本次规则已增加生成后验收和最小局部修正要求；更新后的版本仍需安装后用另一张新参考图复测。结构和安装验证不代表最终视觉质量验证。

## 更新

```powershell
codex plugin marketplace upgrade anime-to-real-step-one
codex plugin add anime-to-real-step-one-en-json@anime-to-real-step-one
codex plugin add anime-to-real-step-two@anime-to-real-step-one
```

## 许可说明

本仓库未附带开源许可证。公开可见不自动授予复制、修改或再分发许可；如需开放授权，请由仓库所有者另行选择并添加许可证。

---

## English summary

This repository contains two separate Codex plugins that must be used in an alternating Step One → Step Two cycle for every source image.

- Step One reads only the current artwork, creates a strict visual-evidence layer, and outputs an approved JSON contract.
- Step Two requires both the same current artwork and that complete approved JSON, then generates the live-action result or returns the final generation prompt.

When the source image or design changes, return to Step One. Never reuse historical images or JSON as a substitute for current inputs.

Real-image regression was executed on 2026-09-29. The pipeline produced an image from the same source and approved JSON, but the result failed final acceptance because the face still drifted toward a generic AI beauty template and the skin remained too uniformly smooth. Successful generation is not recorded as a visual-quality pass.

Install both plugins:

```powershell
codex plugin marketplace add baoshuai741-ai/anime-to-real-step-one-en-json
codex plugin add anime-to-real-step-one-en-json@anime-to-real-step-one
codex plugin add anime-to-real-step-two@anime-to-real-step-one
```
