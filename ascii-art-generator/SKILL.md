---
name: ascii-art-generator
description: Generate ASCII art from user prompts, including text banners, simple icons, boxed layouts, decorative terminal art, and optional multi-color output. Use when the user asks to draw ASCII art, stylize text in monospaced art, or create terminal/README/chat illustrations with plain or colored output.
---

# ASCII Art Generator

Create clean monospaced art for terminal-friendly output.

## Workflow

1. Identify requested art type: text banner, icon, scene, frame, or separator.
2. Confirm constraints: width, height, detail level, and output target.
3. Draft structure in plain ASCII first.
4. Apply style variant (Minimal, Standard, Detailed).
5. If color is requested, apply a color mode (ANSI or HTML).
6. Return the final result in a fenced code block.

## Character Rules

- Default charset: ` `, `.`, `:`, `-`, `=`, `+`, `*`, `#`, `/`, `\\`, `_`, `|`, `(`, `)`.
- Default to ASCII-only unless Unicode is explicitly requested.
- Keep left alignment consistent and avoid trailing spaces.

## Color Modes

- `mono` (default): plain monochrome ASCII.
- `ansi`: colored output using ANSI escape sequences for terminals.
- `html`: colored output using `<span style="color:#RRGGBB">` wrappers.

## Color Guidelines

- Use at most 3-5 colors in one piece for readability.
- Preserve contrast: foreground must remain readable on dark and light backgrounds.
- Do not color every character if it hurts legibility.


## Output Policy

- Return exactly one final art output per request.
- Do not include automatic fallback versions.
- Only output additional variants when the user explicitly asks.
## Compatibility Notes

- ANSI color works in most modern terminals, but not all chat/code viewers.
- HTML color is useful for web/Markdown renderers that allow inline HTML.
- GitHub code fences usually do not render ANSI colors.

## Variants

- Minimal: compact and narrow.
- Standard: balanced detail for chat/CLI.
- Detailed: wider output with richer shading.

## Quality Checklist

- Verify symmetry where intended.
- Verify line lengths are visually coherent.
- Verify output remains readable in monospace.

## Example Requests

- "Draw a cat as ASCII art"
- "Create a LOGIT text banner in ASCII"
- "Make a colorful ANSI rocket ASCII art"
- "Generate HTML-colored ASCII divider for README"
