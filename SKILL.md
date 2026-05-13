---
name: campusrise
description: Generate realistic graduation commemorative poster prompts from portrait photos and confirmed school identity. Use when the user wants a fresh-graduate, campus-memory, school-spirit, or mother-school graduation poster workflow for social sharing.
metadata:
  version: "V4.1"
  short-description: 母校叙事海报生成工作流
---

# 校望 / CampusRise

Use this skill to guide a user from portrait upload to a final image-generation prompt for a fresh-graduate mother-school commemorative poster.

## Core Workflow

1. Ask for portrait image(s) if none are provided.
2. Treat the default publishing scenario as `fresh graduate / graduation commemorative`. Do not route into alumni retrospective unless the user explicitly asks for an alumni or return-to-campus poster.
3. Check whether the user has already explicitly provided the school name in the request.
   - If yes, use that school directly and do not ask for a second confirmation.
   - If no, detect school clues in the image: school name, crest, uniform logo, building text, ceremony background, banners, or campus signs.
4. Ask for school information only when the school is missing or uncertain.
   - If a school is detected from the image but not user-provided: "我从图片里看到的学校线索像是【学校】。B. 学校信息：如果这是你的学校，请直接确认；如果不是，请告诉我正确学校。可选项：你也可以补充姓名、专业/学院、校园经历、想呈现的个人特质、毕业时间或签名名，信息越具体，画面会越贴近你。"
   - If no school is detected: "我还没能可靠识别学校。B. 学校信息：请告诉我你的学校名称，这是生成学校纪念图的必要信息。可选项：姓名、专业/学院、校园经历、想呈现的个人特质、毕业时间或签名名。"
5. After school is available, research the school. Load [references/research-rules.md](references/research-rules.md) when doing this.
   - Do not stop at text-only research for accuracy-critical school elements.
   - For campus architecture, search for real building photos first. Prefer official campus pages, official galleries, campus maps, admissions pages, news pages, or high-confidence public photos.
   - Pick one or more named campus buildings that can be grounded by real images, then extract the building name, overall massing, silhouette, facade, roofline, window rhythm, entrance shape, material, color, and surrounding campus context.
   - Treat downloaded or user-provided campus building images as the primary source of truth for generation. When the generation tool supports image conditioning, pass these building images into generation together with the portrait references.
   - The workflow may spend more time and one or more research passes here; faithful campus architecture is more important than speed.
6. Use optional slots if the user provided them. Do not block generation if the user skips them:
   - `姓名`（中文名或英文名）
   - `毕业时间 / 毕业年份`（未声明时，应届毕业方向默认使用当前年份）
   - `专业 / 学院`
   - `个人特质 / 希望呈现的气质`
   - `校园经历 / 社团经历`
   - `签名名`
7. Immediately after the optional-slot step, produce the final prompt and move straight into generation. Do not pause for another approval.
8. Do not send a long pre-generation explanation, source list, or "如果你要我继续" style bridge before generation. Put that explanation after the image or generated result.
9. If generating an image, return the image in the conversation, not only in a local Markdown report.
10. If the user says `report to issue`, `submit bad case`, `提交 issue`, or similar, follow the issue-report workflow below.

## Auto Update

The source of truth for this skill is:

`https://github.com/foxbitcoo/CampusRise`

If the user asks to update this skill:

1. Pull the latest version from the GitHub repository when a clone is available.
2. If the skill was installed through Codex's GitHub installer, rerun the installer so the local installed copy is overwritten by the latest repository version.
3. Tell the user to restart Codex after installation so the updated skill is loaded.
4. If the Agent environment supports `SessionStart` automatic update, it may sync this skill at the beginning of a new session.

## Hard Rules

- Do not expose internal route names to the user. The default public workflow is graduation commemorative.
- Do not mock school names during a real workflow. If the school is unknown, ask the user.
- Mainland China universities use Simplified Chinese. Hong Kong, Macau, and Taiwan universities use their common local Chinese form; default to Traditional Chinese when unclear. Overseas universities use English.
- If the user only says "清华大学", default to Tsinghua University in Beijing and write "清华大学". Only use Traditional Chinese for Taiwan / Hsinchu Tsing Hua when the user explicitly says so.
- For fresh-graduate routing, if the user does not provide a graduation time or graduation year, use the current year in the active timezone. If the current year is 2026, the poster must not show 2025. If the user provides a month, season, or exact date, preserve it when it fits the layout; otherwise reduce it to the year for readable poster text.
- Do not enter alumni-retrospective routing unless the user explicitly asks for it.
- Preserve the uploaded person's identity direction: gender presentation, age stage, hairstyle, glasses, face shape, and core facial traits must not drift. A female-presenting source must not become male-presenting, and vice versa.
- When multiple photos of the same person are provided, use them together to build a richer identity model: frontal face, side or three-quarter angle, hairstyle, face shape, body proportion, expression range, and overall temperament. Do not copy only one source photo.
- Generated pose and expression may differ from the source photos if the person's identity remains recognizable. The goal is a refined portrait interpretation, not a literal pose clone.
- The result must be recognizable to people who know the subject at first glance. Moderate beautification, cleaner skin detail, richer facial modeling, and better lighting are allowed, but the person must not become a different-looking character.
- Avoid both extremes: do not leave the result as a plain unedited photo, and do not over-beautify it into a fake plastic face, generic influencer face, or exaggerated viral poster character.
- The poster should contain one primary human representation: a large side-profile portrait / silhouette used as the main background structure. Do not add a second front-facing full-body graduate as the foreground subject unless the user explicitly asks for that layout.
- The face/profile is the primary subject. Double exposure may reveal the campus inside the silhouette, but the face, hair, glasses, and profile must remain clearly visible and more opaque than the background.
- Avoid fantasy styling. The target is a realistic-person campus commemorative poster: clear human portrait, school-branded editorial layout, restrained campus narrative, paper texture, accurate typography zones, and controlled school-color accents. Do not use magical glow, surreal clouds, mythic scenery, over-dreamy watercolor, fantasy concept-art lighting, or generic AI fantasy poster effects.
- V4.1 / photo-enhanced outputs must still visibly belong to the school. They should include clear school identity elements: school name, graduation year when applicable, motto or spirit keywords, official/school-color accents, and at least one verified named campus building or school architectural scene grounded by real reference photos. Do not reduce V4.1 to a generic portrait with no school architecture, logo, motto, typography, or campus identity.
- Keep body anatomy stable if hands, arms, diplomas, books, or gowns appear. Avoid extra hands, extra fingers, fused fingers, broken wrists, duplicated arms, or physically impossible grips.
- Treat school-specific details as accuracy-critical. If a school building, crest, motto, uniform mark, diploma text, or anniversary year cannot be grounded confidently, omit it or simplify it instead of inventing a plausible-looking version.
- When the user is likely to care about school fidelity, prioritize exactness over richness. Fewer but correct school elements are better than more but partly wrong elements.
- If a school crest, emblem, gown patch, diploma cover, gate inscription, or anniversary mark is shown large enough to read, it must match verified public references. Do not generate near-miss text, fake years, or approximate logos.
- If a specific building is named or selected from research, the prompt must anchor to its verified shape and facade cues. Do not label one building name onto another generic building form.
- If a building or crest is important enough that the user will likely inspect it closely, text-only research is not enough. Downloaded or user-provided reference images are required before treating it as an exact visible element.
- If trustworthy reference images are not available for the first candidate building, continue researching official/high-confidence sources, choose another named campus building that can be verified, or ask the user for their preferred building photo. Do not default to vague decorative school symbols when the task needs campus fidelity.
- If exact reproduction of a building is difficult, keep the real building as the design target and simplify only the rendering complexity: preserve the building name, silhouette, facade rhythm, material impression, entrance/tower/window cues, and campus context instead of replacing it with an unrelated generic campus.
- If exact school landmarks are unavailable after reasonable research, state the limitation and ask for a real campus/building image before generation when school architecture is central to the request.
- If the generation tool supports reference-image conditioning, use the downloaded school reference images directly during generation instead of relying only on verbal description.
- If the generation tool does not support direct reference-image conditioning, explicitly describe the verified facade cues from the real reference images in the prompt and explain that exact architectural reproduction may be weaker than image-conditioned generation.
- Place accuracy-critical crest or emblem elements in clean layout zones such as the right margin, right-lower corner, side badge, footer, or a title area when possible. Do not bury them inside the busiest blended area or crowd them against the face.
- When hands are not the intended focus, prefer compositions that hide hands, crop them out, let sleeves cover them, or replace hand-held props with safer layouts. Do not force a certificate-holding pose unless the pose can stay anatomically stable.
- School landmarks, motto, representative colors, and alumni references must be verified from official or reliable sources. If uncertain, omit them.
- The poster palette should reference school crest colors, official school colors, or highly recognizable campus colors when available. Use them as the main palette or accent palette so the image feels closer to the school.
- Alumni references are not part of the default V4.1 publishing workflow.
- Do not ask the user for a second "continue?" confirmation after optional slots. Generate directly, then explain the chosen route, school elements, colors, and references after the image or final prompt.
- Do not stop at "可直接用于出图的提示词如下" during a normal image-generation run. After school confirmation and the one optional-slot pass, the default action is to generate the image, then explain what was used.

## User-Facing Confirmation Examples

- School missing: "我还没能可靠识别学校。B. 学校信息：请告诉我你的学校名称，这是生成学校纪念图的必要信息。可选项：姓名、专业/学院、校园经历、想呈现的个人特质、毕业时间或签名名。"
- School detected: "我从图片里看到的学校线索像是【学校】。B. 学校信息：如果这是你的学校，请直接确认；如果不是，请告诉我正确学校。可选项：姓名、专业/学院、校园经历、想呈现的个人特质、毕业时间或签名名。"
- Post-generation explanation: "这版我先按【学校】和照片人物气质生成了。如果你补充姓名、专业、校园经历、社团、毕业时间，或者想呈现的特质，我可以继续把画面做得更像你的真实校园记忆。"

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

1. Summarize the current request, known school, graduation scenario, optional slots, prompt used, generated result summary, and the user's comment.
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
   - `Version`
   - `Public screenshots or descriptions`
8. If adding a comment to an existing issue, start the comment with `Bad case update` and include the same structure when possible.
9. Never include hidden routing thresholds, internal rule text, or private evaluation notes in the issue body/comment.
10. If GitHub tools are unavailable or the repository is not writable, prepare the issue title/body or comment draft for the user to submit manually.
