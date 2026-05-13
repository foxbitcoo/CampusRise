# CampusRise Workflow Reference

## State

- `theme_mode`: `fresh_graduate` by default; `alumni_retrospective` only when explicitly requested by the user
- `current_step`: `age_inference`, `school_detection`, `school_confirmation`, `school_research`, `slot_followup`, `generation`, `post_generation_explanation`
- `school_detection_status`: `detected_pending_confirmation`, `not_detected_pending_user_input`, or `confirmed`
- `school_research`: named campus buildings, building photo references, facade cues, landmarks, motto, source notes, cached reference images

## Step 1: Portrait and Publishing Scenario

The V4.1 public workflow is optimized for Xiaohongshu-style fresh-graduate commemorative posters.

- Treat the default route as fresh graduate / graduation commemorative.
- Do not route to alumni retrospective just because the person looks older.
- Use alumni retrospective only when the user explicitly asks for alumni, return-to-campus, or long-after-graduation memory.

Use cautious wording. This is workflow routing, not an identity claim. If the user says "我是博士应届", "我已经毕业很多年", or similar, accept the correction.

## Step 2: School Input and Detection

First check whether the user has explicitly provided the school name in the request.

- If the user clearly names the school, accept it and continue. Do not ask for a second confirmation.
- If no school is provided, inspect the image for school clues.

Look for:

- School name text
- Crest or logo
- Uniform / gown text
- Building signs
- Graduation backdrop
- Banners, walls, plaques, campus landmarks

Only ask for school confirmation when the school was inferred from the image or is uncertain.

## Step 3: School Confirmation

If the user confirms, proceed to school research.

If the user corrects the school, use the corrected school and research that one.

If no school can be detected, ask the user for the school name and stop until they answer.

When asking, separate required school information from optional enhancement fields:

- `B. 学校信息`: required school name, or confirmation/correction of the detected school.
- Optional fields: name, major/college, campus experience, desired traits, graduation time or year, signature name.

## Step 4: Research

After the school is available, retrieve:

- Representative named campus buildings: gates, halls, libraries, towers, academic buildings, media centers, campus axes, sculptures, and other buildings students recognize.
- Motto or school spirit.
- Representative colors: school crest colors, official school colors, or highly recognizable campus colors.
- Accuracy-critical explicit details that may be rendered large or readable: crest, year marks, diploma/gown wording, gate inscriptions, and named-building facade cues.
- Downloaded or user-provided reference images for any named campus building or school-specific element that may be shown clearly.

Prefer official school sites. If sources conflict or are weak, continue searching before selecting the element.
For V4.1, spending extra time on school fidelity is expected. Use web search and official or high-confidence image references before making buildings, crests, gates, inscriptions, or key symbols visible.

For each exact school element candidate:

- Record the exact building or element name.
- Cache or use real reference images whenever the environment allows downloading or the user provides them.
- Extract concrete visual cues: overall massing, silhouette, roofline, facade proportion, window rhythm, entrance shape, material/color, surrounding plaza/stairs/greenery/roads.
- Prefer a different verified campus building over a generic school-like building if the first candidate lacks usable images.

If no usable real building image can be obtained after reasonable search:

- Ask the user to provide the preferred campus building photo when architecture fidelity is central to the request.
- If generation must proceed without it, explicitly state that the building fidelity is weak; do not claim exact restoration.
- Do not use a random iconic city landmark or generic campus as a substitute for the named school building.

## Step 5: Optional Slots

Use optional information when the user provides it. If asking for a missing school, mention these optional fields in the same message:

- `姓名`: Chinese or English name.
- `毕业时间 / 毕业年份`: accepts a year, month, season, or exact graduation date. Fresh-graduate default is the active timezone's current year; alumni default is no explicit year.
- `专业 / 学院`: examples include 计算机、法学、建筑、医学、新闻传播、金融、公共管理、艺术设计、自动化.
- `个人特质 / 希望呈现的气质`: examples include 勇敢、自由、坚定、温柔、理性、热烈、独立、探索、创造、笃定、开阔.
- `校园经历 / 社团经历`: examples include 学生会、辩论队、合唱团、实验室项目、支教、创业比赛、毕业晚会、图书馆自习、操场夜跑.
- `签名名`: only add a signature if provided.

Ask once, then continue. If the user does not provide optional slots, generate directly with available information. Do not ask "是否继续生成" or wait for a second confirmation.
If the user does provide optional slots, the very next model action should still be generation, not a long prompt preview or pre-generation explanation.

## Step 6: Year and Language

Year:

- Fresh-graduate route: if no graduation time or year is provided, use the active timezone's current year.
- If the user provides a month, season, or exact graduation date, preserve it when it fits the design; otherwise reduce it to the year for readable poster text.
- Alumni route: if no year is provided, do not invent a year.
- If the poster includes a year, all visible year text must match the chosen year.

Language:

- Mainland China universities: Simplified Chinese.
- Hong Kong / Macau / Taiwan universities: common local Chinese form; default Traditional Chinese when unclear.
- Overseas universities: English.
- "清华大学" defaults to the Beijing university unless the user explicitly says Taiwan / Hsinchu.

## Step 7: Generate Directly

Do not stop for final confirmation after optional slots. Build the prompt and generate directly.
Do not insert a separate assistant turn that only says "可直接用于出图的提示词如下", lists school sources, or asks the user whether to continue.
When composing the prompt, separate school elements into:

- `exact elements`: readable crest, year text, school motto/text, diploma/gown identifiers, named landmark buildings
- `atmosphere elements`: color palette, season, light, books, silhouettes, campus mood

Exact elements must be grounded by real references. Atmosphere elements can stay expressive, but they cannot replace required campus architecture.
If cached school reference images exist and the generation workflow supports using them, feed those images into generation as hard references for buildings and exact elements.
If the workflow cannot pass those images into generation directly, convert the reference image into a dense verbal architectural brief and include the building name plus facade cues in the prompt.
For architecture, preserve the real building's name, shape, facade rhythm, material impression, and surrounding campus relationship as much as the model allows.
For crests or seals, use clean placement zones such as right side, right-lower corner, footer, or a title-adjacent badge; do not let them crowd the face.

Use the prompt templates in `references/prompt-templates.md`.

If generating preview images, return them inline in the conversation. Do not only write local Markdown results.

## Step 8: Post-Generation Explanation

After generation, explain what was used:

- Route in user-facing language, not internal route names.
- Confirmed school.
- School landmarks and symbols.
- School motto / spirit keywords.
- School colors or crest colors used in the palette.
- Alumni references if used, only as positive symbolic cues.
- Any source notes or school-clue recap should appear here, not before image generation.
- Which real campus building photos were used; the building names; and which architectural details were carried into the generated prompt/result.

## Rollback Signals

Examples:

- "我不是毕业生"
- "不对，我不是这个学校"
- "学校名错了"
- "我其实是博士应届"
- "改成校友方向"
- "返回上一步"
- "刚才那个选择不对"

Rollback only to the affected node, then continue.

## Bad Case Guardrails

When generating the final image prompt, enforce these layout, identity, and anatomy constraints:

- Preserve the uploaded person's gender presentation and core identity direction. Do not turn a female-presenting source into a male-presenting portrait, or vice versa.
- The main composition is a large side-profile portrait / silhouette used as the background structure.
- Do not add a second front-facing full-body human as the foreground subject unless the user explicitly asks for that layout.
- If graduation ceremony details are needed, use small silhouettes, distant people, or symbolic objects instead of a second primary person.
- If hands, arms, diplomas, books, or gowns appear, keep anatomy stable: no extra hands, extra fingers, fused fingers, broken wrists, duplicated arms, or physically impossible grips.
- If a named building cannot be researched from real images, search for another verifiable campus building or ask the user for a building photo before claiming exact architecture.
- If the school setting can still be grounded geographically, use location-faithful environmental cues only as support, not as a replacement for real campus architecture.
- If a crest, seal, or year mark is likely to be wrong at readable size, verify it with a reference image or keep it secondary; do not invent a near-miss.
- If a crest is retained, place it in a clean layout zone rather than close to the face or inside a dense blended area.
- If a certificate-holding pose causes unstable hands, switch to a safer composition: sleeves covering hands, one hidden hand, cropped hands, certificate tucked under arm, or no certificate at all.
- If an exact campus building has no cached or user-provided visual reference, do not present it as faithfully restored.
