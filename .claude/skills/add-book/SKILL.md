---
name: add-book
description: Add a new book page to the books section
argument-hint: "Book Title" by Author Name
---

Create a new book page in `content/books/`. Ask the user for any details they haven't provided:
- Book title
- Author name (include common alias if applicable, e.g. "Uncle Bob")
- Publisher
- Publication date (month and year)
- A link where the book can be purchased
- A brief description of the book (1-2 paragraphs, approximately 465 characters to fill the card nicely)

### Cover Image
Try to find the book cover from Open Library using their Covers API:
1. Search for the book: `https://openlibrary.org/search.json?title={title}&author={author}`
2. Extract the `cover_i` ID from the search results
3. Build the cover URL: `https://covers.openlibrary.org/b/id/{cover_i}-L.jpg`
4. Share the cover URL with the user so they can verify it's the correct edition
5. Only after the user confirms, download it with `curl` to `static/images/books/{slug}.jpg`

If Open Library doesn't have a cover, ask the user to provide one. The image should be placed in `static/images/books/` with a filename matching the page slug. Cover images are constrained to 238px height via CSS, so any size will work.

Use this template for the page content:

```markdown
---
title: {Book Title}
type: docs
---

<div class="book-card">
  <img src="/images/books/{slug}.jpg" alt="{Book Title} cover">
  <div>

<p class="book-meta">{Author} &nbsp;·&nbsp; {Publisher} &nbsp;·&nbsp; {Date} &nbsp;·&nbsp; <a href="{link}">Available here</a></p>

{Description}

  </div>
</div>

```

### Book Grid
After creating the book page, add the cover to the book grid in `content/books/_index.md`. Insert a new `<a>` entry inside the `<div class="book-grid">`, maintaining alphabetical order by title:

```html
  <a href="/books/{slug}"><img src="/images/books/{slug}.jpg" alt="{Short Title}"></a>
```

### Humanize
After writing the description, always run the humanizer skill on it to remove any AI writing patterns. This is mandatory, not optional. Do it automatically without being asked.

Important:
- Do not include a Reflections section. The user will add one themselves if needed.
- After generating the book summary/description, show it to the user for verification before writing the file.
- Never use em dashes (—) in the content.
