# neel.blog

Personal site built with [Hugo](https://gohugo.io/) and the [Bear Blog theme](https://github.com/janraasch/hugo-bearblog). Deployed automatically via Netlify on every push to `main`.

---

## How it works

```
content/          ← your writing lives here
hugo.toml         ← site config (title, description, etc.)
layouts/partials/ ← custom CSS/JS overrides (dark mode toggle)
static/images/    ← favicon and social share image
themes/hugo-bearblog/ ← the theme (git submodule, don't edit)
```

When you push to GitHub, Netlify runs `hugo` and publishes the `public/` folder. You never touch `public/` manually.

---

## Adding a blog post

1. Create a new file at `content/blog/your-post-title.md`
2. Add this block at the top (called "front matter" — Hugo reads it to know the title, date, etc.):

```toml
+++
title = "Your Post Title"
date = "2026-06-27"
description = "One sentence for SEO and link previews. Optional."
tags = ["spark", "data-engineering"]
+++

Your content goes here in plain Markdown.
```

3. Write the post body in Markdown below the closing `+++`
4. `git add`, `git commit`, `git push` — Netlify deploys automatically

The post will appear at `yoursite.com/your-post-title/` and show up in the blog list.

### Front matter fields

| Field | Required | What it does |
|---|---|---|
| `title` | Yes | Post heading and page title |
| `date` | Yes | Sort order in the blog list |
| `description` | No | SEO meta description and link preview text |
| `tags` | No | Groups posts by topic |
| `draft = true` | No | Hides the post from production until removed |

### Markdown quick reference

```markdown
# Heading 1
## Heading 2

**bold**   *italic*   `inline code`

[link text](https://example.com)

> blockquote

- bullet
- list

1. numbered
2. list
```

Code blocks with syntax highlighting (line numbers are on by default):

````markdown
```java
public class Example {
    public static void main(String[] args) {
        System.out.println("Hello");
    }
}
```
````

Supported languages: `java`, `python`, `sql`, `bash`, `go`, `javascript`, `toml`, and [many more](https://gohugo.io/content-management/syntax-highlighting/#list-of-chroma-highlighting-languages).

---

## Editing existing pages

### Homepage (`content/_index.md`)

Edit the text that appears when someone lands on the site. The nav links at the bottom (`Blog · About`) are hardcoded there — update them if you add new pages.

### About page (`content/bear.md`)

Plain Markdown. No special front matter needed beyond `title`, `menu`, and `slug`. The `slug = "about"` line controls the URL (`/about/`).

---

## Site-wide config (`hugo.toml`)

```toml
title = "Neel Khare"          # browser tab and header
baseURL = "https://..."       # set this to your actual Netlify URL

[params]
  description = "..."         # homepage meta description
  dateFormat = "2006-01-02"   # date format shown on posts (Go time format)
  enablePostNavigator = true  # previous/next links at bottom of posts
```

> **Note:** Hugo uses a quirky date format. `2006-01-02` means YYYY-MM-DD. `January 2, 2006` means "Month Day, Year". Don't change the numbers — change the layout of them.

---

## Customisations already in place

### Dark / light mode toggle

A button appears top-right on every page. It respects the visitor's system preference on first load, then remembers their choice.

The code lives in two files:
- `layouts/partials/custom_head.html` — CSS variables for both colour schemes and a script that sets the theme before the page renders (prevents flash)
- `layouts/partials/custom_body.html` — the toggle button and its click handler

To change colours, edit the CSS variables in `custom_head.html`:

```css
html.dark {
  --background-color: #01242e;
  --text-color: #ddd;
  --link-color: #8cc2dd;
  /* etc. */
}
html.light {
  --background-color: #fff;
  --text-color: #444;
  --link-color: #3273dc;
}
```

### Code highlighting

Set in `hugo.toml` under `[markup.highlight]`:
- Style: `friendly` (light-coloured theme)
- Line numbers: on by default

To change the style, replace `friendly` with any name from the [Chroma style gallery](https://xyproto.github.io/splash/docs/).

---

## Adding a new page (e.g. Projects, Now)

1. Create `content/projects.md` with front matter:

```toml
+++
title = "Projects"
menu = "main"
weight = 30
slug = "projects"
+++
```

2. `weight` controls nav order — homepage is 1, about is 20, so 30 puts this after about
3. Add a link to it from `content/_index.md` if you want it on the homepage nav

---

## Local preview

You need Hugo installed (`brew install hugo` on Mac).

```bash
hugo server -D
```

Open `http://localhost:1313`. The `-D` flag shows draft posts. Changes reload live in the browser.

---

## Deployment

Push to `main` → Netlify builds → site updates in ~30 seconds.

Netlify config (`netlify.toml`):
```toml
[build]
  command = "hugo"
  publish = "public"

[build.environment]
  HUGO_VERSION = "0.151.2"
```

The `public/` folder is gitignored and rebuilt fresh by Netlify on every deploy — never commit it.

---

## Theme updates

The theme lives at `themes/hugo-bearblog/` and is a git submodule. To update it:

```bash
git submodule update --remote themes/hugo-bearblog
git add themes/hugo-bearblog
git commit -m "update bearblog theme"
git push
```

Do not edit files inside `themes/` — overrides go in the top-level `layouts/` folder instead (which is already how the dark mode toggle works).
