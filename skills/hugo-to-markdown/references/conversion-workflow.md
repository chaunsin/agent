# Conversion Workflow

## Purpose

Use this workflow when converting a Hugo documentation site into standard Markdown that no longer depends on Hugo runtime features.

## Step 1: Locate the real rule sources

Read these in order:

1. `hugo.toml`, `hugo.yaml`, `hugo.yml`, `hugo.json`, or `config/*`
2. `archetypes/*`
3. `layouts/_shortcodes/*`
4. `layouts/_markup/*`
5. `content/*`

For the Hugo docs fixture in this repository, also read:

1. `testdata/hugo/docs/hugo.toml`
2. `testdata/hugo/docs/archetypes/*.md`
3. `testdata/hugo/docs/layouts/_shortcodes/*.html`
4. `testdata/hugo/docs/layouts/_markup/*.html`
5. `testdata/hugo/docs/content/en/**/*.md`

## Step 2: Build a site inventory

Run:

```bash
python3 skills/hugo-to-markdown/scripts/inventory_hugo_rules.py \
  --site-root testdata/hugo/docs
```

Inspect the output for:

- content root and module mounts
- active shortcode names
- render hook names
- frequently used shortcodes
- front matter keys

Use the inventory to batch files by complexity:

- plain Markdown only
- front matter only
- built-in shortcode usage
- custom shortcode usage
- render-hook-sensitive links and assets

## Step 3: Convert one slice at a time

Preferred order:

1. plain pages
2. pages with only front matter normalization
3. pages using shared includes
4. pages using custom shortcodes
5. pages whose content is partially generated from sections or data files

This keeps regressions local and makes validation easier.

## Step 4: Materialize dynamic content

If a shortcode generates prose, lists, tables, or badges, replace it with the resulting Markdown.

Examples from the Hugo docs site:

- `include` pulls another content file and renders its shortcodes
- `quick-reference` expands section content
- `render-list-of-pages-in-section` builds a list from a section
- `render-table-of-pages-in-section` builds a table from section pages
- `glossary` materializes glossary content

Do not keep these as live Hugo shortcodes in the final standard Markdown.

## Step 5: Keep literal Hugo examples literal

The docs site frequently documents Hugo syntax itself. Distinguish:

- live shortcode calls that affect rendering
- escaped shortcode examples intended for readers

Common literal-example pattern:

```text
{{</* shortcode arg=value */>}}
```

When the construct is inside a fenced code block or otherwise clearly documentation, preserve it literally.

## Step 6: Validate aggressively

After each batch:

```bash
python3 skills/hugo-to-markdown/scripts/check_standard_markdown.py \
  --root /path/to/output
```

Treat validator hits as unresolved work unless they are deliberate examples inside code fences.
