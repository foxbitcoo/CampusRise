# CampusRise School Research Rules

## Required Research

After the school is confirmed, research:

- Representative visual elements.
- Motto or school spirit.
- Representative colors from the school crest, official school colors, or highly recognizable campus colors.
- Positive, verifiable, well-known alumni references.
- Accuracy-critical explicit details when they may appear in the image: crest shape, visible year marks, diploma wording, gown / uniform identifiers, gate inscriptions, and named building facade cues.
- Downloaded reference images for accuracy-critical school elements whenever the environment allows it.

## Source Priority

1. Official school pages.
2. Official museum / archive / alumni pages.
3. Encyclopedic or authoritative media sources when official pages are insufficient.

If a fact is not verifiable, do not use it.

If a school-specific visual claim is important enough to be visually checked by students, alumni, or prospective users, do not rely on text-only search when a reference image can be obtained. For V4.1 release runs, spending more time on web search and image references is preferred over generating fast but generic school elements.

## Reference Image Collection

For each accuracy-critical school element that may be shown clearly in the final image:

- Download 1-3 official or otherwise high-confidence reference images.
- Prefer official school websites, official galleries, official campus maps, official media kits, official admissions pages, official social accounts, or official campus news pages.
- Cache the downloaded references locally before generation when the environment allows it.
- Keep a short note for each cached image:
  - source URL
  - what the image verifies
  - whether it is safe for exact visible use or only safe as a secondary cue

Recommended priority for downloading:

1. Named landmark buildings.
2. School crest / emblem.
3. Gate or wall inscriptions.
4. Diploma cover / gown identifiers.
5. Campus signs strongly tied to identity.

If reference images cannot be downloaded reliably, explicitly downgrade the affected element.

## What To Extract

Visual elements:

- Gates, lakes, halls, libraries, towers, sculptures, campus axes, old buildings, signature views.
- Prefer elements that make the user immediately feel "this is my school".
- For named buildings, capture 2-4 facade cues that distinguish the real building: overall massing, silhouette, roofline, window rhythm, tower/cylinder presence, material impression, and surrounding layout.
- Prefer to extract these facade cues from downloaded reference images, not just from text summaries.
- If you cannot verify those facade cues well enough, downgrade the building from "explicit named landmark" to a generic campus atmosphere cue and do not print its name in the image explanation as if it were exact.

Motto / spirit:

- Extract 2-5 positive keywords.
- Convert them into visual direction: light, path, water, books, architecture, season, time, horizon, ascent.

Representative colors:

- Prefer official school colors or crest colors when available.
- If official colors are not easy to verify, infer cautiously from the crest or the most recognizable campus palette.
- Use school colors as the main palette or accent palette, balanced with the poster style.
- Do not force saturated brand colors if they damage portrait clarity or poster elegance.
- If color facts are uncertain, describe them as visual inspiration rather than official colors.

Regional fallback:

- If the exact campus landmark cannot be verified well enough for direct visual use, extract the school's real geographic and cultural setting instead.
- Examples of fallback dimensions:
  - coastal / harbor / island / bay atmosphere
  - Lingnan / South China / northern academic / Jiangnan / plateau / overseas urban context
  - local vegetation, light quality, climate, shoreline, hills, skyline distance, paving, arcades, or architectural material tendencies
- The fallback must remain plausibly tied to the school's real location, not just any famous landmark from the broader region.
- Do not insert unrelated regional icons simply because they are famous. A school in Macau does not automatically justify any Macau landmark unless the landmark is relevant and visually coherent with the school.

Crest / text / year marks:

- Any crest or emblem that will be shown at readable size must be checked against official references.
- Any visible year or anniversary mark must match verified school history and the user's chosen graduation year. Do not let the model improvise year numbers.
- If exact crest reproduction is risky, prefer a smaller emblem, embossed texture, or removal over a large incorrect badge.
- If no trustworthy crest reference image is cached, do not show a large readable crest.
- When a crest is used, prefer clean placement in secondary layout zones such as a right-side badge, footer seal, upper corner mark, or title-adjacent insignia, where distortion risk is lower and face readability is preserved.

Uniform / diploma identifiers:

- Only include gown patches, diploma covers, degree text, or official-style marks when they are visible in the user photo or can be verified reliably.
- If the exact wording or seal is uncertain, simplify the prop and avoid readable fake text.
- If no trustworthy reference image is cached for the identifier, avoid making it large enough for close inspection.

Alumni:

- Use only real, recognizable, positive public figures.
- Do not require the viewer to recognize the alumnus.
- Do not paste alumni portraits into the poster by default.
- Convert alumni references into symbolic cues such as science, public service, literature, education, engineering, art, or social responsibility.

## Language Defaults

- Mainland China universities: Simplified Chinese.
- Hong Kong / Macau / Taiwan universities: common local Chinese form; default Traditional Chinese when unclear.
- Overseas universities: English.
- "清华大学" defaults to Beijing Tsinghua and Simplified Chinese.

## Return Note

After generation, briefly tell the user:

- Which school visual elements were used.
- Which motto/spirit keywords were used.
- Which school or crest colors influenced the palette.
- Which alumni references were used, if any, and that they were used only as positive symbolic cues.
- Which school-specific details were kept exact on purpose, and which risky details were intentionally omitted or abstracted.
- Whether downloaded reference images were used for the exact school elements, or whether some elements were downgraded because image references were unavailable.
