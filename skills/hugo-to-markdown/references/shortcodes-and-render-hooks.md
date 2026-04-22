# Shortcodes And Render Hooks

## Shortcode Notation

Hugo has two shortcode notations:

- `{{< ... >}}`
- `{{% ... %}}`

Use Hugo's rule, documented in the official shortcode pages:

- `%` notation is rendered before Markdown
- `<` notation is rendered after Markdown

For conversion, do not preserve this live syntax in the final standard Markdown unless the document is explicitly teaching Hugo syntax.

## Docs-Site Custom Shortcodes

The Hugo docs fixture defines these custom shortcodes in `layouts/_shortcodes/`:

- `code-toggle`
- `datatable`
- `deprecated-in`
- `eturl`
- `get-page-desc`
- `glossary`
- `glossary-term`
- `hl`
- `img`
- `imgproc`
- `include`
- `module-mounts-note`
- `new-in`
- `newtemplatesystem`
- `per-lang-config-keys`
- `quick-reference`
- `render-list-of-pages-in-section`
- `render-table-of-pages-in-section`
- `root-configuration-keys`
- `syntax-highlighting-styles`

Read the local template file before deciding the replacement.

## High-Impact Local Rules

### Logical content paths

The docs fixture mounts `content/en` to Hugo's logical `content` root. Resolve links and `include` targets against logical paths such as `/configuration/markup` or `/_common/permalink-tokens.md`, not filesystem paths that retain `/en/`.

### `include`

`include` renders another content page through `RenderShortcodes`. This means:

- the included file may contain more shortcodes
- the included file is part of the final visible content
- conversion must recursively resolve the referenced file
- the included file should contribute body content, not duplicate front matter

### `quick-reference`

This shortcode renders sections and child pages dynamically. Replace it with a materialized Markdown structure.

### `render-list-of-pages-in-section`

This shortcode builds lists from a section path. Replace it with normal Markdown lists and descriptions.

### `render-table-of-pages-in-section`

This shortcode builds tables from a section path and filters. Replace it with a standard Markdown table if practical, otherwise a clear list.

### `glossary-term` and `glossary`

These inject glossary links or glossary content. Preserve the resulting prose or links, not the shortcode syntax.

### `new-in` and `deprecated-in`

Convert these into plain Markdown callouts or inline labels such as:

- `New in Hugo 0.144.0.`
- `Deprecated in Hugo 0.144.0.`

### `code-toggle`

Preserve the underlying example content as fenced code, not the UI toggle mechanism.

Local implications from the docs fixture:

- `file=hugo` or similar parameters indicate repository-style config examples
- `fm=true` means the emitted example includes front matter semantics
- `config=` and `dataKey=` style usage can pull data-backed snippets, so read local data files before flattening the shortcode

### `glossary-term` and glossary links

The docs fixture uses glossary shortcuts in two forms:

- `glossary-term` shortcode usage
- Markdown links whose destination is exactly `(g)`

Both should become explicit Markdown links or explicit glossary labels in the output.

## Render Hooks

The local docs site defines render hooks for:

- blockquotes
- code blocks
- links
- passthrough
- tables

Important implications:

- a Markdown link may resolve against pages, page resources, section resources, or global resources
- content may depend on local validation logic for broken links
- rendered HTML may differ from generic CommonMark defaults
- blockquotes can carry alert semantics such as note, tip, important, warning, and caution
- code blocks can carry file labels, copy flags, details wrappers, summaries, trim behavior, and language remapping
- passthrough hooks can make math delimiters meaningful content rather than raw noise

For link-heavy pages, read the local `render-link.html` and the official render-hook docs before rewriting links.
