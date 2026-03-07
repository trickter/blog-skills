---
name: blog-writer
description: Write Chinese technical blog posts from a topic brief or daily hot trends. Use when you want 5 blog topic ideas first, want to learn style from the local blog at http://127.0.0.1:5000, want stronger titles and openings, want to publish to the local blog project first, or want to back up finished posts to the blog-backup GitHub repository without cloning it locally.
argument-hint: [blog brief or topic]
disable-model-invocation: true
allowed-tools: Read, Bash(curl *), Bash(git *), Bash(ls *), Bash(mkdir *), Bash(find *)
---

# Blog Writer

Write Chinese technical blog posts in the style of the local blog, starting from either daily hot trends or a user-provided topic. Always propose 5 topic options with creation advice before writing the article. After the user picks one option, write the post, publish it into the local blog project behind port `5000`, then back it up to GitHub as a reference copy without cloning the backup repository.

## Quick Start

Input can be empty, a short topic, or a longer brief.

- If input is empty, use Baidu Qianfan plus Baidu hot trends to discover what is worth writing today.
- If input contains a topic or brief, use Baidu Qianfan to research the topic and expand it into better article angles.
- In both cases, propose exactly 5 topic options first.
- Each option must include:
  - `主题：...`
  - `创作建议：...`

Do not write the full article until the user chooses a topic.

## Workflow

### Step 1: Understand the entry mode

Classify the request into one of these modes:

1. `daily-hot-topics`
   - No clear topic was provided
   - You need to discover candidate topics from current hot trends
2. `topic-research`
   - The user already gave a topic or brief
   - You need to research and sharpen it into stronger article directions

If the user has already chosen a topic in the current conversation, skip topic ideation and continue to planning and writing.

### Step 2: Gather external material with Baidu Qianfan

Use Baidu Qianfan as the research layer.

- In `daily-hot-topics` mode:
  - Use Baidu Qianfan to retrieve or summarize current Baidu hot trends
  - Extract topics that are timely, discussable, and suitable for technical readers
  - Prefer topics where a strong argument can be made, not simple news rewrites
- In `topic-research` mode:
  - Use Baidu Qianfan to search for recent information, supporting facts, opposing views, and missing context around the user topic
  - Turn a broad topic into sharper, more opinionated article directions

If Qianfan is unavailable, fall back to direct Baidu hot trends or web-accessible public sources, and state that fallback briefly.

### Step 3: Learn the local house style

Read the local blog at `http://127.0.0.1:5000` before proposing final titles or writing.

Process:

1. Fetch the homepage
2. Extract recent article titles, summaries, and links
3. Open several recent article detail pages
4. Summarize the house style across:
   - Title pattern
   - Opening hook
   - Tone
   - Argument structure
   - Ending style

Treat these style traits as defaults:

- Chinese writing for technical readers
- Strong viewpoint in the title
- Opening gives the conclusion early
- Commentary and judgment matter more than neutral reporting
- Practical takeaways should appear before the ending becomes abstract

Do not imitate specific sentences. Learn the patterns only.

### Step 4: Propose 5 topic options

Always produce exactly 5 options before drafting.

Each option must contain:

```markdown
1. 主题：...
   创作建议：...
```

Selection standard:

- Suitable for technical readers
- Timely or strategically relevant
- Supports a non-obvious point of view
- Can match the local house style
- Has enough material to produce a complete blog post

When generating the 5 options:

- Vary the angle, not just the wording
- Prefer argument-led topics over generic explainers
- Make at least 2 options deliberately sharper and more controversial
- Avoid empty hype or clickbait with no substance

### Step 5: Confirm the selected topic

After presenting the 5 options:

- Ask the user to pick one
- If the user wants, refine the selected option with one more round of angle tightening
- Do not write the full article until a topic is selected

### Step 6: Plan the post

Before drafting, define:

- Core claim
- Intended reader takeaway
- Evidence structure
- Section outline
- Title candidates
- Opening hook strategy

The title and opening must be deliberately strong:

- The title should signal tension, surprise, or a direct judgment
- The opening should hook the reader within the first 2 to 4 sentences
- The opening should quickly answer:
  - Why now
  - What is the real point
  - Why this article is worth reading

### Step 7: Write the article

Write a complete Chinese technical blog post.

Default output structure:

1. Title
2. Opening
3. Main body with section headings
4. Closing section

Required writing behaviors:

- Prioritize original synthesis over paraphrased news
- If using facts or dated claims, anchor them to the research you found
- If a claim cannot be verified, mark it as tentative rather than inventing certainty
- Keep the tone direct, clear, and opinionated
- Avoid filler introductions and generic inspirational language
- Write for smart readers with limited time

For detailed standards, see [writing-guidelines.md](writing-guidelines.md).

### Step 8: Publish to the local blog project

The local blog project is the primary publishing target.

Requirements:

- Find the local blog project that serves `http://127.0.0.1:5000`
- Identify the post storage format used by that project
- Save the final post into that project first
- Prefer the project's existing content path, frontmatter shape, and naming conventions

If the project path is not obvious:

- Inspect the running project and nearby directories
- Infer the content location from existing post files
- If multiple plausible paths exist, choose the one that already stores the currently published posts

Do not treat GitHub backup as the primary publish step. Local publication comes first.

### Step 9: Produce backup-ready markdown

Format the final post as a markdown file for the backup repository.

Requirements:

- File path pattern: `posts/YYYY-MM-DD-slug.md`
- Include frontmatter matching the backup repo conventions
- Use a slug that is readable, stable, and derived from the final title
- Default `source` should be `orBlog`
- Default `status` should be `published` unless the user asks for draft status

For the exact format, see [backup-format.md](backup-format.md).

### Step 10: Back up to GitHub without cloning

Back up the post to `https://github.com/trickter/blog-backup`.

Assumptions:

- The target branch is `main`
- A GitHub token or equivalent authenticated method is available when backup is requested

Workflow:

1. Build the final markdown file content in memory or a temporary local file
2. Determine the target path `posts/YYYY-MM-DD-slug.md`
3. Use the GitHub Contents API to create or update that single file
4. Use a clear commit message such as `Add post: <slug>` or `Update post: <slug>`
5. Report the resulting commit or API response summary

If backup fails:

- Explain the exact failure
- Preserve the markdown content for manual recovery
- Do not silently skip the backup step

## Output Contracts

### Topic Ideation Output

When proposing options, use this shape:

```markdown
1. 主题：...
   创作建议：...

2. 主题：...
   创作建议：...
```

### Draft Output

When writing the article, include:

- Final title
- Optional 2 or 3 alternate title candidates if useful
- Full article body
- Backup filename

### Backup Report

After backup, report:

- Local blog publish path
- Backup target path
- Commit message or API result

## Success Criteria

- The user always gets 5 topic options before article drafting
- Topic options are informed by either hot trends or Qianfan-backed topic research
- The article style reflects the local blog at `127.0.0.1:5000`
- Titles are stronger and openings are more compelling than generic blog writing
- The article is published into the local blog project before GitHub backup
- The final markdown matches the backup repository format
- The post is backed up to GitHub without requiring a local clone of `blog-backup`

## Notes

- Prefer the local blog style over generic internet writing advice when there is a conflict
- Do not turn the article into pure SEO copy
- Do not overfit to one sample post; learn recurring patterns across recent posts
