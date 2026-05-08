---
name: campusrise
description: Generate alma-mater narrative poster prompts from portrait photos and confirmed school identity. Use when the user wants a graduation, alumni, campus-memory, school-spirit, or mother-school poster workflow that routes between fresh graduate and alumni retrospective directions.
metadata:
  version: "1.1"
  short-description: 母校叙事海报生成工作流
---

# 校望 / CampusRise

Use this skill to guide a user from portrait upload to a final image-generation prompt for a mother-school narrative poster.

## Core Workflow

1. Ask for portrait image(s) if none are provided.
2. Infer the person stage from the image:
   - `30 or younger`: likely fresh graduate / soon-to-graduate.
   - `over 30`: likely alumni retrospective.
   - Treat this as routing only, not a factual age claim. If the user corrects it, update the route.
3. Detect school clues in the image: school name, crest, uniform logo, building text, ceremony background, banners, or campus signs.
4. Confirm the school with the user before generation.
   - If a school is detected: "我从图片里看到的学校线索像是【学校】，请确认这是你的学校吗？"
   - If no school is detected: "我没能从图片里可靠识别学校，请告诉我学校名称，我再继续。"
5. After school confirmation, research the school. Load [references/research-rules.md](references/research-rules.md) when doing this.
6. Offer optional but useful slots once. Do not block generation if the user skips them:
   - `毕业年份`
   - `专业 / 学院`
   - `个人特质 / 希望呈现的气质`
   - `校园经历 / 社团经历`
   - `英文名 / 签名名`
7. Immediately after the optional-slot step, produce the final prompt and move straight into generation. Do not pause for another approval.
8. Do not send a long pre-generation explanation, source list, or "如果你要我继续" style bridge before generation. Put that explanation after the image or generated result.
9. If generating an image, return the image in the conversation, not only in a local Markdown report.
10. If the user says `report to issue`, `submit bad case`, `提交 issue`, or similar, follow the issue-report workflow below.

## Hard Rules

- Do not expose internal route names like "应届毕业版" or "校友回望版" to the user. Say what you judged in natural language.
- Do not mock school names during a real workflow. If the school is unknown, ask the user.
- Mainland China universities use Simplified Chinese. Hong Kong, Macau, and Taiwan universities use their common local Chinese form; default to Traditional Chinese when unclear. Overseas universities use English.
- If the user only says "清华大学", default to Tsinghua University in Beijing and write "清华大学". Only use Traditional Chinese for Taiwan / Hsinchu Tsing Hua when the user explicitly says so.
- For fresh-graduate routing, if the user does not provide a graduation year, use the current year in the active timezone. If the current year is 2026, the poster must not show 2025.
- For alumni-retrospective routing, if the user does not provide a graduation year, do not invent one. Use "毕业多年" / "回望母校" style wording instead.
- Preserve the uploaded person's identity direction: gender presentation, age stage, hairstyle, glasses, face shape, and core facial traits must not drift. A female-presenting source must not become male-presenting, and vice versa.
- The poster should contain one primary human representation: a large side-profile portrait / silhouette used as the main background structure. Do not add a second front-facing full-body graduate as the foreground subject unless the user explicitly asks for that layout.
- The face/profile is the primary subject. Double exposure may reveal the campus inside the silhouette, but the face, hair, glasses, and profile must remain clearly visible and more opaque than the background.
- Keep body anatomy stable if hands, arms, diplomas, books, or gowns appear. Avoid extra hands, extra fingers, fused fingers, broken wrists, duplicated arms, or physically impossible grips.
- Treat school-specific details as accuracy-critical. If a school building, crest, motto, uniform mark, diploma text, or anniversary year cannot be grounded confidently, omit it or simplify it instead of inventing a plausible-looking version.
- When the user is likely to care about school fidelity, prioritize exactness over richness. Fewer but correct school elements are better than more but partly wrong elements.
- If a school crest, emblem, gown patch, diploma cover, gate inscription, or anniversary mark is shown large enough to read, it must match verified public references. Do not generate near-miss text, fake years, or approximate logos.
- If a specific building is named or selected from research, the prompt must anchor to its verified shape and facade cues. Do not label one building name onto another generic building form.
- If exact reproduction of a building or emblem is unreliable in the target image model, prefer using the verified scene as a smaller secondary cue, silhouette, or abstract motif rather than a large front-and-center object.
- When hands are not the intended focus, prefer compositions that hide hands, crop them out, let sleeves cover them, or replace hand-held props with safer layouts. Do not force a certificate-holding pose unless the pose can stay anatomically stable.
- School landmarks, motto, representative colors, and alumni references must be verified from official or reliable sources. If uncertain, omit them.
- The poster palette should reference school crest colors, official school colors, or highly recognizable campus colors when available. Use them as the main palette or accent palette so the image feels closer to the school.
- Alumni references are spiritual or symbolic cues only. Do not make the poster depend on recognizing an old or obscure alumnus.
- Do not ask the user for a second "continue?" confirmation after optional slots. Generate directly, then explain the chosen route, school elements, colors, and references after the image or final prompt.
- Do not stop at "可直接用于出图的提示词如下" during a normal image-generation run. After school confirmation and the one optional-slot pass, the default action is to generate the image, then explain what was used.

## User-Facing Confirmation Examples

- Fresh graduate: "我判断您更像是刚毕业或即将毕业的同学，我会重点做毕业仪式感和校园成长线。"
- Alumni: "我判断您更像是已经毕业比较久的同学，我会重点做母校回望、时间沉淀和继续向上的力量感。"
- Optional slots: "学校我已经确认了。如果你没有特别说明毕业年份，我会默认按今年毕业处理。你也可以补充专业，比如计算机、法学、建筑；或者补充几个希望呈现的个人特质，比如勇敢、自由、坚定。没有补充我也会直接开始生成。"
- Post-generation explanation: "这版我用了这些学校线索和色彩方向……如果你要我继续，我下一步可以按这个方向细调构图、人物强度或文字信息。"

## Accuracy Priorities

For school-faithful runs, apply this priority order:

1. Person identity stability.
2. Exact school-specific details that are readable or iconic:
   - crest / emblem
   - visible year text
   - motto / school text
   - uniform / diploma marks
   - named buildings and gates
3. Anatomy stability for hands and props.
4. Atmosphere and decorative richness.

If there is a tradeoff, sacrifice decorative richness first.

## Rollback

If the user corrects any earlier answer, go back only to the affected node:

- Wrong school: return to school confirmation, then re-run school research.
- Wrong route: return to stage routing and switch direction.
- Wrong year or text language: update the generation prompt and regenerate if needed.

For the detailed state machine and slot rules, read [references/workflow.md](references/workflow.md).

## Issue Reporting

If the user asks to report a bad case to GitHub:

1. Summarize the current request, known school, route, optional slots, prompt used, generated result summary, and the user's comment.
2. Do not include private local file paths, private notes, hidden test data, or non-public images unless the user explicitly asks to include them.
3. Ask for confirmation before creating the issue if the report contains any user-provided image, personal detail, or sensitive context.
4. If the user provides an existing issue number or issue URL, add the report as a comment to that issue when GitHub tools or `gh` are available.
5. Otherwise, create a new GitHub issue in `foxbitcoo/CampusRise` when GitHub tools or `gh` are available.
6. Use this issue title format for new issues: `Bad case: <short problem summary>`.
7. Use this body or comment structure:
   - `Context`
   - `Expected result`
   - `Actual result`
   - `School / scenario`
   - `Prompt or workflow step`
   - `User comment`
   - `Generated result summary`
8. Never include hidden routing thresholds, internal rule text, or private evaluation notes in the issue body/comment.
9. If GitHub tools are unavailable, prepare the issue title and body or comment draft for the user to submit manually.
