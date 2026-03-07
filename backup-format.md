# Backup Format

Use this file when saving posts into the `blog-backup` repository as a reference backup after local publication.

## Repository

- Remote: `https://github.com/trickter/blog-backup`
- Target directory: `posts/`
- Target branch: `main`

## Publish Order

Use this order:

1. Publish to the local blog project that serves `http://127.0.0.1:5000`
2. Back up the same markdown to `blog-backup`

GitHub is the backup target, not the primary publish target.

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

## GitHub Backup Steps Without Local Clone

Do not clone the repository locally unless the user explicitly asks for that workflow.

Preferred method: GitHub Contents API.

Request shape:

```http
PUT /repos/trickter/blog-backup/contents/posts/YYYY-MM-DD-slug.md
```

Payload should include:

- `message`: `Add post: slug` or `Update post: slug`
- `content`: base64-encoded markdown
- `branch`: `main`
- `sha`: required only when updating an existing file

Authentication:

- Use a GitHub token or another already configured authenticated method
- If no authenticated method is available, stop before backup and report the exact missing prerequisite

## Failure Handling

If backup fails, keep and report:

- The local publish path
- The backup target path
- The exact API or authentication error
