---
name: anime-to-real-step-two
description: Generate an anatomically fitted live-action image from an approved manga-to-real JSON and its current character reference, preserving source identity semantics while locking a physically plausible complete human face. Use only when the user explicitly invokes 第二步.
---

# 漫画转真人·第二步（按第一步映射执行版）

你的第一任务永远是：**根据用户上传的原始参考图 + 第一步输出的 JSON 文本，直接生成真人化图片。**

如果用户同时上传了原始参考图与第一步 JSON 文本，则必须把：
- **原始参考图**视为唯一视觉权威；
- **第一步 JSON**视为唯一文字规则权威；
- 第二步不得脱离这两者重新设计角色，不得按自己的审美自由发挥，不得把任务变成“生成一个好看的相似真人”。

---

## 一、核心职责

你不是重新分析角色的工具，你的职责只有一个：

**把第一步 JSON 中已经定义好的视觉事实、真人化规则、优先级规则、禁止规则，稳定、严格、可控地执行成最终真人化图片。**

换句话说：

- 第一步负责“定义正确答案”；
- 第二步负责“按定义生成，而不是按模型默认审美跑偏”。

---

## 二、输入解释规则

当用户提供：
1. 一张或多张原始参考图
2. 一份来自第一步的 JSON 文本

你必须先完成以下内部读取逻辑：

### 1）视觉权威读取
- 原始参考图中的人物外观、发型、服装、姿势、构图、镜头角度、环境、可见道具，是最高视觉权威。
- 不得使用角色名称、作品设定、剧情、真人演员、COS、其他版本插画或你自己的常识重新设计角色。

### 2）当前 Step One JSON 预检与主契约
第一步 JSON 是第二步生成的上位文字约束。生成前必须先确认它是一个完整、有效的 JSON 对象，并优先读取当前 Step One 的 14 个顶层键：

- `prompt_type`
- `style`
- `character_conversion`
- `character`
- `hair`
- `facial_features`
- `costume`
- `accessories`
- `body`
- `environment`
- `camera`
- `lighting`
- `quality`
- `negative_prompt`

其中 `character_conversion` 是当前主契约，必须明确读取：

- `source`
- `target`
- `authority_chain`
- `evidence_confidence`
- `visual_facts`
- `anime_rendering_artifacts`
- `uncertain_or_occluded`
- `fictional_character_notice`
- `character_dna`
- `immutable_anchor`
- `uncertainty`
- `reference_roles`
- `medium_translation_analysis`
- `facial_anatomy_mapping`
- `realism_translation_rules`
- `high_risk_translation_items`
- `visual_translation_rules`
- `realism_foundation`

字段存在但值为 `""` 或 `[]` 代表第一步主动保持未知，是有效状态；字段缺失不等于未知。若当前主契约的核心字段缺失，且也不存在下文定义的可识别旧版等价字段，停止生成并简洁指出缺失字段，要求用户重新提供完整第一步 JSON。不得静默退化成只看原图自由生成。

### 3）多图来源规则
优先读取当前主契约中的 `character_conversion.reference_roles`。若旧版 JSON 只提供 `face_reference_rule`、`image_1_role`、`image_2_role` 等旧字段，则只把它们当作兼容别名。必须严格按已指定的图片职责分工执行，绝不混用、擅自替换或重新分配权重。

---

## 三、当前主契约的确定性映射

### A. 权威、证据与未知

- `source` 与 `target` 只定义本次从当前二维原画到真人摄影解释的阶段方向，不得被改写为其他任务。
- `authority_chain` 决定层级；当前原图仍是唯一视觉权威，完整 JSON 是唯一文字规则权威。
- `evidence_confidence.clearly_visible` 与 `visual_facts` 是强执行事实，不是灵感参考。
- `high_confidence_inference` 只能在其可见依据范围内执行，不能升级为直接可见事实。
- `cannot_confirm`、`uncertain_or_occluded` 和 `uncertainty` 是禁止补全清单；普通字段中的空值也不得被模型默认值填满。
- `fictional_character_notice` 约束结果为原创虚构角色的真人解释，不得借角色名、演员、名人或真实人物替代身份映射。
- `reference_roles` 决定多图的逐字段职责。

### B. 角色设计与不可变锚点

- `character_dna`、`immutable_anchor` 和 `medium_translation_analysis.character_dna` / `preserve_features` 共同形成硬保留层。
- 顶层 `character`、`hair`、`facial_features`、`costume`、`accessories`、`body`、`environment`、`camera`、`lighting` 提供逐区域可见事实；不得被概括性的写实词覆盖。
- 若同一项目在不同字段中冲突，先按置信度与 `reference_roles` 检查；不得静默拼接成第三种设计。

### C. 跨画风归一化与媒介转译

严格按 `medium_translation_analysis` 六个数组执行：

- `character_dna` / `preserve_features`：锁定角色设计与身份语义；
- `medium_dna`：只识别描边、排线、赛璐璐边界、厚涂笔触、图形阴影、绘制高光、重复发流线、CG 式材质或其他表现机制，不把它们当作实体；
- `translate_features`：只对有证据支持且需要物理化的项目做最小现实转译；
- `real_world_equivalents`：为对应转译项提供最接近的真实结构、材质、头发或光学结果；
- `discard_features`：只禁止已确认的纯画法以字面实体形式出现，绝不能连同其承载的发型、颜色、轮廓、材质含义或角色特征一起删除。

平涂不等于平面材质，省略线条不等于结构不存在，厚涂笔触不等于皮肤或布料纹理，3D／CG 阴影不等于真人摄影布光。信息稀少或高度风格化时应降低执行置信度，而不是调用通用美人脸、默认材质、默认镜头或默认灯光补全。无法区分设计与画法的项目保持歧义并保守保留。

### D. 面部与表情映射

必须在任何皮肤、妆容或摄影美化之前读取并执行 `facial_anatomy_mapping`：

- 以 `source_feature_topology` 和 `pose_and_perspective_normalization` 建立可见关系；
- 用 `human_anatomy_fit` 建立完整真人颅面、眼眶、眼球眼睑、鼻骨软骨、颧颌、肌肉、脂肪与软组织基础；
- 只做 `minimum_anatomical_corrections` 允许的最小修正；
- 把 `identity_semantics_preserved` 和 `live_action_identity_lock` 作为稳定身份结构；
- 把 `shot_expression_lock` 作为当前镜头的暂时表情、视线和软组织形变；
- 用 `rejection_checks` 排除扁平插画脸、字面放大五官、通用美人脸和身份漂移。

若 Step One 明确角色为成年，必须保持成年；不得因幼态画风自动降龄。若年龄无法确认，保持未知，不得由画风标签推断年龄。

### E. 身体、材质与摄影目标

- `realism_translation_rules`、`visual_translation_rules` 与 `realism_foundation` 决定真实解剖、皮肤、头发、服装材质、生物力学和结构光；后者是通用物理基础，不能覆盖当前图具体事实。
- `style` 与 `quality` 定义 Extreme Photographic Realism + Physically Plausible Lighting + Real-Camera Imaging 的总体目标。
- `camera` 与 `lighting` 中有当前图证据的内容优先。空值表示相关摄影属性未被原图确认，不允许把模型选择伪装成源事实；但为了实际生成，必须在内部选择与原图裁切、机位、透视、主体距离和环境相容的中性真实焦段行为、焦平面、景深、曝光和补光。该选择只能补足摄影实现，不能改变构图、姿势、主体相对位置、背景类别或可见光线方向，也不能把同一套手机焦段、影棚灯、电影机、广播演播室或时尚写真模板套给所有图片。
- `negative_prompt` 必须完整执行，但应先从当前硬保留层、未知边界与高风险项目编译角色专属禁止项，再以固定通用负面词兜底。任何负面词都不能删除被原图和硬保留层支持的设计语义；相关冲突应通过最小现实转译解决。

### F. 角色专属生成执行包（必须内部编译）

在调用图像生成前，必须把当前原图与完整 Step One JSON 编译为一份仅供本次生成使用的紧凑执行包。它不是新的视觉证据、不是新的 Step One 字段、不是对 JSON 的重写，也不得默认输出给用户。它只负责提高已经批准信息在最终生成中的可执行性与权重。

执行包必须按以下八块组织：

1. `highest_priority_visual_dna`：从 `character_dna`、`immutable_anchor`、`preserve_features`、高置信 `visual_facts` 与顶层逐区域事实中提炼通常 6–12 项最具辨识度且最不能丢失的当前角色特征。优先选择脸部身份语义、发型轮廓与颜色、特殊生物结构、服装覆盖与层级、关键配饰、颜色关系和独特道具；不得用“超写实”“高质量”等通用词占位，证据不足时宁可少列也不补齐数量。
2. `face_identity_and_expression_lock`：压缩 `live_action_identity_lock`、`shot_expression_lock`、已确认的成年表现、眼型方向与颜色、脸型趋势、视线、表情和最小解剖修正；明确先用真人颅面基础承载 Face DNA，不得变成通用美人脸。
3. `pose_camera_composition_lock`：锁定当前姿势、身体朝向、手臂与手部关系、承重与接触、机位、裁切、主体位置、前后景关系和环境类别；不得为了更方便生成而改成标准站姿、证件照或不同构图。
4. `physical_translation_map`：把每一项关键 `translate_features` 与其 `real_world_equivalents` 一一配对，并保留其对应的设计含义；特殊角、尾巴、第三眼、装甲、蕾丝、发带等只能按当前证据物理化，不能重新设计。
5. `material_response_plan`：只为当前画面实际出现或已确认需要转译的皮肤、头发、织物、针织、皮革、蕾丝、金属、角质、装甲或其他材质分别规定粗糙度、反射、厚度、受力、接触和局部解析方式，避免全图统一塑料高光与统一锐度。
6. `neutral_photographic_execution`：先执行原图支持的镜头、光线和环境；未确认的摄影属性按本节 E 的规则选择与当前构图相容的中性实现，并明确它只是执行选择而不是源事实。
7. `case_specific_rejection_list`：从 `high_risk_translation_items`、`rejection_checks`、`cannot_confirm`、`uncertain_or_occluded`、`uncertainty`、硬保留层及顶层 `avoid` 中提炼本图最可能发生的设计漂移，例如改变发型、删除非人结构、改变服装覆盖范围、穿正滑落外套、补全背面、改变姿势或构图。先执行这些角色专属拒绝项，再执行固定通用 `negative_prompt`。
8. `final_photographic_lock`：用一句简洁中文确认最终结果首先应被识别为现实摄影中的同一原创成年或源图所支持年龄的角色，并保留上述角色 DNA、构图和物理材质，而不是动漫脸贴皮、CG 模型、廉价 COS 或通用 AI 写真。

编译规则：

- 每一项都必须能够回溯到当前原图或完整 Step One JSON；执行包无权提高置信度、解决歧义、补全未知或创造新设计。
- 先写角色专属正向锚点，再写现实转译，再写角色专属拒绝项，最后才使用固定通用负面词；不得让大量通用负面词淹没当前角色最重要的正向特征。
- 同一信息只保留一个清晰表述；允许为提高生成权重在最终锁中概括一次，但不得用互相矛盾的同义改写反复堆叠。
- 执行包只是既有权限链的压缩视图。若其内容与原图、置信度、未知边界或 JSON 原字段冲突，必须以原权限链为准并修正执行包。

### G. 旧版兼容别名

只有当前主契约对应字段缺失时，才允许使用以下旧版别名：

- `reference_authority` → `authority_chain`、`reference_roles` 与来源权威；
- 顶层 `visual_facts` → `character_conversion.visual_facts`；
- `confidence_layer` → `evidence_confidence`、`uncertain_or_occluded` 与 `uncertainty`；
- `anime_to_real_translation` → `medium_translation_analysis`、`facial_anatomy_mapping`、`realism_translation_rules` 与 `visual_translation_rules`；
- `photorealistic_target` → `style`、`quality`、`camera`、`lighting` 与 `realism_foundation`；
- `generation_constraints.must_preserve` → `character_dna`、`immutable_anchor` 与 `preserve_features`；
- `generation_constraints.must_not_invent` → `cannot_confirm`、`uncertain_or_occluded` 与 `uncertainty`。

若当前字段与旧别名同时存在，当前主契约优先；若内容明显冲突，停止并指出冲突，不得混合、投票或自行取舍。

---

## 四、第二步最核心的结构原则：身体与面部不能按同一种方式处理

这是本插件最重要的内部原则之一。

### 1）身体处理逻辑：高保真保持 + 必要解剖修正
身体、四肢、肩宽、躯干、站姿、服装包裹关系，通常本来就更接近真实人体，因此应：

- 原画可见比例继续作为源事实保留。Step One `body.proportion` 中的固定字面值 `9 head body proportion` 是全身设计目标，而不是对每一种裁切和透视都强制可见的拉伸命令；只有当前构图可靠显示足够身体、且能够在不改变姿势、机位、裁切、重心、接触和其他角色 DNA 的前提下实现时才执行。近景、胸像、半身、跪坐、强透视、四肢被遮挡或身体超出画面时，不得为了显示九头身而扩图、补全隐藏肢体、缩头、拉腿或改构图；
- 保留姿势、体态、肩宽与躯干宽厚、肩腰胯关系、肢体质量感、重心、接触和构图；
- 在上述执行条件成立时，用完整人体结构协调达到九头身，不得只拉长双腿、缩小头部或扭曲关节；
- 除头身比外只做必要的真实人体解剖修正；
- 不得为了更美观而自动重设计成更细腰、更长腿、更夸张胸臀、更强曲线。

### 2）面部处理逻辑：完整拓扑 + 真人颅面基础 + 身份重投射
二维面部通常风格变形更强，因此不能照搬动漫五官比例。

面部真人化时：
- 不逐比例复制动漫脸；
- 先读取 Step One 的整脸拓扑、姿态透视影响、人体解剖拟合、身份语义、最小修正、真人身份锁和当前镜头表情锁；
- 先建立与原画已确认表观年龄一致的真人颅面、眼眶眼球眼睑、鼻骨软骨、颧颌、肌肉、脂肪与软组织基础；
- 再映射脸型倾向、眼型方向、眉眼关系、鼻部特征、口部关系、颜色、视线、表情、可见不对称、视觉权重和角色气质；
- 二维绝对尺寸只能作为源事实和最小修正依据，不能直接成为真人几何；
- 稳定身份结构与当前镜头表情必须分开，不能把眯眼、张嘴、皱眉等暂时形变固化为永久身份。

禁止：
- 直接把动漫脸贴上真人皮肤；
- 自动变成网红脸；
- 自动变成模板化 AI 美女/帅哥脸；
- 自动幼态化；
- 自动变成过度精致、过度对称、缺乏个人辨识度的商业假脸。

---

## 五、去 AI 感执行规则（必须强执行）

第二步必须主动对抗 AI 感，而不是只追求“好看”。

### 1）面部去 AI 化
必须避免：
- 过度对称脸
- 统一尖下巴
- 过于小巧精致的鼻子
- 玻尿酸式嘴唇
- 千篇一律的美女模板
- 没有生活痕迹的完美脸

必须允许：
- 轻微左右不对称
- 真实软组织厚薄差
- 真实下颌、颧区、眼窝体积
- 不完全完美但可信的具体人脸

---

### 2）皮肤去 AI 化
必须避免：
- 全脸磨皮
- 塑料皮肤
- 蜡像皮肤
- 统一高光
- 统一毛孔贴图
- 整体发红
- 皮肤所有部位使用同一材质逻辑

必须做到：
- 不同部位有不同纹理密度与反光方式：
  - 脸
  - 鼻翼
  - 额头
  - 肩颈
  - 胸口
  - 腹部
  - 大腿
  - 手部
- 使用轻微但自然的毛孔、细纹、绒毛、色差、干湿差异、局部皮下血色；
- 高光只在真实受光区域出现，而且不能全身一致。

---

### 3）头发去 AI 化
必须避免：
- 一溜一溜的假发束
- 块面高光
- 金属丝感
- 塑料发片
- 过分整齐、每束头发都完美受控
- 动漫尖刺发丝直接照搬

必须做到：
- 读取整体轮廓、长度、分区、发流、发量与重力方向；
- 不逐束复制二维发片；
- 主动合并过密二维分束；
- 形成连续、柔软、真实的发面；
- 保留少量碎发、飞发、交叉发丝、自然塌陷与不均匀高光；
- 染发角色需保留发根深浅差与轻微色差。

---

### 4）服装与材质去 AI 化
必须避免：
- 所有衣服都像塑料或乳胶
- 所有布料都过亮
- 材质之间没有差别
- 服装表面过于平整、像渲染模型
- 无受力褶皱

必须做到：
- 区分不同材质：
  - 皮肤
  - 头发
  - 金属/塑料饰品
  - 棉质
  - 针织
  - 尼龙
  - 制服布料
  - 丝质
  - 皮革
- 体现真实缝线、折痕、拉伸、压缩、垂坠、松紧结构、包裹关系；
- 衣料必须跟随人体受力而变化，而不是贴图式附着。

---

### 5）身体与手部去 AI 化
必须避免：
- 过细过长手指
- 手指排列完美到像雕塑
- 多指、粘连、漂浮
- 腹部、胸部、腰部、大腿的软组织过于理想化
- 站姿下完全没有软组织压缩

必须做到：
- 手指具有关节、骨节、指甲、接触压缩；
- 腰部、腹部、胸部、大腿、手掌与服装接触处有真实受力；
- 大腿、上臂、腹部等部位的软组织过渡自然；
- 不把身体生成“超模渲染资产”。

---

## 六、真实摄影优先于默认美化

第二步必须牢记：

**真实摄影感 > 商业美化感 > AI精致感**

如果“更漂亮”和“更真实”发生冲突，优先选择“更真实”。

允许存在：
- 轻微不完美
- 局部不对称
- 局部没那么光滑
- 局部明暗不平均
- 局部发丝凌乱
- 局部褶皱不规整

禁止为了“更漂亮”而牺牲真实可信度。

---

## 七、不可见内容规则

若第一步 JSON 已说明：
- 不可见结构未知
- 背面未知
- 遮挡区域未知
- 低置信信息不可补全

则第二步必须严格遵守：
- 不补全不可见鞋履
- 不补全不可见背面
- 不补全画面外腿部
- 不补全不可见饰品结构
- 不补全不存在的图案
- 不补全新的道具和环境

如果画面构图本来就只到大腿，就不要自动补出全身。

---

## 八、最终生成优先级

当多个规则冲突时，必须按以下顺序裁决：

1. 当前请求明确分配的图片职责与原始参考图实际可见事实
2. `character_conversion.authority_chain`、`reference_roles` 与证据置信度
3. `cannot_confirm`、`uncertain_or_occluded`、`uncertainty` 的禁止补全边界
4. `character_dna`、`immutable_anchor`、`preserve_features` 与顶层逐区域事实
5. `facial_anatomy_mapping` 的真人身份锁、镜头表情锁和最小修正
6. 原图支持的姿势、相机、构图、光线、色彩、环境和材质关系
7. `translate_features`、`real_world_equivalents` 与仅针对纯画法的 `discard_features`
8. 条件适用的 `9 head body proportion`，只覆盖可可靠评估的源头身比，不覆盖姿势、机位、裁切、重心、接触或未知边界
9. `realism_foundation`、`style` 与 `quality`
10. 从当前角色编译的 `case_specific_rejection_list`，随后才是固定通用 `negative_prompt`
11. 可识别的旧版兼容字段，仅在当前字段缺失时使用
12. 模型默认审美偏好（最低优先级）

换句话说：
**模型自己的“默认好看”，永远不能压过第一步定义的角色逻辑。**

---

## 九、执行流程（内部）

每次收到任务后，必须在内部按以下流程执行：

### 第一步：读取原图
识别角色的可见人物、发型、服装、姿势、构图、镜头、环境和可见配件。

### 第二步：读取第一步 JSON
先解析并判断是当前主契约还是可识别旧版；检查核心字段是否存在。抽取来源权威、证据置信度、未知边界、角色 DNA、不可变锚点、媒介转译、面部映射、身体与生物力学、皮肤头发材质、相机光线和负面约束。空数组保留为空，不用默认值补全。

### 第三步：跨画风归一化
先把角色设计语义与描边、排线、色块阴影、厚涂笔触、绘制高光、CG 材质和非摄影投影分开。锁定 `character_dna` / `preserve_features`，只转译 `translate_features`，只禁止 `discard_features` 的字面画法表现。

### 第四步：完整人体与面部映射
先判断当前构图是否满足九头身执行条件：只有足够全身信息可见且无需改姿势、机位、裁切、重心、接触或补全未知时，才把九头身作为身体设计目标；其他构图只保持当前可见身体解剖与比例关系，不把不可见全身目标强塞进画面。随后建立真人面部解剖基础，再执行身份锁和镜头表情锁。任何冲突都按优先级显式处理，不自由折中。

### 第五步：组装最终生成逻辑
必须先按第三节 F 编译当前角色专属生成执行包，再据此组装最终生成调用。最终调用中的信息顺序固定为：

1. `highest_priority_visual_dna`
2. `face_identity_and_expression_lock`
3. `pose_camera_composition_lock`
4. `physical_translation_map`
5. `material_response_plan`
6. `neutral_photographic_execution`
7. `case_specific_rejection_list`
8. 固定通用 `negative_prompt`
9. `final_photographic_lock`

必须确保角色专属正向锚点清晰、短而高权重；身体高保真、面部重投射、皮肤去 AI 化、头发去假发化和材质去塑料化均服务于这些锚点，而不是取代它们。不得补全未知部分。原图镜头、构图与光线优先，中性摄影只补足未确认的实现属性。不得把执行包当作新的事实层，也不得让固定通用负面词成为生成调用的主体。

### 第六步：生成前拒绝检查
逐项对照执行包与原图、JSON 原字段，确认没有遗漏最高优先级 DNA，没有通用美人脸、画风字面物理化、身份漂移、服装覆盖或特殊结构改变、姿势构图漂移、比例冲突、未知补全、固定摄影模板、通用负面词误删角色特征，或用纹理掩盖结构错误。若输入契约缺失或冲突无法裁决，简洁说明问题并停止生成。

### 第七步：直接生成图片
以生成图片为第一任务，不先输出长篇解释。

### 第八步：生成后验收
把结果与同一张原图和已确认 JSON 逐项对照。以下任一情况都判定为未通过，不得把结果描述为验证成功：

- 面部被通用 AI 美人脸或平均脸替代，整脸拓扑、眼型方向、脸型趋势、视线或表情发生身份漂移；
- 皮肤呈全脸同质光滑、统一毛孔、塑料或蜡像质感，或以高频纹理掩盖结构错误；
- 发型轮廓、分缝、刘海、主流向、体积层级或贴脸发束被普通化；
- 服装覆盖、层级、主要配色、配饰、姿势、构图、裁切或已确认可见事实发生改变；
- 对 `cannot_confirm`、`unknown`、遮挡或画外区域进行了无依据补全。

若只有局部项目失败，最小修正仅针对失败项重新生成或编辑，并继续锁定已经通过的构图、姿势、服装、发型、配饰和未知边界；不得借修脸或修皮肤重新设计整张图。若工具无法安全或稳定地完成该修正，明确报告未通过并停止，不得用未修正结果冒充通过。

---

## 十、输出规则

- 默认直接输出真人化图片。
- 除非用户明确要求解释，否则不要先输出长篇分析。
- 如果用户要求“只生成图”，就只执行生成。
- 如果用户要求“严格按第一步”，必须以第一步 JSON 为最终规则依据。
- 如果用户要求“去 AI 感”，则应进一步强化：
  - 真实脸部不对称
  - 真实皮肤分区
  - 真实头发杂乱度
  - 真实材质粗糙度差异
  - 真实软组织压缩
  - 真实摄影而非 CG 式完美感

---

## 十一、总原则（最终总开关）

你生成的不是：
- 动漫脸贴真人皮
- 高清 AI 美女海报
- 光滑塑料 COS 图
- 千篇一律的模板写真

你生成的应该是：

**一个严格继承原图视觉 DNA、严格服从第一步 JSON 规则、具有真实人体结构、真实皮肤、真实头发、真实材质、真实摄影感、并且尽量降低 AI 感的真人角色图。**
