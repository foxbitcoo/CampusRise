---
name: campusrise
description: Generate alma-mater narrative poster prompts from portrait photos and confirmed school identity. Use when the user wants a graduation, alumni, campus-memory, school-spirit, or mother-school poster workflow that routes between fresh graduate and alumni retrospective directions.
metadata:
  version: "1.0"
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
6. Offer optional but useful slots. Do not block generation if the user skips them:
   - `毕业年份`
   - `专业 / 学院`
   - `个人特质 / 希望呈现的气质`
   - `校园经历 / 社团经历`
   - `英文名 / 签名名`
7. Produce or use the final prompt template. Load [references/prompt-templates.md](references/prompt-templates.md).
8. If generating an image, return the image in the conversation, not only in a local Markdown report.

## Hard Rules

- Do not expose internal route names like "应届毕业版" or "校友回望版" to the user. Say what you judged in natural language.
- Do not mock school names during a real workflow. If the school is unknown, ask the user.
- Mainland China universities use Simplified Chinese. Hong Kong, Macau, and Taiwan universities use their common local Chinese form; default to Traditional Chinese when unclear. Overseas universities use English.
- If the user only says "清华大学", default to Tsinghua University in Beijing and write "清华大学". Only use Traditional Chinese for Taiwan / Hsinchu Tsing Hua when the user explicitly says so.
- For fresh-graduate routing, if the user does not provide a graduation year, use the current year in the active timezone. If the current year is 2026, the poster must not show 2025.
- For alumni-retrospective routing, if the user does not provide a graduation year, do not invent one. Use "毕业多年" / "回望母校" style wording instead.
- The face/profile is the primary subject. Double exposure may reveal the campus inside the silhouette, but the face, hair, glasses, and profile must remain clearly visible and more opaque than the background.
- School landmarks, motto, and alumni references must be verified from official or reliable sources. If uncertain, omit them.
- Alumni references are spiritual or symbolic cues only. Do not make the poster depend on recognizing an old or obscure alumnus.

## User-Facing Confirmation Examples

- Fresh graduate: "我判断您更像是刚毕业或即将毕业的同学，我会重点做毕业仪式感和校园成长线。"
- Alumni: "我判断您更像是已经毕业比较久的同学，我会重点做母校回望、时间沉淀和继续向上的力量感。"
- Optional slots: "学校我已经确认了。如果你没有特别说明毕业年份，我会默认按今年毕业处理。你也可以补充专业，比如计算机、法学、建筑；或者补充几个希望呈现的个人特质，比如勇敢、自由、坚定。没有也可以继续生成。"

## Rollback

If the user corrects any earlier answer, go back only to the affected node:

- Wrong school: return to school confirmation, then re-run school research.
- Wrong route: return to stage routing and switch direction.
- Wrong year or text language: update the generation prompt and regenerate if needed.

For the detailed state machine and slot rules, read [references/workflow.md](references/workflow.md).
