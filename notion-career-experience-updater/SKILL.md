---
name: notion-career-experience-updater
description: Update the user's Notion career workspace via MCP for STAR-based experience management. Use when the user provides a new experience and wants immediate updates to the existing PARA setup (Resources STAR DB and Area > Career hub), including creating STAR entries, monthly intake entries, and related interview/self-intro artifacts.
---

# Notion Career Experience Updater

Use this skill to quickly ingest user-provided experiences into the existing Notion career system.

## Fixed workspace targets

Use these IDs by default.

- STAR data source: `collection://cf3fa183-87fc-4105-9aad-5a21c7fc5814`
- STAR DB page: `https://www.notion.so/abe86ddc82fb4d24acfcfbd238bd9955`
- Career hub page: `312ccbd9-b928-813e-a08e-fc72e9bf0c28`
- Monthly intake data source: `collection://548ec5cb-80bf-452a-9a73-06c784cc61a3`
- Interview questions data source: `collection://91d894d4-6ab9-47db-b8ec-1a19db7e3180`
- Self-intro answers data source: `collection://69532ffb-9f96-4ac2-918d-f44a28329bee`

If any target fails, run `notion-search` with the expected Korean title and recover the new ID.

## Minimum intake from user

Collect this in one pass when missing:

1. Experience title
2. Rough date/month
3. Situation/Task/Action/Result notes (bullet fragments are fine)
4. Experience type: `기술` / `협업/인성` / `혼합`
5. Whether to mark `이력서 사용` and `포트폴리오 사용`

## STAR insertion workflow

1. Normalize user notes into concise STAR fields.
2. Infer `분야` and `직무역량` from content.
3. Create a page in STAR data source using `notion-create-pages`.
4. Immediately write STAR body content into the created page using `notion-update-page`.

Property mapping rules:

- `분야` and `직무역량` are multi-select properties. Pass them as JSON strings.
  - Example: `"분야": "[\"Android\",\"문제해결\"]"`
  - Example: `"직무역량": "[\"문제해결\",\"오너십\"]"`
- Date format:
  - `"date:시기:start": "YYYY-MM-DD"`
  - `"date:시기:is_datetime": 0`
- Checkboxes:
  - `"이력서 사용": "__YES__"` or `"__NO__"`
  - `"포트폴리오 사용": "__YES__"` or `"__NO__"`

Body template rules:

- Always populate page body with this order:
  - `## 한줄 요약`
  - `## 정량 성과`
  - `## Situation`
  - `## Task`
  - `## Action`
  - `## Result`
- If the page is blank, use `replace_content`.
- If the page already has content, use `replace_content` unless user asks to preserve old text.

## Backfill workflow for existing rows

When user asks to improve readability for existing STAR records:

1. Find rows from STAR DB with `notion-search` using `data_source_url`.
2. Fetch each row page.
3. Build body content from properties.
4. Update each page with STAR body template.

## Optional linked updates

Run only when the user asks.

1. Monthly intake sync
- Add a short record to `월간 경험 수집 DB` and set `STAR 이관` to `__YES__`.

2. Interview question generation
- Add 2-3 probable interview questions for the new experience in `면접 질문 아카이브`.

3. Self-intro answer draft
- Add one mapped prompt/answer draft row in `자소서 문항 답변 DB` with STAR link.

## Output format

After updates, always return:

1. What was created/updated
2. Direct Notion URLs
3. Any assumptions made (especially inferred metrics or roles)
4. Next optional actions as a numbered list

## Guardrails

- Never delete existing content unless explicitly requested.
- Prefer appending new pages over replacing full page content, except STAR entry pages where standardized body replacement is required.
- If STAR data is weak, ask one focused follow-up question, then proceed.
