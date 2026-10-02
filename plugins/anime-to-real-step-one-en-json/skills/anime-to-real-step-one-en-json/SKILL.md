---
name: anime-to-real-step-one-en-json
description: "Analyze the currently uploaded anime, manga, illustration, or virtual-character reference through a strict visual-evidence layer, map the complete face through anatomically constrained human fitting, then return one isolated live-action conversion JSON object with English keys, Chinese descriptions, and fixed English negative prompts. Use only when the user explicitly invokes Step One."
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

- A later layer must not overwrite, beautify, normalize, delete or contradict an earlier supported fact. The literal source observation must remain intact in the evidence record; this does not require an anatomically impossible 2D magnitude to be copied as final human geometry. Every necessary anatomical correction must be explicit, minimal and traceable from source fact to live-action equivalent.
- The visual-evidence layer has no authority to redesign materials, correct proportions, beautify the face, invent anatomy, complete occlusion, or decide how the final live-action result should look.
- The realism-translation layer may explain how supported 2D design facts can become physically plausible, but must preserve the recorded visual facts and clearly expose every necessary exception or minimum correction.
- Step Two must receive both the current source image and the complete Step One JSON. JSON never replaces the source image as visual authority.

## Visual Evidence Extraction | must run before interpretation

Before writing any live-action rule, create an evidence ledger using exactly three confidence classes:

- `clearly_visible`: directly observable in the current image. Record only what is visible.
- `high_confidence_inference`: not fully shown but strongly supported by visible geometry, continuity or context. State the evidence and keep it separate from direct facts.
- `cannot_confirm`: occluded, cropped, too small, ambiguous, absent, or unsupported. Use `""` / `[]` in downstream descriptive fields and never complete it.

For each important observation, record its subject/region, Chinese description, confidence class and visible basis. Where useful, also record whether it is identity-critical. Do not use character names, IP knowledge, web references, cosplay, actors, other frames or prior messages to raise confidence.

The evidence pass must cover, when visible: subject count; apparent age and presentation; head and face outline; brow and orbital placement; eye aperture, iris-to-visible-eye ratio, dark-iris/dark-eye proportion, eyelid relationships and optical highlights; nasal root, bridge, alae and tip; cheek and midface volume; philtrum, lips, mouth corners and jaw opening; chin, jawline, ears and visible asymmetry; skin color as depicted; non-human structures; hair base color, silhouette, volume, parting, locks and accessories; body orientation, source-visible proportion, pose, balance and contact; clothing layers, colors and visible construction; accessories and biological structures; composition, camera, environment and lighting. Raw two-dimensional positions and proportions, including exaggerated absolute facial dimensions, are source evidence rather than the final live-action geometry. At this stage, tag potential identity cues only as feature type, direction, relative trend, visual weight, identity semantics and supported asymmetry; do not set their final size or placement. During interpretation, establish the human anatomical foundation first, then read these tagged cues as Face DNA and map them onto that base. Never promote a stylized absolute size into the final human measurement.

During this pass, never:

- beautify, rejuvenate, feminize/masculinize, normalize or create an AI beauty/influencer face;
- shrink or enlarge eyes, nose, mouth, ears, jaw, head or body to fit realistic averages;
- turn painted highlight bands, cel-shading, graphic shadow blocks or outline edges into dye, physical streaks, rigid hair slabs, skin marks or material seams;
- count every illustrated hair-direction line, separated graphic lock, ribbon highlight or dense flyaway mark as a literal real strand or independently bounded hair strip when it functions only as a cue for flow, softness, motion, layering or airiness;
- replace illustrated skin, hair or fabric with newly designed realistic materials;
- add/delete clothing parts, decorations, props, accessories, limbs, ears, tails, horns or other biological structures;
- infer a hidden back, sole, inner layer, occluded hand, body surface or attachment mechanism.

Only after the evidence ledger is complete may the same JSON add `character_dna`, `realism_translation_rules` and `high_risk_translation_items`.

For detailed invocation, field meanings, stage boundaries, common errors, backup and recovery, read [references/usage-guide-zh.md](references/usage-guide-zh.md).

## Source-image lock and layer separation

Separate source-visible design from illustration-only rendering and from realism decisions. `visual_facts` and the detailed top-level `character`, `hair`, `facial_features`, `costume`, `accessories`, `body`, `environment`, `camera` and `lighting` fields hold source evidence. `anime_rendering_artifacts` records 2D depiction mechanisms without treating them as physical design. `character_conversion.character_dna` is a brief identity-critical index, not a copy of all descriptions. `immutable_anchor` locks supported identity-defining facts, not unknowns. For the face, `immutable_anchor.face_geometry_and_facial_relationships` preserves source evidence, identity-bearing directions, ordering, relative tendencies and supported asymmetry; it must not freeze a literally impossible 2D magnitude as final human geometry. The validated final human geometry belongs in `facial_anatomy_mapping.live_action_identity_lock`. `uncertain_or_occluded` and legacy `uncertainty` record unknown, occluded and do-not-infer fields; use `""` or `[]` in ordinary fields where the source is indeterminate. `realism_translation_rules` contains only later-stage translation decisions. `capture_conditioned_realism` converts already supported evidence into scale-aware regional material, imaging and visual-acceptance constraints; it is not a new evidence source and cannot promote an unknown into a positive instruction. Never convert an illustration shading artifact into a physical feature.

## Cross-style stability gate

### Canonical records and compact handoff

Do not create competing descriptions of one attribute. `visual_facts` is the canonical observation ledger; detailed top-level fields expand only their relevant region with the same evidence and confidence. `character_dna`, `immutable_anchor` and `preserve_features` are concise indexes of those facts, not independent reinterpretations. `facial_anatomy_mapping.live_action_identity_lock` is canonical for the fitted human face; `shot_expression_lock` is canonical for expression. `medium_translation_analysis` owns source-to-physical conversion; `capture_conditioned_realism` owns scale-aware implementation. Legacy `visual_translation_rules` may summarize those decisions but cannot add a second conversion. Resolve accidental contradictions before emitting JSON; genuine ambiguity remains unknown.

After the other fields are complete, add `character_conversion.execution_summary`: `priority_anchors`, `allowed_translations`, `case_rejections`, and `unresolved_conflicts` arrays. Each populated item uses `source_paths` (existing JSON Pointer paths), `instruction` (concise Chinese), and `confidence` (the existing enum). Usually select 6–12 priority anchors across identity, hair, garment, pose and framing; use fewer if evidence is sparse. Refer to canonical fields instead of recopying long general rules. This is a derived navigation index, not new authority. Never resolve uncertainty by writing an assertion in the summary. Old JSON without this optional object remains valid for Step Two.

Keep the legacy English `negative_prompt` list unchanged for compatibility, but it is a scoped defect vocabulary, not a ready-to-send ban list. Specify relevant restrictions in `negative_prompt_scope`: `oversized eyes` / `oversized irises` reject anatomically impossible geometry, not supported large-eye identity; `bent limbs` rejects broken anatomy, not a bent joint; `plastic costume` / `rubber costume` reject misrendered material, not an explicitly supported polymer garment. Do not suppress natural skin variation, supported highlights or focal softness. Generic wording must never overrule specific source evidence.

### Garment evidence before textile rendering

Keep garment design facts in `costume`: outline, layering, coverage, colors, pattern placement, visible seams, closures, hems and wearing state. Distinguish a drawn fold, a printed stripe, a seam and a highlight; ambiguous marks remain ambiguous. A flat-painted region does not establish cotton, silk, wool, leather or synthetic fiber identity.

For visible garments, expand existing `capture_conditioned_realism.material_response_map` entries with `material_identity_evidence`, `optical_response_class`, `construction_lock`, `fold_force_map`, `texture_scale_and_direction`, `edge_thickness_and_contact`, `specular_and_transmission`, and `garment_failure_signs`. Use empty values for unsupported identity or hidden construction. Describe observable response classes (matte / directional sheen / glossy / translucent) before naming fibers. Relate folds to visible suspension, compression, bending or contact; record dominant fold locations and contact points for preservation. Microstructure follows fabric curvature, perspective and focus, not a screen-space texture overlay. Surface rendering may explain existing cloth; it must not add seams, pockets, weave patterns, embroidery, wear, transparency or new garment shapes. Step One does not read real-person photographs or the photographic profile.

Apply the same evidence standard before interpreting any source style. Cel shading, painterly rendering, grayscale manga, sparse line art, chibi exaggeration, semi-realistic illustration, 3D/CG-like rendering and heavy color grading may change how evidence is depicted, but they must not change which visible facts count as Character DNA. A broad style label is never enough to justify a character, anatomy, material, camera or lighting decision.

For every identity-critical region, separate the supported design from the depiction mechanism across these axes when visible:

- contour and linework: outlines, hatching, edge accents and graphic separations;
- geometry and proportion: source-visible relationships versus style-amplified absolute magnitudes;
- value, shading and highlights: physical form cues versus cel boundaries, painted patches or ribbon highlights;
- color: supported local color relationships versus global grading, bloom or palette compression;
- texture and material: visible construction cues versus omitted, simplified or brush-painted surface detail;
- camera and scene: supported pose, framing, perspective, light direction and atmosphere versus non-photographic projection or rendering conventions.

Record the literal source observation first. Preserve the supported design semantics, route only the depiction mechanism into `anime_rendering_artifacts` and `medium_dna`, and describe any required physical conversion explicitly. Sparse or highly stylized information lowers confidence; it never authorizes generic beauty, default materials, invented anatomy, invented lens behavior or invented lighting. If design and depiction cannot be separated, keep the item ambiguous and do not discard or positively translate it.

**Face Foundation First.** After the evidence ledger is complete, establish an apparent-age-appropriate human facial base before deciding final geometry: cranial and facial bones, brow and orbital cavities, eyeballs and eyelids, nasal bones and cartilage, zygomatic support, maxilla and mandible, facial muscles, fat compartments and other soft tissues must form one physically coherent head. Then map source Face DNA—feature type, direction, relative trend, visual weight, identity semantics and supported asymmetry—onto that base. The source's stylized absolute dimensions remain evidence only; they never become the geometric foundation.

**Skin Structure First.** Realistic skin is a regional biological and optical system, not a smooth color layer with added pore noise. The live-action interpretation must use natural fine vellus hair, region-dependent pores and microtexture, local roughness and sebum differences, subsurface blood coloration, only very faint anatomically appropriate vascular influence, subsurface scattering, and angle-dependent changes in texture contrast, highlight shape and reflectance. Never manufacture discrete veins, redness, blemishes or dirt that are not visible source facts. Avoid uniform pores, uniform sharpening, uniform reflectance, plastic, wax, porcelain or ceramic skin, and beauty-filter smoothing.

**Hair Intent First.** Hair Design DNA includes visible base color, length, parting, bangs, silhouette, volume hierarchy, curl/straight tendency, major flow, tied structures, distinctive face-framing locks, supported flyaway intent and hair accessories. Read illustrated lines and separated shapes as visual language for flow, softness, movement, layering and airiness before treating them as physical structure. Preserve the hairstyle outline, length, parting, bangs, main direction, volume hierarchy, key face-framing locks and the intended fluid/soft character, while actively weakening illustration-only over-segmentation, hard strip boundaries, ribbon-like highlight bands and densely drawn flyaway marks. Real-world translation must form continuous, natural, soft hair masses with plausible growth, gravity, density, restrained strand variation and natural specular response; it must not become a wig, row-by-row or strip-by-strip hair, hard hair plates, plastic hair or CG hair. If design and depiction cannot be separated confidently, record the ambiguity and do not discard it.

Large irises, a high dark-eye/iris proportion, strong wet mirror reflection, unusual eye spacing or eye angle may be identity-critical source evidence when visibly supported. Record the literal source fact first, but do not treat a non-human absolute magnitude as an immutable live-action measurement. Preserve the identity semantics—eye shape, angle, spacing tendency, gaze, color, expression and relative prominence—then fit the eyeballs, orbits, eyelids, iris exposure and surrounding tissue to the closest apparent-age-appropriate humanly feasible result. Human anatomy is a hard boundary for final geometry; a generic average or beauty-template face has no authority to replace the source identity.

## Facial structure analysis and anatomically constrained live-action fitting

Run this pipeline only after the visual-evidence ledger is complete:

`source-visible evidence -> feature topology -> pose and perspective normalization -> human anatomical foundation -> Face DNA mapping -> muscle, soft-tissue and skin response -> identity verification -> live-action identity lock`

This is facial structure analysis and anatomically constrained reconstruction, not biometric identification. Never match the character to a real-person database, celebrity, actor, demographic stereotype, beauty ideal, golden ratio or generic average face.

1. **Source feature topology:** Map the visible relationships among the head/face outline, forehead and brow, bony orbital region, eyes and eyelids, nose, cheekbones and midface, philtrum and mouth, chin and jaw, ears, and supported left-right asymmetry. Record visible positions, directions, overlap and relative tendencies in a head-local relationship map. Do not convert them yet.
2. **Pose and perspective normalization:** Explain how head yaw, pitch, roll, expression, foreshortening, lens perspective and cropping affect the visible positions. Normalize relationships only to reason about structure; never invent a frontal face, hidden landmark or unseen side.
3. **Human anatomical foundation:** Before setting the final size or placement of any feature, establish the closest plausible human skull and facial-bone base, orbital cavities, eyeball/eyelid system, nasal bone/cartilage support, zygomatic structure, maxilla, mandible, facial muscles, fat compartments and other soft tissues appropriate to the source-supported apparent age. All features must share this coherent support and mutual depth/occlusion system. Human anatomical feasibility constrains final absolute geometry, but it must not authorize beautification or generic normalization.
4. **Face DNA mapping and identity-semantic preservation:** Map the source's feature type, directional character, relative trend, visual weight, distinctive relationship, supported asymmetry, gaze and expression onto the human foundation. These semantics, rather than literal 2D scale, determine the closest identity-preserving result. When a 2D magnitude is impossible, apply only the minimum correction required for a real human face and state which identity cue survives.
5. **Muscle, soft-tissue and skin response:** Interpret supported expression through coordinated brows, eyelids, eyes, cheeks, nose, lips, jaw, neck and visible skin compression/stretch. Use FACS-compatible visible-action language when useful, but do not guess Action Units that the image cannot support. Texture follows structure and motion; it cannot conceal incorrect geometry.
6. **Two separate locks:** `live_action_identity_lock` stores the stable, validated human face structure after fitting. `shot_expression_lock` stores the current expression, gaze, eyelid state, mouth/jaw action and temporary soft-tissue deformation. Never make a transient expression deformation part of permanent identity geometry.
7. **Explicit correction record:** For every non-human or ambiguous source magnitude that requires adjustment, pair the source observation with its closest humanly feasible result and the identity cue that must survive. Put unresolved regions in uncertainty rather than forcing a fit.

The priority rule is fixed: the source image is authoritative about what is visibly drawn; human anatomy is authoritative about what final human geometry can physically be; source identity semantics determine the closest plausible mapping; beauty averages have no authority. Lock the face only after the anatomical fit and identity verification pass.

Hair dynamics must be derived from Hair Intent First and a real physical cause chain rather than copied as frozen anime geometry: scalp attachment and growth direction -> coherent layered flow and restrained strand groups -> density, length, mass, flexibility and supported styling -> gravity, head/body motion, inertia, supported air movement or moisture -> collision and friction with the face, shoulders, body, clothing, accessories and other hair -> the final continuous visible shape. Treat repeated 2D linework as directional encoding, not strand count. Record the source hairstyle silhouette, major flow, volume hierarchy and identity-defining face-framing locks separately from illustration-only segmentation. If a source shape is feasible through real cutting, braiding, tying, restrained styling product or a visible accessory, retain it. If it cannot physically persist, prescribe only the smallest feasible adjustment that preserves its recognizable direction, soft/fluid character, volume hierarchy and silhouette intent; never normalize it into an ordinary hairstyle. Do not invent wind, wetness, stiffness, hidden supports, motion or extra flyaways that the current image does not support, and do not translate every drawn split into a hard lock, strip or hair plate.

`natural_optical_rendering` must specify high resolution through real structural information and lens behavior. High resolution != oversharpening. Avoid excessive clarity, micro-contrast, crispy edges, sharpening halos, exaggerated pores, hyper-defined hair strands, uniform sharpness and artificial fabric-fiber emphasis. Skin, hair and materials must remain naturally resolved, with focal-plane detail and gentle optical roll-off.

## Capture-conditioned photographic realism | additive execution layer

After `facial_anatomy_mapping`, populate `character_conversion.capture_conditioned_realism`. Keep every listed array present and use `[]` when the current image cannot support a decision. This layer does not claim that the source artwork already contains photographic pores, sensor noise, real fabric fibers or camera metadata. It converts supported design and composition into an executable photographic specification while preserving the evidence boundary.

When an array is populated, each entry must be a JSON object with English keys and Chinese values. Use the shared keys `region`, `source_evidence`, `confidence`, `resolvable_scale`, `physical_behavior`, `must_preserve`, `must_not_invent` and `failure_signs`; add only field-specific keys that materially improve execution. `source_evidence` must cite the current image, and `confidence` must use the existing three confidence classes. `resolvable_scale` states whether the final framing can plausibly resolve macro structure, medium-scale variation or microdetail.

- `source_capture_limitations`: record crop, occlusion, low pixel coverage, stylized simplification, painted blur, compression-like artifacts or ambiguous light only when visible; never invent EXIF, focal length, aperture, ISO or a sensor model.
- `resolution_visibility_gate`: bind pores, vellus hair, fine lines, individual hair strands, fabric fibers, tiny scratches, noise and grain to subject pixel coverage, focus, depth of field, motion, exposure and supported image quality. If the final framing cannot resolve a detail, omit it rather than enlarge or sharpen it.
- `skin_region_map`: distinguish forehead, glabella, nose and alae, cheeks, eye area, mouth area, lips, jaw, neck and any other visible skin by relative microtexture, roughness, sebum, local color variation, highlight shape and shadow transition. Treat subsurface scattering as low-frequency light transport, never as global glow, generalized redness or waxy translucency. Natural physiological variation is allowed; fixed negatives such as `skin blotches` and `patchy skin tone` reject unsupported large, abrupt or repeated artifacts, not subtle regional skin variation.
- `hair_multiscale_map`: specify macro silhouette and mass, irregular medium-scale groups, root density and opacity, and sparse focal-plane or silhouette-edge strands. Preserve the Hair Design DNA while preventing both strip-by-strip hair and a single polished hair sheet. Minimal edge irregularity that does not change silhouette, parting, bangs, key locks or identity may be used as neutral physical rendering; it is not permission to invent a new flyaway hairstyle, wind or motion.
- `face_physical_structure_map`: make the existing face fit executable by recording supported eyelid thickness and overlap, eyeball seating, nasal bridge-tip-ala support, philtrum, lip volume and wet/dry transition, perioral tissue, cheek and jaw continuity, ears, and pose-normalized supported asymmetry. It must agree with `facial_anatomy_mapping` and cannot introduce a new identity.
- `body_soft_tissue_map`: record only visible mass, joint support, weight bearing, compression, stretch and contact. Do not infer hidden anatomy or use idealized muscle definition to create realism.
- `material_response_map`: for each visible material, record relative thickness, edge behavior, seam or construction evidence, stretch or compression direction, fold cause, drape, translucency and specular width. Do not assign an unsupported material merely because the artwork is flat.
- `camera_imaging_pipeline`: record only source-supported perspective, focus target, sharpness hierarchy, depth relationships, exposure relationships, mixed-light or white-balance behavior, highlight clipping or roll-off, shadow floor, motion blur and capture artifacts. Unconfirmed photographic settings remain empty for Step Two's neutral execution choice.
- `environment_light_transport`: connect visible environment lights and reflective surfaces to color spill, secondary reflection, contact shadow and occlusion on the subject. Do not create a new light source or reflective surface.
- `negative_prompt_scope`: state which broad fixed negatives reject synthesis defects only. Natural defocus, motion blur, sensor-like noise, compression, subtle skin color variation and locally soft detail are not `low detail`, `blurry face`, `blurry costume`, `skin blotches` or `patchy skin tone` when physically supported.
- `body_proportion_override_gate`: retain the fixed literal `9 head body proportion` for contract compatibility, but permit visible enforcement only when reliable full-body evidence is available and no change to pose, camera, crop, balance, contact, occlusion or unknown anatomy is required.
- `visual_acceptance_criteria`: define image-specific PASS/FAIL signs for face identity, skin regionality, hair hierarchy, body mechanics, material separation, optical sharpness and environment coupling. Generation success alone is never a visual PASS.

The currently uploaded original artwork is the sole visual authority for this analysis. Extract only the pose, orientation, camera angle, composition, background, environment, lighting direction, color temperature, shadows, highlights, atmosphere, and character DNA actually visible in the original artwork. Live-action conversion may only translate the two-dimensional representation into believable real human anatomy, skin, hair, clothing materials, props, environment, photography, and lighting, and must not alter the design identity already established by the original artwork.

The character's gender, apparent age, race/species, skin color, facial-feature colors, and special physiological structures must be dynamically extracted from the currently uploaded original artwork. If they are not visible or cannot be determined, use an empty string or empty array. Do not preset the character as female, adult, having a specific skin color, specific eye color, or possessing any character traits from the current case.

## Fixed Foundational Rules for Live-Action Conversion

- Fictional characters must be presented as original real-life human interpretations and must not imitate real celebrities, actors, or real individuals.
- Use realistic skeletal structure, joints, muscles, soft tissue, hands, feet, and body proportions appropriate to the identity and apparent age shown in the original artwork. Record the original artwork's visible body proportions as source facts and retain the fixed literal `9 head body proportion` as the user-authorized proportion exception. Step Two may visibly enforce that target only when `body_proportion_override_gate` confirms reliable full-body evidence and no change to pose, camera, crop, balance, contact, occlusion or unknown anatomy is required. Otherwise preserve the current visible anatomy and framing; do not stretch limbs, shrink the head, deform joints or invent obscured body details.
- A two-dimensional face must be reconstructed on a real human foundation first: cranial and facial bones, brow ridge and eye sockets, eyeballs and eyelids, nasal bones and cartilage, nasal alae, cheekbones, maxilla, mandible, facial muscles, facial fat, cheek and perioral soft tissues, philtrum, lips, chin, jaw and ears must form one supported three-dimensional system. Only then map the original artwork's Face DNA—feature types, ordering, directions, relative trends, visual weights, identity semantics and supported asymmetry—onto that base. Do not mechanically preserve a flat or anatomically impossible 2D absolute magnitude; route it through `facial_anatomy_mapping` and apply only the closest plausible human result.
- Every visible expression, gesture and full-body pose must be interpreted through a real biomechanical cause chain: intention and action -> skeletal alignment and joint range -> active and opposing muscle tension -> tendon and soft-tissue displacement -> skin compression or stretch -> balance, weight transfer, contact forces and secondary motion. Preserve the source-visible emotion, expression intensity, gaze, gesture, pose direction and silhouette intent. When literal anime geometry is not humanly achievable, specify only the smallest anatomically feasible translation; do not replace it with a different emotion, neutral pose or generic body language, and do not invent hidden anatomy or unsupported muscle definition.
- Skin must use real biological structure and regional optical realism: natural fine vellus hair; different pore visibility and microtexture across the forehead, glabella, nose, cheeks, eye area, mouth area and lips; local roughness and sebum variation; subsurface blood coloration; only very faint, anatomically appropriate vascular influence in plausible thin-skin regions; and believable subsurface scattering. Texture contrast, highlights and reflectance must change with region and light angle. These differences follow the source-visible skin color and photographic logic, never a uniform pore layer, uniform sharpening or global filter. Do not create plastic, wax, porcelain or ceramic skin, beauty-filter smoothing, generalized redness, explicit unsupported veins, blemishes or dirt.
- Hair must follow Hair Intent First: preserve silhouette, length, parting, bangs, main flow, volume hierarchy, key face-framing locks and the intended soft, fluid, airy character, while treating excessive illustrated splits, hard strip boundaries, ribbon highlights and dense drawn flyaways as depiction cues rather than a literal strand inventory. The real result must be continuous, naturally grouped and softly resolved; avoid a wig, individually separated rows/strips, hard hair plates, plastic hair, CG hair and root-by-root over-definition.
- Lighting must preserve realistic structural facial shadows. Fill light may only control contrast and must not flatten the natural depth relationships of the eye sockets, nasal alae, cheekbones, corners of the mouth, or jaw.
- Clothing must preserve the structure, colors, layering, and decorative placement visible in the original image and translate them into realistic, wearable, manufacturable materials and construction.
- Record the source-visible body ratio in `body.source_visible_proportion` when observable, or `""` when not. Do not put a conflicting source ratio into `immutable_anchor.body_proportions`; that anchor may lock only body traits other than the overridden ratio. `body.proportion` carries the fixed target literal `9 head body proportion`. Extreme facial-skin realism means genuine camera-resolved skin, not extra pores, scars, freckles or oversharpening. The final target is scale-aware photographic realism, while all source-visible identity and facial relationships stay locked. The complete positive foundation rules must be defined only within `character_conversion.realism_foundation`. For enforcement, only `character.appearance.skin.avoid` and `hair.avoid` may mirror concise rejection labels drawn from those foundations; they must not invent source facts or create a second translation system. All other content in `character`, `facial_features`, `lighting`, and `visual_translation_rules` must contain only observations from the currently uploaded original artwork and their corresponding specific translations, without repeating the same general rules.

## Medium translation analysis | additive Step One layer

After the evidence ledger, add `character_conversion.medium_translation_analysis` without removing or renaming any existing field. Keep all six arrays present, using `[]` when no item is supported. This layer classifies existing evidence; it does not create new visual facts. Each populated entry is a concise Chinese description that names the affected region, the visible basis or supported rendering artifact, its confidence class, the design meaning that survives, and the required treatment; include the controlling current reference from `reference_roles` when multiple images are assigned. Do not use a style name by itself as evidence. Never promote `high_confidence_inference` to `clearly_visible`; never place `cannot_confirm` content in a positive preserve, translate, discard or equivalent instruction. Unknowns remain in `uncertain_or_occluded` and `uncertainty`.

- `character_dna`: identity-bearing source design from `visual_facts` and the existing `character_dna`; do not place drawing technique here.
- `medium_dna`: 2D depiction mechanisms from `anime_rendering_artifacts`, such as outlines, cel-shading boundaries, painted highlights, repeated hair-direction strokes, graphic lock separations and ribbon-like hair highlights, not physical hair dye, separate real strands, skin marks or garment details.
- `preserve_features`: visible identity semantics, humanly feasible face relationships, hair-design intent, costume structure, color hierarchy, accessories, pose and other supported anchors that must survive; align with `immutable_anchor`. For the face, preserve feature type, order, direction, relative tendency, visual weight, identity semantics and supported asymmetry, not an impossible absolute size. For hair, preserve silhouette, length, parting, bangs, major flow, volume hierarchy, key face-framing locks and supported softness/fluidity/airiness rather than every drawn split.
- `translate_features`: only supported features whose 2D representation needs a physically feasible anatomy, material, hair or optical equivalent; preserve the underlying design and allow only the minimum necessary conversion. Any source facial magnitude outside plausible human anatomy belongs here and in `facial_anatomy_mapping.minimum_anatomical_corrections`, paired with the identity semantics that must survive. Hair entries must translate depiction lines into coherent real flow and natural grouping, not one-to-one physical strips. The existing fixed `9 head body proportion` exception remains explicit and does not authorize changes to other DNA.
- `discard_features`: only proven illustration-only marks that must not survive as literal physical marks or boundaries. This can include confirmed excess hair segmentation or painted ribbon highlights as physical structure, but never the supported hairstyle intent beneath them. Never discard a supported character feature, accessory, seam, color area, prop or uncertain detail.
- `real_world_equivalents`: one-to-one Chinese mapping from each `translate_features` item to its closest plausible physical result. Do not add wear, sweat, dirt, flyaways, makeup, materials or unseen construction without evidence.

When an item could be both design and depiction, preserve the supported design and classify only its rendering artifact as medium DNA; if separation is uncertain, record ambiguity and do not discard it. Flat color is not proof of flat material, missing linework is not proof that a structure is absent, painterly texture is not physical surface texture, and 3D/CG shading is not a real-camera lighting instruction. For multiple current images, classify each field under its assigned `reference_roles` authority, never borrow another image's role or historical material. Keep `realism_translation_rules` and legacy `visual_translation_rules` consistent with this added layer. All pre-existing JSON keys, fixed `negative_prompt` entries and foundational rules remain intact.

Populate `character_conversion.facial_anatomy_mapping` after `medium_translation_analysis`. Every populated entry must use Chinese and trace back to current-image evidence. `source_feature_topology` records relationships rather than a beauty judgment and retains stylized absolute sizes only as source facts. `pose_and_perspective_normalization` may explain distortion but may not invent a canonical frontal face. `human_anatomy_fit` first establishes the coherent human skull/orbit/eyeball-eyelid/nasal bone-cartilage/zygomatic/maxilla-mandible/muscle/fat/soft-tissue foundation, then fits the source Face DNA to it. `identity_semantics_preserved` names the feature types, directions, relative trends, visual weights, supported asymmetry and other cues that make the character recognizable. `minimum_anatomical_corrections` records explicit source-to-human deltas without beautification. `muscle_soft_tissue_skin_response` connects expression to physical response and cannot hide a wrong structural fit with texture. `live_action_identity_lock` and `shot_expression_lock` must remain separate. `rejection_checks` lists concrete full-face failures, including literal scaling of non-human dimensions, features floating on one plane, generic beauty normalization, unsupported soft-tissue structure and loss of identity semantics. Use `[]` where evidence is insufficient.

## Required JSON structure

{
  "prompt_type": "Live-action conversion",
  "style": "Photographic Realism + Physically Plausible Lighting + Real-Camera Imaging; preserve source-supported camera angle, composition, lighting direction, color relationships and atmosphere; use neutral real-camera behavior only where the source does not determine a photographic property.",
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
    "execution_summary": {
      "priority_anchors": [],
      "allowed_translations": [],
      "case_rejections": [],
      "unresolved_conflicts": []
    },
    "medium_translation_analysis": {
      "character_dna": [],
      "medium_dna": [],
      "preserve_features": [],
      "translate_features": [],
      "discard_features": [],
      "real_world_equivalents": []
    },
    "facial_anatomy_mapping": {
      "source_feature_topology": [],
      "pose_and_perspective_normalization": [],
      "human_anatomy_fit": [],
      "identity_semantics_preserved": [],
      "minimum_anatomical_corrections": [],
      "muscle_soft_tissue_skin_response": [],
      "live_action_identity_lock": [],
      "shot_expression_lock": [],
      "rejection_checks": []
    },
    "realism_translation_rules": [],
    "high_risk_translation_items": [],
    "visual_translation_rules": [],
    "realism_foundation": {
      "source_authority": "当前上传原画是角色 DNA、身份、姿态、镜头、构图、材质与可见光线的唯一视觉权威。固定目标 `9 head body proportion` 是唯一经用户授权的身体比例例外；源图可见比例必须单独作为观察记录，不得为实现九头身而改变其他 DNA。",
      "identity_extraction": "角色的性别呈现、表观年龄、种族或物种、肤色、五官颜色与特殊结构，只能从当前上传原画动态提取；未知信息不得预设或补全。",
      "presentation_quality": "受成像尺度约束的摄影写实、物理可信光线和真实相机成像；呈现原创、不可识别且符合原画表观年龄的人类面孔。原图明确的镜头角度、构图、光线方向、色温、明暗关系与氛围优先；只在原图无法确定的摄影属性上使用中性真实相机行为，不预设手机焦段、影棚布光、电影机或时尚写真风格。",
      "facial_structure_pipeline": "先完整记录源图可见五官证据、拓扑、姿态与透视影响；进入真人化解释时，先建立符合表观年龄且内部一致的真人头骨与颅面、眼眶、眼球眼睑、鼻骨鼻软骨、颧骨、上颌下颌、面部肌肉、脂肪与软组织基础，再把源图 Face DNA 映射到该基础；随后连接肌肉、软组织与皮肤响应，执行身份回检后分别形成真人身份结构锁与当前镜头表情锁。",
      "facial_geometry_priority": "源图决定可见事实与身份语义；真人解剖决定最终几何基础与可行范围；Face DNA 只提取五官类型、方向、相对趋势、视觉权重、身份语义与有证据支持的不对称，并决定最接近的真人映射。二维夸张绝对尺寸必须作为源图证据保留，但不得成为最终真人几何基础；审美平均值、黄金比例、网红脸、名人或真实人物模板均无权覆盖。",
      "facial_anatomy_translation": "先以真人头骨、颅面骨性支撑、眼眶与眼球眼睑、鼻骨鼻软骨、颧骨、上颌下颌、面部肌肉、脂肪和软组织建立可行的三维面部，再映射原画脸型倾向、五官类型、顺序、方向、相对突出趋势、视觉权重、可见不对称与整体身份语义；眉弓、眼窝、眼睑、鼻根鼻翼、颧面、面颊、口周、人中、嘴唇、下巴、下颌和耳部必须具有连续支撑、体积、厚度、过渡与遮挡关系。禁止按二维夸张绝对尺寸直接缩放真人五官，禁止扁平插画脸、通用美人脸或以皮肤纹理掩盖结构错误。",
      "human_biomechanics_translation": "所有可见表情、手势与姿态都必须通过可实现的骨骼排列、关节活动范围、主动肌与拮抗肌协同、肌腱与软组织位移、皮肤形变、平衡、重心转移、接触力和次级运动来解释。保留情绪、表情强度、视线、动作意图、姿态方向与轮廓；若字面动漫几何不可实现，只做最小的解剖可行调整，不得替换为不同表情或通用姿势。",
      "hair_dynamics_translation": "Hair Intent First：先读取原画头发真正表达的轮廓、长度、分缝、刘海、主要流向、体积层级、关键脸侧发束以及柔顺、飘逸、层次与空气感，再以真实头皮附着、生长方向、连续发流、自然群组、密度、质量、柔韧性、重力、惯性、受证据支持的环境力和碰撞摩擦来实现。二维中的多条走向线、过多分束、条带边界、丝带状高光与密集碎发常是视觉提示，不得逐条物理化；应主动弱化仅属画法的过度分割，同时保留发型设计语义。结果必须自然连续、柔顺、有空气感，禁止假发感、一束一束或一溜一溜的硬分条、硬边发片、塑料发与 CG 发。",
      "regional_skin_realism": "Skin Structure First：面部皮肤先遵循真实人类皮肤结构与光学，再承载角色可见肤色。额头、眉间、鼻部、面颊、眼周、口周与嘴唇应具有自然细小绒毛、不同的毛孔可见度与微纹理、局部粗糙度和油脂差异、皮下血色、只在合理部位极轻微呈现的皮下血管影响、可信亚表面散射及不同反射率；纹理对比、高光形状和反射必须随部位、曲率与光线角度自然变化。禁止统一毛孔、统一粗糙度、统一锐化、统一反射、塑料皮、蜡像皮、瓷器皮、磨皮滤镜、整体发红、明显无依据血管或用夸张毛孔代替真实感；保留受原图与光照支持的低对比区域差异；不主动新增色斑、污渍，也不通过磨皮清除已有身份标记。",
      "structural_lighting": "光线必须保留支撑面部三维结构的自然阴影；补光不得抹平眼窝、鼻翼、颧骨、嘴角或下颌的深度关系。皮肤微纹理对比、绒毛边缘光、高光形状与反射强度必须随入射角、观察角、区域粗糙度和骨性曲率自然变化，不得用统一高光、全脸同等锐度或整体泛红制造真实感。"
    },
    "capture_conditioned_realism": {
      "source_capture_limitations": [],
      "resolution_visibility_gate": [],
      "skin_region_map": [],
      "hair_multiscale_map": [],
      "face_physical_structure_map": [],
      "body_soft_tissue_map": [],
      "material_response_map": [],
      "camera_imaging_pipeline": [],
      "environment_light_transport": [],
      "negative_prompt_scope": [],
      "body_proportion_override_gate": [],
      "visual_acceptance_criteria": []
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
        "avoid": ["统一毛孔纹理", "统一皮肤粗糙度", "全脸统一锐化", "塑料皮肤", "蜡像皮肤", "瓷器皮肤", "磨皮滤镜", "整体皮肤发红", "无依据的明显皮下血管", "皮肤斑块", "脏污感皮肤"]
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
    "avoid": ["假发感", "一束一束的硬质发条", "一溜一溜的条带式分束", "硬边发片", "丝带状绘制高光的字面物理化", "密集插画碎发的逐条物理化", "塑料头发", "CG头发", "根根过度定义"]
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
    "camera": "",
    "lens": "",
    "shot_type": "",
    "camera_angle": "",
    "camera_position": [],
    "composition": [],
    "perspective_control": [],
    "depth_of_field": "",
    "photography_style": []
  },
  "lighting": {
    "style": "",
    "elements": [],
    "lighting_behavior": [],
    "color_palette": []
  },
  "quality": {
    "resolution": "",
    "natural_optical_rendering": ["高分辨率不等于过度锐化", "微细节必须服从主体像素占比、焦平面、景深、运动、曝光和图像质量", "自然焦平面清晰度与柔和光学衰减", "允许物理可信的焦外柔化、运动模糊、轻微噪点、降噪与压缩痕迹", "禁止统一锐化、过度清晰度、微对比、脆硬边缘、锐化光晕、夸张毛孔、用整体泛红伪造皮下血色，或把头发逐束切割并根根过度定义"],
    "render_style": ["受成像尺度约束的摄影写实", "物理可信光线", "真实相机成像", "原创且符合原画表观年龄的真实人物外观"],
    "realism_priority": ["先建立真人颅面基础，再映射 Face DNA 并通过身份回检的完整五官结构", "具有自然绒毛、分区微纹理、皮下血色与真实光学响应的皮肤", "连续柔顺、保留设计意图而不逐条物理化二维线条的真实头发", "区域肤色和反射连续，不新增色斑或整体泛红，不以磨皮消除正常差异", "各材质受同一场景光照、透视与焦点约束，不统一锐化或商业精修"]
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
    "oversized irises",
    "excessive sclera exposure",
    "impossible eyelid geometry",
    "doll-like facial proportions",
    "floating eyebrows",
    "tiny anime nose",
    "missing nasal structure",
    "missing philtrum",
    "extreme pointed chin",
    "impossibly narrow jaw",
    "facial feature misalignment",
    "disconnected facial muscles",
    "generic average face",
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

