# CampusRise Workflow Reference

## State

- `theme_mode`: `fresh_graduate`, `alumni_retrospective`, or `unknown`
- `current_step`: `age_inference`, `school_detection`, `school_confirmation`, `school_research`, `slot_followup`, `generation`, `post_generation_explanation`
- `school_detection_status`: `detected_pending_confirmation`, `not_detected_pending_user_input`, or `confirmed`
- `school_research`: landmarks, motto, positive alumni references, source notes

## Step 1: Portrait and Stage

Infer the approximate life stage from the uploaded portrait(s):

- 30 or younger: treat as fresh graduate / soon-to-graduate by default.
- Over 30: treat as alumni retrospective by default.

Use cautious wording. This is workflow routing, not an identity claim. If the user says "我是博士应届", "我已经毕业很多年", or similar, accept the correction.

## Step 2: School Detection

Look for:

- School name text
- Crest or logo
- Uniform / gown text
- Building signs
- Graduation backdrop
- Banners, walls, plaques, campus landmarks

Always confirm before generation. Do not proceed from detection directly to prompt generation.

## Step 3: School Confirmation

If the user confirms, proceed to school research.

If the user corrects the school, use the corrected school and research that one.

If no school can be detected, ask the user for the school name and stop until they answer.

## Step 4: Research

After confirmation, retrieve:

- Representative visual elements: gates, lakes, halls, libraries, towers, sculptures, campus axes, iconic buildings.
- Motto or school spirit.
- Representative colors: school crest colors, official school colors, or highly recognizable campus colors.
- Positive famous alumni when they are well-known and verifiable.
- Accuracy-critical explicit details that may be rendered large or readable: crest, year marks, diploma/gown wording, gate inscriptions, and named-building facade cues.

Prefer official school sites. If sources conflict or are weak, omit uncertain items.

## Step 5: Optional Slots

Ask for optional information only after school confirmation:

- `毕业年份`: fresh-graduate default is current year; alumni default is no explicit year.
- `专业 / 学院`: examples include 计算机、法学、建筑、医学、新闻传播、金融、公共管理、艺术设计、自动化.
- `个人特质 / 希望呈现的气质`: examples include 勇敢、自由、坚定、温柔、理性、热烈、独立、探索、创造、笃定、开阔.
- `校园经历 / 社团经历`: examples include 学生会、辩论队、合唱团、实验室项目、支教、创业比赛、毕业晚会、图书馆自习、操场夜跑.
- `英文名 / 签名名`: only add a signature if provided.

Ask once, then continue. If the user does not provide optional slots, generate directly with available information. Do not ask "是否继续生成" or wait for a second confirmation.
If the user does provide optional slots, the very next model action should still be generation, not a long prompt preview or pre-generation explanation.

## Step 6: Year and Language

Year:

- Fresh-graduate route: if no year is provided, use the active timezone's current year.
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

Exact elements must be correct or omitted. Atmosphere elements can stay expressive.

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
- If a named building cannot be rendered faithfully, remove the name cue and fall back to verified school colors or a less explicit campus silhouette.
- If a crest, seal, or year mark is likely to be wrong at readable size, reduce its size, convert it to an abstract embossed cue, or omit it.
- If a certificate-holding pose causes unstable hands, switch to a safer composition: sleeves covering hands, one hidden hand, cropped hands, certificate tucked under arm, or no certificate at all.
