---
name: anime-to-real-step-one-en-json
description: "Analyze the currently uploaded anime, manga, illustration, or virtual-character reference through a strict visual-evidence layer, then return one isolated live-action conversion JSON object with English keys, Chinese descriptions, and fixed English negative prompts. Use only when the user explicitly invokes Step One."
---

# Step One | Convert Manga Reference Image to JSON

WORKFLOW POSITION: STEP 1 / 3 — first extract Visual Evidence, then derive Character DNA and realism translation rules inside the same JSON. Pass the complete approved JSON and current source image to Step Two; do not generate an image here.

Use only the manga, anime, illustration, or fictional-character reference image(s) uploaded in the current message. Do not read, reuse, infer, or combine any historical messages, historical images, previous JSON, character settings, IP information, previous prompts, or content from other stages.

If the user explicitly supplies multiple current images, first record each image's assigned authority and fields. The character image controls identity, face, hair and body; another image controls clothing or an asset only when the user assigns that role. Do not merge unassigned or conflicting features. For each field, follow the current explicit role assignment; otherwise use the primary character image. This does not permit historical references.

Return only one valid JSON object. Do not output Markdown, headings, explanations, or any additional text.

All JSON keys must use English. All source observations, judgments, explanations and translation descriptions must use Chinese. Only the fixed `negative_prompt` entries remain English.

Prohibited:

- Completing information that is not visible, is occluded, or cannot be determined from the original image;
- Generating an image or proceeding to subsequent steps;
- Using prohibited prompts.

## Mandatory authority chain

The permission order is immutable:

`current source image > visual facts > realism interpretation > final generation`

- A later layer must not overwrite, beautify, normalize, delete or contradict an earlier supported fact.
- The visual-evidence layer has no authority to redesign materials, correct proportions, beautify the face, invent anatomy, complete occlusion, or decide how the final live-action result should look.
- The realism-translation layer may explain how supported 2D design facts can become physically plausible, but must preserve the recorded visual facts and clearly expose every necessary exception or minimum correction.
- Step Two must receive both the current source image and the complete Step One JSON. JSON never replaces the source image as visual authority.

## Visual Evidence Extraction | must run before interpretation

Before writing any live-action rule, create an evidence ledger using exactly three confidence classes:

- `clearly_visible`: directly observable in the current image. Record only what is visible.
- `high_confidence_inference`: not fully shown but strongly supported by visible geometry, continuity or context. State the evidence and keep it separate from direct facts.
- `cannot_confirm`: occluded, cropped, too small, ambiguous, absent, or unsupported. Use `""` / `[]` in downstream descriptive fields and never complete it.

For each important observation, record its subject/region, Chinese description, confidence class and visible basis. Where useful, also record whether it is identity-critical. Do not use character names, IP knowledge, web references, cosplay, actors, other frames or prior messages to raise confidence.

The evidence pass must cover, when visible: subject count; apparent age and presentation; face geometry and feature relationships; eye aperture, iris-to-visible-eye ratio, dark-iris/dark-eye proportion and optical highlights; nose and mouth; skin color as depicted; ears and non-human structures; hair base color, silhouette, volume, parting, locks and accessories; body orientation, source-visible proportion, pose, balance and contact; clothing layers, colors and visible construction; accessories and biological structures; composition, camera, environment and lighting.

During this pass, never:

- beautify, rejuvenate, feminize/masculinize, normalize or create an AI beauty/influencer face;
- shrink or enlarge eyes, nose, mouth, ears, jaw, head or body to fit realistic averages;
- turn painted highlight bands, cel-shading, graphic shadow blocks or outline edges into dye, physical streaks, rigid hair slabs, skin marks or material seams;
- replace illustrated skin, hair or fabric with newly designed realistic materials;
- add/delete clothing parts, decorations, props, accessories, limbs, ears, tails, horns or other biological structures;
- infer a hidden back, sole, inner layer, occluded hand, body surface or attachment mechanism.

Only after the evidence ledger is complete may the same JSON add `character_dna`, `realism_translation_rules` and `high_risk_translation_items`.

For detailed invocation, field meanings, stage boundaries, common errors, backup and recovery, read [references/usage-guide-zh.md](references/usage-guide-zh.md).

## Source-image lock and layer separation

Separate source-visible design from illustration-only rendering and from realism decisions. `visual_facts` and the detailed top-level `character`, `hair`, `facial_features`, `costume`, `accessories`, `body`, `environment`, `camera` and `lighting` fields hold source evidence. `anime_rendering_artifacts` records 2D depiction mechanisms without treating them as physical design. `character_conversion.character_dna` is a brief identity-critical index, not a copy of all descriptions. `immutable_anchor` locks supported identity-defining facts, not unknowns. `uncertain_or_occluded` and legacy `uncertainty` record unknown, occluded and do-not-infer fields; use `""` or `[]` in ordinary fields where the source is indeterminate. `realism_translation_rules` contains only later-stage translation decisions. Never convert an illustration shading artifact into a physical feature.

Hair Design DNA includes visible base color, length, parting, bangs, silhouette, volume, curl/straight tendency, major flow, tied structures, distinctive face-framing locks and hair accessories. Preserve identity-defining shapes while noting which geometry requires physically plausible real-hair translation. Painted highlight bands, cel-shading boundaries, graphic light/dark blocks, stylized strand shadows and hard illustrated edges are 2D rendering artifacts: do not treat them as dye, separate real strands or hairstyle structure. Reference fidelity applies to hair design, not illustrated shading. Real-world translation uses plausible growth, gravity, grouping, density, curl and restrained natural specular response; avoid a Cosplay wig appearance.

Large irises, a high dark-eye/iris proportion, strong wet mirror reflection, unusual eye spacing or eye angle may be identity-critical Character DNA when visibly supported. Record the source fact first. The realism layer must reconstruct a physically credible eye without automatically reducing it to a generic adult average, while still prohibiting literal anime eyes or impossible anatomy.

Hair dynamics must be derived from a real physical cause chain rather than copied as frozen anime geometry: scalp attachment and growth direction -> layered locks and strand groups -> density, length, mass, flexibility and styling support -> gravity, head/body motion, inertia, air movement and moisture -> collision and friction with the face, shoulders, body, clothing, accessories and other hair -> the final visible shape. Record the source hairstyle silhouette, directional intent and identity-defining locks separately from the physical translation. If a source shape is feasible through real cutting, braiding, tying, restrained styling product or a visible accessory, retain it. If it cannot physically persist, prescribe only the smallest feasible adjustment that preserves its recognizable direction, volume hierarchy and silhouette intent; never normalize it into an ordinary hairstyle. Do not invent wind, wetness, stiffness, hidden supports or motion that the current image does not support.

`natural_optical_rendering` must specify high resolution through real structural information and lens behavior. High resolution != oversharpening. Avoid excessive clarity, micro-contrast, crispy edges, sharpening halos, exaggerated pores, hyper-defined hair strands, uniform sharpness and artificial fabric-fiber emphasis. Skin, hair and materials must remain naturally resolved, with focal-plane detail and gentle optical roll-off.

The currently uploaded original artwork is the sole visual authority for this analysis. Extract only the pose, orientation, camera angle, composition, background, environment, lighting direction, color temperature, shadows, highlights, atmosphere, and character DNA actually visible in the original artwork. Live-action conversion may only translate the two-dimensional representation into believable real human anatomy, skin, hair, clothing materials, props, environment, photography, and lighting, and must not alter the design identity already established by the original artwork.

The character's gender, apparent age, race/species, skin color, facial-feature colors, and special physiological structures must be dynamically extracted from the currently uploaded original artwork. If they are not visible or cannot be determined, use an empty string or empty array. Do not preset the character as female, adult, having a specific skin color, specific eye color, or possessing any character traits from the current case.

## Fixed Foundational Rules for Live-Action Conversion

- Fictional characters must be presented as original real-life human interpretations and must not imitate real celebrities, actors, or real individuals.
- Use realistic skeletal structure, joints, muscles, soft tissue, hands, feet, and body proportions appropriate to the identity and apparent age shown in the original artwork. Record the original artwork's visible body proportions as source facts, but set the live-action target to the fixed literal `9 head body proportion` regardless of the source ratio. This is the user-authorized proportion exception to source fidelity. Keep natural anatomy, apparent-age and identity cues, pose, camera and composition; do not stretch limbs, deform joints or invent obscured body details.
- A two-dimensional face must be reconstructed as realistic three-dimensional facial anatomy: the brow ridge, eye sockets, nasal root, nasal alae, cheekbones, cheek soft tissue, philtrum, lips, chin, and jaw must all have natural volume, thickness, and mutual occlusion relationships. Preserve the original artwork's face shape, facial-feature proportions, placement, and overall character while avoiding mechanical reproduction of a flat two-dimensional appearance.
- Every visible expression, gesture and full-body pose must be interpreted through a real biomechanical cause chain: intention and action -> skeletal alignment and joint range -> active and opposing muscle tension -> tendon and soft-tissue displacement -> skin compression or stretch -> balance, weight transfer, contact forces and secondary motion. Preserve the source-visible emotion, expression intensity, gaze, gesture, pose direction and silhouette intent. When literal anime geometry is not humanly achievable, specify only the smallest anatomically feasible translation; do not replace it with a different emotion, neutral pose or generic body language, and do not invent hidden anatomy or unsupported muscle definition.
- Skin must use regional realism: different facial regions should exhibit different pore visibility, microtexture, roughness, localized color variation, subsurface blood coloration, and reflectivity. These differences must follow the visible skin color in the original artwork and realistic photographic logic rather than appearing as a uniform layer of pore noise or a filter.
- Lighting must preserve realistic structural facial shadows. Fill light may only control contrast and must not flatten the natural depth relationships of the eye sockets, nasal alae, cheekbones, corners of the mouth, or jaw.
- Clothing must preserve the structure, colors, layering, and decorative placement visible in the original image and translate them into realistic, wearable, manufacturable materials and construction.
- Record the source-visible body ratio in `body.source_visible_proportion` when observable, or `""` when not. Do not put a conflicting source ratio into `immutable_anchor.body_proportions`; that anchor may lock only body traits other than the overridden ratio. `body.proportion` carries the fixed target literal `9 head body proportion`. Extreme facial-skin realism means genuine camera-resolved skin, not extra pores, scars, freckles or oversharpening. The final target is hyperrealistic photographic realism, while all source-visible identity and facial relationships stay locked. The following fixed rules must be fully expressed only within `character_conversion.realism_foundation`. `character`, `facial_features`, `lighting`, and `visual_translation_rules` must contain only observations from the currently uploaded original artwork and their corresponding specific translations, without repeating the same general rules.

## Required JSON structure

{
  "prompt_type": "Live-action conversion",
  "style": "Extreme Photographic Realism + Physically Plausible Lighting + Real-Camera Imaging; authentic human appearance, high-definition smartphone camera capture aesthetic, combination of natural light and high-definition studio lighting, cinematic quality, premium costume design presentation, overall clean, professional, and consistent.",
  "character_conversion": {
    "source": "",
    "target": "",
    "authority_chain": [
      "当前上传原图",
      "视觉事实层",
      "真人化解释层",
      "最终生成层"
    ],
    "evidence_confidence": {
      "clearly_visible": [],
      "high_confidence_inference": [],
      "cannot_confirm": []
    },
    "visual_facts": {
      "subjects": [],
      "face_and_identity": [],
      "eyes": [],
      "hair": [],
      "body_pose_and_contacts": [],
      "clothing_and_accessories": [],
      "biological_structures": [],
      "camera_composition_environment_lighting": []
    },
    "anime_rendering_artifacts": [],
    "uncertain_or_occluded": {
      "unknown": [],
      "occluded": [],
      "cropped_or_out_of_frame": [],
      "ambiguous": [],
      "do_not_infer": []
    },
    "fictional_character_notice": [],
    "character_dna": {
      "identity": [],
      "face": [],
      "hair_design": [],
      "body": [],
      "clothing": [],
      "accessories": []
    },
    "immutable_anchor": {
      "identity": [],
      "face_geometry_and_facial_relationships": [],
      "hair_design": [],
      "body_proportions": [],
      "clothing_design": [],
      "accessories": [],
      "pose_camera_composition": []
    },
    "uncertainty": {
      "unknown": [],
      "occluded": [],
      "do_not_infer": []
    },
    "reference_roles": [],
    "realism_translation_rules": [],
    "high_risk_translation_items": [],
    "visual_translation_rules": [],
    "realism_foundation": {
      "source_authority": "当前上传原画是角色 DNA、身份、姿态、镜头、构图、材质与可见光线的唯一视觉权威。固定目标 `9 head body proportion` 是唯一经用户授权的身体比例例外；源图可见比例必须单独作为观察记录，不得为实现九头身而改变其他 DNA。",
      "identity_extraction": "角色的性别呈现、表观年龄、种族或物种、肤色、五官颜色与特殊结构，只能从当前上传原画动态提取；未知信息不得预设或补全。",
      "presentation_quality": "极致摄影写实、物理可信光线和真实相机成像；呈现原创真实成人面孔、高清智能手机拍摄质感、自然光与高清影棚光结合、电影质感与高品质服装展示，整体干净、专业、一致。",
      "facial_anatomy_translation": "保留原画脸型、五官比例、位置关系与整体角色特征，把二维脸转译成真实三维结构：眉弓、眼窝、鼻根与鼻翼、颧骨、面颊软组织、人中、嘴唇、下巴和下颌均具有自然体积、厚度、过渡与遮挡关系；禁止生成扁平插画脸。",
      "human_biomechanics_translation": "所有可见表情、手势与姿态都必须通过可实现的骨骼排列、关节活动范围、主动肌与拮抗肌协同、肌腱与软组织位移、皮肤形变、平衡、重心转移、接触力和次级运动来解释。保留情绪、表情强度、视线、动作意图、姿态方向与轮廓；若字面动漫几何不可实现，只做最小的解剖可行调整，不得替换为不同表情或通用姿势。",
      "hair_dynamics_translation": "保留 Hair Design DNA，并以可信的头皮附着、生长方向、分层发束、密度、质量、柔韧性、重力、惯性，以及有画面依据的空气流动或湿度和头发与脸、身体、服装、配饰及其他发束的碰撞摩擦来解释发型。可通过真实修剪、绑扎、编发、克制定型或原图可见支撑实现的造型必须保留；否则只做维持可辨方向、体积层级与轮廓意图所需的最小物理调整。不得普通化设计、虚构环境力或复制僵硬动漫发块。",
      "regional_skin_realism": "面部皮肤采用极致摄影写实：具有自然分区差异与光学细节的可信真人皮肤，禁止插画、CG、瓷器或塑料外观。额头、眉间、鼻部、面颊、眼周、口周与嘴唇应具有不同的毛孔可见度、微纹理、粗糙度、局部色彩变化、皮下血色与反射率；禁止统一毛孔贴图、统一反射率或磨皮滤镜。整体皮肤细腻、干净、护理良好，不新增色斑、污渍或脏污感。",
      "structural_lighting": "光线必须保留支撑面部三维结构的自然阴影；补光不得抹平眼窝、鼻翼、颧骨、嘴角或下颌的深度关系，高光只出现在符合真实皮肤行为与骨性突起的位置。"
    }
  },
  "character": {
    "gender": "",
    "age": "",
    "appearance": {
      "face": {
        "type": "",
        "features": [],
        "expression": ""
      },
      "skin": {
        "material": "",
        "details": [],
        "avoid": ["skin discoloration", "skin patches", "dirty-looking skin"]
      }
    }
  },
  "hair": {
    "color": "",
    "style": [],
    "texture": [],
    "hair_design_dna": [],
    "illustration_shading_artifacts_rejected": [],
    "real_world_feasible_translation": [],
    "avoid": []
  },
  "facial_features": {
    "eyes": {
      "style": "",
      "features": [],
      "color": "",
      "avoid": []
    },
    "makeup": {
      "style": "",
      "details": [],
      "avoid": []
    }
  },
  "costume": {
    "style": "",
    "design": [],
    "color_design": [],
    "material": [],
    "construction": [],
    "adjustment": [],
    "avoid": []
  },
  "accessories": {
    "neck_accessory": {
      "design": [],
      "material": []
    },
    "waist_device": {
      "design": [],
      "material": [],
      "construction": []
    },
    "coat_graphics": {
      "design": [],
      "material": []
    },
    "footwear": {
      "design": [],
      "material": [],
      "detail": []
    }
  },
  "body": {
    "source_visible_proportion": "",
    "proportion": [
      "符合原画身份与表观年龄的真实人体解剖结构",
      "9 head body proportion"
    ],
    "anatomy_rules": [
      "保持真实人体骨骼结构",
      "保持解剖正确的关节结构",
      "保持真实肌肉与软组织分布",
      "保持真实四肢厚度",
      "保持真实手掌与手指比例",
      "保持真实足部比例",
      "只有真实镜头光学造成时才允许透视夸张",
      "不得为模仿动漫解剖而拉伸或扭曲身体"
    ],
    "pose": {
      "style": "",
      "position": [],
      "gesture": [],
      "perspective": [],
      "anatomical_constraints": []
    }
  },
  "environment": {
    "scene": "",
    "background": [],
    "visual_style": []
  },
  "camera": {
    "camera": "高清智能手机相机拍摄质感",
    "lens": "",
    "shot_type": "",
    "camera_angle": "",
    "camera_position": [],
    "composition": [],
    "perspective_control": [],
    "depth_of_field": "",
    "photography_style": ["摄影写实角色", "原创真实成人外观", "高清智能手机拍摄质感", "电影质感", "高品质服装设计展示"]
  },
  "lighting": {
    "style": "自然光与高清影棚光结合",
    "elements": ["自然光", "高清影棚光"],
    "lighting_behavior": ["保持电影质感", "保留原画可见的光线方向、明暗关系、高光与阴影逻辑"],
    "color_palette": []
  },
  "quality": {
    "resolution": "",
    "natural_optical_rendering": ["高分辨率不等于过度锐化", "自然焦平面清晰度与柔和光学衰减", "禁止过度清晰度、微对比、脆硬边缘、锐化光晕、夸张毛孔或根根过度定义的头发"],
    "render_style": ["摄影写实角色", "原创真实成人外观", "电影质感", "高品质服装设计展示"],
    "realism_priority": ["细腻真实皮肤纹理", "干净且护理良好、不新增色斑或斑块的皮肤", "整体干净、专业、一致"]
  },
  "negative_prompt": [
    "real celebrity likeness",
    "real actress likeness",
    "real person identity",
    "celebrity face",
    "face copying",
    "identity imitation",
    "anime face",
    "anime eyes",
    "oversized eyes",
    "cartoon",
    "manga",
    "illustration",
    "digital painting",
    "CG character",
    "3D render",
    "game character",
    "video game screenshot",
    "AI beauty face",
    "flat face",
    "flat facial anatomy",
    "facial features on one plane",
    "plastic skin",
    "waxy skin",
    "porcelain skin",
    "ceramic skin",
    "over-smoothed skin",
    "beauty filter",
    "uniform pore texture",
    "uniform skin roughness",
    "uniform skin tone",
    "skin blotches",
    "patchy skin tone",
    "dirty skin texture",
    "fake wig",
    "CG hair",
    "plastic hair",
    "solid metallic hair",
    "cheap cosplay",
    "plastic costume",
    "rubber costume",
    "flat painted fabric",
    "unreal fabric",
    "toy accessories",
    "plastic electronics",
    "unrealistic footwear",
    "toy-like platform sandals",
    "unreal anatomy",
    "anime anatomy",
    "distorted legs",
    "deformed thighs",
    "unnatural hips",
    "broken knees",
    "broken ankles",
    "bent limbs",
    "extra limbs",
    "extra fingers",
    "missing fingers",
    "deformed hands",
    "deformed feet",
    "excessive body stretching",
    "unrealistic body proportions",
    "artificially elongated legs",
    "anatomical deformation caused by perspective",
    "excessive sexualization",
    "voyeuristic framing",
    "pornographic composition",
    "unreal lighting",
    "flat lighting",
    "flattened facial shadows",
    "overexposed skin",
    "oversaturated colors",
    "low detail",
    "low resolution",
    "blurry face",
    "blurry costume",
    "poor texture quality"
  ]
}

