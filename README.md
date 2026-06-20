# neel_bearblog

Personal blog built with [Hugo](https://gohugo.io) and the [bearblog theme](https://github.com/janraasch/hugo-bearblog).

## Writing a new post

**1. Start the local server**

```bash
cd ~/neel_bearblog
hugo server
```

Open `http://localhost:1313` in your browser. It hot-reloads — any file you save appears instantly.

**2. Create the post file**

Posts live in `content/blog/`. Create a new `.md` file there:

```bash
# filename becomes part of the URL, so keep it short and hyphenated
touch content/blog/my-post-title.md
```

**3. Add the frontmatter at the top**

Every post needs this header block:

```
+++
title = "Your post title"
date = "2026-06-20"
description = "One sentence shown in the blog list."
draft = false
+++

Your content starts here.
```

- `title` — shown on the post and in the blog list
- `date` — used to sort posts (newest first). Format: YYYY-MM-DD
- `description` — the subtitle shown under the title on the /blog listing page
- `draft = false` — set to `true` if you want to write without it going live

**4. Write in Markdown**

```markdown
# Heading

Normal paragraph text.

**bold**, *italic*, `inline code`

## Subheading

- bullet
- list

> blockquote

\```java
// fenced code block with syntax highlighting
public class Example {}
\```
```

**5. Check it looks right**

With the server running, open `http://localhost:1313/blog/` to see your post in the list, then click through to read it. Check it on a narrow window too — the theme is mobile-friendly but worth verifying.

**6. When you're done**

```bash
git add content/blog/my-post-title.md
git commit -m "post: my post title"
git push
```

Cloudflare Pages auto-deploys on push to main. Your post is live within ~30 seconds.

---

## Editing existing content

| File | What it controls |
|---|---|
| `content/_index.md` | Homepage text |
| `content/bear.md` | About page |
| `content/blog/*.md` | Individual posts |
| `hugo.toml` | Site title, description, baseURL |

---

## Stopping the local server

`Ctrl+C` in the terminal where `hugo server` is running.
