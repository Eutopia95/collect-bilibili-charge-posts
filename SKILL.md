---
name: collect-bilibili-charge-posts
description: Use an already signed-in browser to collect a Bilibili creator's charge-exclusive (充电专属) posts for a requested month and turn each substantive post into a clean Markdown file. Use when the user asks to整理、归档、导出或下载 B站充电专属/充电动态，尤其是按月份处理、排除资料打包或抽奖帖、保留原帖信息与配图，以及要求文件名以日期开头时。
---

# Collect Bilibili Charge Posts

Use the user's signed-in browser session to identify and archive eligible charge-exclusive posts. Preserve the author's substance; improve only Markdown structure and readability.

## Resolve the request

Extract these parameters from the request and prior conversation:

- Creator space URL or UID.
- Target month and year.
- Output directory. For the creator 好人松松 (UID 2078781964), default to the fixed folder `/Users/echo/Downloads/好人松松/`. For other creators, default to a clearly named subfolder under the user's Downloads directory.
- Inclusion and exclusion rules.

Apply these defaults unless the user says otherwise:

- Include only posts visibly marked as charge-exclusive on the creator's dynamic page.
- Include substantive long-form posts published within the requested calendar month.
- Exclude monthly material-package posts (`充电资料打包整理`), lotteries or participation posts (`抽奖`, `参与`), advertisements, and public reposts or public excerpts.
- Name every file `YYYY-MM-DD_标题.md`.
- For 好人松松 (UID 2078781964), put every month's Markdown files directly in `/Users/echo/Downloads/好人松松/`, without creating month-specific subfolders.
- For other creators, put all files for one month in `Downloads/<博主名>_YYYY年M月充电专属/` unless the user specifies another location.

Treat a request such as “继续处理” as reusing the creator, browser, filtering rules, and output convention established earlier in the task.

## Use the browser safely

1. Use the available Chrome or browser-control skill and read its instructions before browser work.
2. Prefer the user's already signed-in browser. Do not inspect cookies, local storage, passwords, or session files.
3. Open the creator's dynamic page and inspect visible cards. A charge-exclusive post is identified by the site's visible charge badge or its corresponding rendered badge element; do not infer exclusivity from wording alone.
4. Build the complete candidate list for the requested month before writing files. Scroll or load more only as needed to cover the month boundary.
5. Open each eligible post's canonical opus page and extract the full visible title, exact publication timestamp,正文, references, links, and source-derived image URLs.
6. Do not transmit paid content to any third party. Local Markdown output requested by the signed-in user is allowed.
7. Finalize browser tabs when extraction is complete.

## Verify the monthly set

Before generating files, report a concise count and list logic in commentary, for example: “共 6 篇符合条件；资料打包和抽奖已排除.”

Check these edge cases:

- Posts on the first or last day belong to the month according to the exact timestamp on the opus page.
- A public post that quotes or screenshots charge material is not charge-exclusive and is excluded by default.
- A charge-exclusive monthly package is still excluded by default.
- Multiple posts on one date remain separate files.
- When the list page shows only `MM月DD日`, use the exact year from the opus detail page.

## Produce Markdown

Create one file per eligible post using this shape:

```markdown
# 原帖标题

正文……

---

原文链接：<https://www.bilibili.com/opus/ID>

## 征引文献

[^1]: Full reference
```

Do not add YAML frontmatter, note properties, or an introductory metadata block. Do not place the author, publication time, permission label, archive note, disclaimer, collection date, edit time, platform, or type at the beginning of the file. Place exactly one visible, clickable canonical source URL in the form `原文链接：<https://www.bilibili.com/opus/ID>` immediately before the scholarly-reference section, with a Markdown horizontal rule (`---`) immediately before the link. Because Obsidian renders native footnotes at the document bottom, this keeps the source link visually before the footnote bibliography. If a post has no scholarly-reference section, end the file with the horizontal rule and source link.

Formatting rules:

- Preserve all substantive claims, examples, numeric values, formulas, citations, and author caveats.
- Convert scholarly citations to native Obsidian footnotes: use `[^1]` at every in-text citation and `[^1]: Full reference` for the matching definition. Keep identifiers unique within each file, and verify that every citation has one definition and every definition is cited. Do not use plain `[1]` markers or numbered Markdown lists for cited literature.
- Do not summarize away paragraphs. Do not silently correct factual claims.
- Convert emoji-number headings and bracketed headings into sensible Markdown heading levels.
- Convert leading asterisks and enumerations into valid Markdown lists where appropriate.
- Normalize spacing, punctuation, units, and paragraph breaks only when meaning is unchanged.
- Keep original links clickable.
- Deduplicate repeated image URLs and place images near their referenced passage when the mapping is clear; otherwise add an `## 原帖配图` section at the end.
- Keep warnings or caveats that are part of the author's substantive post. Do not add a separate `整理说明` at the end of the file.

## Write and validate files

1. Draft files inside the current writable workspace using `apply_patch` when practical.
2. Validate that every file is nonempty and contains the expected title, substantive正文, and exactly one canonical source URL preceded by a horizontal rule and placed immediately before the reference section (or at the end when no reference section exists).
3. Confirm the filename begins with the exact publication date.
4. Confirm the number of Markdown files equals the verified eligible-post count.
5. Validate native footnotes: every `[^id]` reference has exactly one `[^id]:` definition, every definition is used, and no plain numeric citation markers remain.
6. Request filesystem approval when writing to Downloads if it is outside the workspace.
7. If replacing earlier output, move obsolete or duplicate files to the system Trash rather than deleting permanently, and tell the user they are recoverable.
8. Deliver a clickable absolute path to the output folder and state the included count and exclusions.

Do not include comments, reaction counts, navigation text, unrelated recommendations, or page chrome in the Markdown.
