---
description: Prep a blog post for publishing and archive it to ~/knowledge/
---

You are running the /publish-post skill for this Hugo blog. Execute all four steps below in order. Make all metadata decisions yourself — do not ask the user for input unless you cannot determine the target file.

---

## Step 1 — Find the unpublished post

Use the Bash tool to list all `.md` files in `content/blog/` that do NOT start with `+++` or `---` (i.e. have no front matter). These are unpublished posts.

- If exactly one file matches: proceed with it automatically
- If multiple files match: list them and ask the user which one to process, then wait
- If none match: tell the user all posts already have front matter and stop

---

## Step 2 — Read and derive all metadata

Read the full file. Then derive the following — use your own judgement, do not ask:

- **title**: Extract from the leading `# Heading` if one exists. If not, infer a clear title from the content.
- **slug**: Convert the title to kebab-case, all lowercase, no special characters (e.g. "Digital Minimalism - Day 1" → `digital-minimalism-day-1`)
- **date**: Use today's date from `currentDate` in your context (format: `YYYY-MM-DD`)
- **description**: Write one concise sentence that captures the core point of the post. This is for SEO and link previews — make it informative, not a teaser.
- **tags**: Choose 2–4 short lowercase tags that accurately reflect the post's topics (e.g. `["habits", "attention", "personal"]`). Prefer specific over generic.
- **word_count**: Count the words in the post body (exclude the `#` heading line if present)
- **reading_time**: `ceil(word_count / 200)` — round up to nearest whole number, suffix with "min" (e.g. `3 min`)

---

## Step 3 — Format the blog post

Edit `content/blog/{filename}` with these changes. **Do not change any words — formatting only.**

1. **Add TOML front matter** at the very top of the file:
   ```
   +++
   title = "{title}"
   date = "{date}"
   slug = "{slug}"
   description = "{description}"
   tags = {tags as TOML array, e.g. ["habits", "attention"]}
   +++
   ```

2. **Remove the leading `# Heading`** if it exists and matches the title — the Hugo theme renders the `title` field as the page heading already, so leaving the `#` line causes it to appear twice.

3. **Fix double spaces**: replace any sequence of 2+ spaces between words with a single space.

4. **Fix spaces before punctuation**: remove any space that appears immediately before `,` `.` `!` `?` `)` — e.g. `time .` → `time.`, `words  ,` → `words,`.

5. Leave all other content exactly as written.

---

## Step 4 — Archive to knowledge corpus

1. Check if `~/knowledge/` exists. If not, create it with `mkdir -p ~/knowledge`.

2. Determine the archive filename: `{date}-{slug}.md` (e.g. `2026-06-27-digital-minimalism-day-1.md`)

3. Write the file to `~/knowledge/{date}-{slug}.md` with this exact structure:

```
---
title: "{title}"
date: {date}
slug: {slug}
description: "{description}"
tags: {tags as YAML list, e.g. [habits, attention, personal]}
source: "content/blog/{original-filename}"
archived: {today's date}
word_count: {N}
reading_time: {N min}
---

{the full formatted post body — identical to what is now in content/blog/, excluding the TOML front matter block}
```

The body in the corpus file starts after the YAML `---` block. It should contain the post text only (no `+++` TOML block — that belongs to Hugo, not to the corpus).

---

## On Finish

Tell the user:
- Which file was formatted and its new front matter (title, slug, tags)
- Where the corpus file was saved (`~/knowledge/{filename}`)
- The reading time and word count
- Remind them to `git add`, `git commit`, `git push` to deploy
