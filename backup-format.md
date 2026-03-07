# Backup Format

Use this file when saving posts into a configurable GitHub repository as a reference backup after local publication.

## Configuration

Resolve these values before backup:

- `backup_repo`: GitHub repository in `owner/repo` form
- `backup_posts_dir`: target directory inside the repository, default `posts`
- `backup_branch`: target branch, default `main`
- `source_name`: frontmatter source value
- `github_token_env`: env var containing a GitHub token, default `GITHUB_TOKEN`

Read them from the local config file first if present.

Example:

```yaml
backup_repo: trickter/blog-backup
backup_posts_dir: posts
backup_branch: main
source_name: orBlog
github_token_env: GITHUB_TOKEN
```

## Publish Order

Use this order:

1. Publish to the local blog project that serves `http://127.0.0.1:5000`
2. Back up the same markdown to `backup_repo`

GitHub is the backup target, not the primary publish target.

## Filename

Save each post as:

```text
<backup_posts_dir>/YYYY-MM-DD-slug.md
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
- `source`: default to `source_name`

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
PUT /repos/{backup_repo}/contents/{backup_posts_dir}/YYYY-MM-DD-slug.md
```

Payload should include:

- `message`: `Add post: slug` or `Update post: slug`
- `content`: base64-encoded markdown
- `branch`: `backup_branch`
- `sha`: required only when updating an existing file

Authentication:

- Use the token from `github_token_env` or another already configured authenticated method
- If no authenticated method is available, stop before backup and report the exact missing prerequisite

## Failure Handling

If backup fails, keep and report:

- The local publish path
- The backup target path
- The exact API or authentication error
