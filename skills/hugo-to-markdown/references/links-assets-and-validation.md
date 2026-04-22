# Links, Assets, And Validation

## Link Resolution

Treat link resolution as repository-specific behavior.

For the Hugo docs fixture:

- internal links may be plain Markdown destinations
- `ref` and `relref` appear in docs examples and sometimes in live content
- the custom link render hook resolves pages, page resources, section resources, and global resources
- broken-link behavior is controlled by config and local render-hook logic

When converting:

- resolve internal destinations to ordinary Markdown links
- keep remote links as normal external links
- preserve fragments when they still point to stable headings
- avoid copying unresolved Hugo link functions into the output

## Assets

Check all of these before rewriting image or file references:

- page bundle resources
- section resources
- mounted assets
- static files

For this repository's Hugo docs fixture, the link render hook explicitly relies on assets and mounted resources. Read `hugo.toml` and `layouts/_markup/render-link.html` before changing asset paths.

## Validation Checklist

Run:

```bash
python3 skills/hugo-to-markdown/scripts/check_standard_markdown.py \
  --root /path/to/output
```

Review hits for:

- active `{{< ... >}}` or `{{% ... %}}`
- active Go template expressions such as `{{ if ... }}` or `{{ .Page ... }}`
- Hugo-specific link helpers left in prose
- leaked local absolute paths
- executable HTML or script residue that should have been downgraded

## Residue Triage

If the validator reports a construct:

1. Check whether it is inside a fenced code block.
2. If it is a literal example, keep it.
3. If it is active Hugo syntax, resolve or rewrite it.
4. If it cannot be resolved safely, replace it with explicit Markdown text explaining the original behavior.
