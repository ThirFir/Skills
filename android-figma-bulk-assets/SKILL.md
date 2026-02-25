---
name: android-figma-bulk-assets
description: "Export Figma assets in bulk and import them into Android resources. Use when converting many Figma icons/assets at once, preferring VectorDrawable XML for simple SVG assets and automatically falling back to 4x PNG for unsupported or complex SVG features."
---

# Android Figma Bulk Assets

Convert Figma design assets into Android-ready resources in one pipeline.

## Mandatory Precheck

1. Check whether Figma MCP is connected and accessible in the current session.
2. If MCP is not connected, stop immediately.
3. Output this exact message and do not continue:
`Figma MCP is not connected. Connect MCP first, then retry this workflow.`

Do not attempt fallback scraping, manual guessing, or partial export without MCP when this skill is invoked for MCP-based bulk processing.

## Workflow

1. Read the Figma selection/file through MCP and collect export targets.
2. Export all targets as SVG first.
3. Validate each SVG for Android VectorDrawable compatibility.
4. Convert compatible SVG assets to `res/drawable/*.xml` VectorDrawable.
5. Mark incompatible SVG assets and export them as `4x` PNG.
6. Save PNG fallbacks under density buckets and keep a conversion report.

## Classification Rules

Treat an SVG as incompatible for VectorDrawable when it contains one or more of:

- `filter` (blur, drop shadow, color matrix).
- `mask`.
- complex `clipPath` behavior that fails conversion.
- unsupported blend/effect behavior.
- excessive path complexity likely to break import.

If incompatible, skip vector conversion and fallback to PNG.

## Output Rules

- Vector output: `app/src/main/res/drawable/<name>.xml`.
- PNG fallback output:
- `app/src/main/res/drawable-mdpi/<name>.png`
- `app/src/main/res/drawable-hdpi/<name>.png`
- `app/src/main/res/drawable-xhdpi/<name>.png`
- `app/src/main/res/drawable-xxhdpi/<name>.png`
- `app/src/main/res/drawable-xxxhdpi/<name>.png`

Use `xxxhdpi` as the 4x baseline and downscale for lower densities.

## Naming Rules

- Use lowercase snake_case.
- Use `ic_` prefix for icons and `img_` prefix for illustrations/images.
- Keep Android resource names stable and deterministic from Figma node names.

## Quality Checks

1. Verify no duplicate resource names.
2. Verify every source asset produced either VectorDrawable XML or PNG fallback.
3. Verify fallback PNGs have transparent background when required.
4. Verify report includes reason for every fallback decision.

## Guardrails

- Do not silently drop failed assets.
- Do not overwrite unrelated existing resources with different semantics.
- If both vector and png exist for same semantic asset, prefer vector and remove redundant png unless user asked to keep both.
