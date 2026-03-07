# Backup Format

Use this file when saving posts into the `blog-backup` repository.

## Repository

- Remote: `https://github.com/trickter/blog-backup`
- Target directory: `posts/`
- Target branch: `main`

## Filename

Save each post as:

```text
posts/YYYY-MM-DD-slug.md
```

Example:

```text
posts/2026-03-07-frontier-tech-80-percent-noise-three-survivors.md
```

## Frontmatter

Use this structure:

```yaml
---
title: "Final post title"
slug: "final-post-slug"
date: "YYYY-MM-DD"
tags: ["tag1", "tag2"]
status: "published"
source: "orBlog"
---
```

Guidelines:

- `title`: final Chinese title
- `slug`: lowercase kebab-case, stable and readable
- `date`: current local date in `YYYY-MM-DD`
- `tags`: choose a small relevant set
- `status`: default to `published`
- `source`: default to `orBlog`

## Body

After frontmatter, place the final article body in markdown.

- Preserve headings
- Preserve images or source links if they are part of the article
- Do not wrap the whole article in code fences

## Git Backup Steps

If the repository is not present locally:

```bash
git clone https://github.com/trickter/blog-backup.git
```

Then:

```bash
git -C blog-backup add posts/YYYY-MM-DD-slug.md
git -C blog-backup commit -m "Add post: slug"
git -C blog-backup push origin main
```

## Failure Handling

If push fails, keep and report:

- The local file path
- The commit SHA if commit succeeded
- The exact git error
