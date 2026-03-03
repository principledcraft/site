---
name: add-blog-post
description: Add a new blog post with guided structuring
argument-hint: optional topic or title
---

# Add a new blog post

Guide the user through writing a new blog post for `content/blog/`. The target length is around 300 words of body content (roughly 5 short paragraphs), but there is no hard limit. Let the post be as long or short as it needs to be.

## Step 1: Get the overview

Ask the user for a high-level summary of what the post is about. Just a sentence or two is fine. If the user already provided a topic or summary as an argument, skip this step.

## Step 2: Ask structuring questions

Based on the summary, ask 3-5 short questions to help shape the post. Tailor these to the topic, but examples include:

- What prompted you to write about this now?
- Is there a specific experience or example you want to anchor the post around?
- What's the one thing you want readers to take away?
- Are there any common misconceptions you want to address?
- Do you want to reference any books, talks, or articles?
- Is there a connection to any of the principles on the site?

Ask all the questions in a single message using AskUserQuestion (or plain text if more natural). Do not ask yes/no questions; aim for open-ended ones that draw out the user's voice and perspective.

## Step 3: Write the post

Using the user's answers, write the blog post. Follow these rules:

### Title and summary as hooks

The title and summary should work as hooks, not literal descriptions. Look at the existing blog for reference:

- **Title:** "It's a principled craft" is evocative and intriguing, not a dry label like "Why I made this site". The title should make someone want to click. Keep it short and punchy.
- **Summary:** The summary in the frontmatter acts like an opening hook for the blog listing page. It should tease the post's angle or tension in 2-3 sentences, making the reader curious enough to click through. It is not a neutral abstract; it has voice and a point of view.

### Frontmatter format

```yaml
---
title: {Hook-style title}
date: {today's date in YYYY-MM-DD format}
summary: {2-3 sentence hook that draws the reader in, not a neutral abstract}
authors:
  - name: PrincipledCraft
draft: false
---
```

### Writing style

- Write in first person, matching the personal and reflective tone of the existing blog
- Keep paragraphs short (3-5 sentences)
- Do not use headings or subheadings unless the post genuinely needs them
- Do not use bullet lists; write in prose
- Link to related principle pages on the site where relevant (e.g. `/principles/quality/simplicity`)
- When mentioning a person by name, link to their Wikipedia page or personal website. Ask the user to verify the link before publishing
- Never use em dashes
- Generate the slug from the title (lowercase, hyphens, no special characters)

## Step 4: Humanize

Always run the humanizer skill on the final content to remove any AI writing patterns. This is mandatory, not optional. Do it automatically without being asked.

## Step 5: Save, build, and let the user review in the browser

1. Write the file to `content/blog/{slug}.md`
2. Run `hugo build` to verify the site builds without errors
3. Start the Hugo dev server (`hugo server`) if it is not already running
4. Tell the user the local URL where they can read the post in their browser (e.g. `http://localhost:1313/blog/{slug}/`)
5. Ask the user to review the post in the browser, then make any requested changes

Do NOT show the full draft in chat first. Always write the file and build immediately so the user can read it rendered in the browser. If the user asks for changes afterwards, apply them and rebuild.

## Step 6: Commit

After the user is happy with the post, suggest a commit message.

Important:
- Never use em dashes (—) in the content
- Always write the file and build first, then let the user review in the browser
- Match the voice and tone of the existing blog posts
