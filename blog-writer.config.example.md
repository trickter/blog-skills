# Blog Writer Config

Use this file to avoid rediscovering the target blog on every run.

Create your own local copy from this template and fill in the values that are known for your environment.

## Config

```yaml
blog_url: http://127.0.0.1:5000
blog_project_path: /app/ai-code/brainstorm
blog_content_path:
publish_mode: prisma
database_url: file:/app/ai-code/brainstorm/prisma/dev.db
database_provider: sqlite
post_model: Post
post_fields:
  title: title
  slug: slug
  content: content
  published: published
  category_id: categoryId
default_category_slug:
backup_repo: trickter/blog-backup
backup_branch: main
backup_posts_dir: posts
source_name: orBlog
github_token_env: GITHUB_TOKEN
```

## Publish Modes

Supported modes:

- `prisma`: publish by writing to a Prisma-managed database model
- `markdown-files`: publish by creating markdown or mdx files in a content directory
- `http-api`: publish by calling an application API

## Notes

- If `publish_mode` is `prisma`, prefer using the configured `blog_project_path` and project dependencies to write through the app's own data layer.
- If `database_url` is relative in the app config, you may still store an absolute path here to skip runtime path resolution.
- Leave values blank only if you want the skill to fall back to environment discovery.
- For your current blog, `publish_mode: prisma` is the fast path and should avoid repeated port and schema inspection.
