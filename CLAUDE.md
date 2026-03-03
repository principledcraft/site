# Claude Code Instructions

## Writing Style
- Never use em dashes (—). Use commas, semicolons, or restructure sentences instead.
- Fix small, obvious typos (spelling, punctuation) without asking. Just fix them.

## Workflow
- After completing a set of related changes, suggest a commit with a message.

## Project
- Hugo site using the Hextra theme (v0.12.0)
- After any content or config edit, run `hugo build` to verify the site builds without errors.
- Theme module cache: `/root/.cache/hugo_cache/modules/filecache/modules/pkg/mod/github.com/imfing/hextra@v0.12.0/`

## References
- Hextra theme docs: https://imfing.github.io/hextra/docs/
- Hextra GitHub: https://github.com/imfing/hextra
- Always check the Hextra docs and Hugo docs before searching theme source files

## Site Structure

### Content Sections
All principle content lives under `content/principles/` with subsections like `quality/` and `testing/`. Other top-level sections are **books** and **blog**. There is also a standalone `about.md`.

### Adding New Principle Pages
Create a markdown file in the appropriate subsection under `content/principles/`. Use `type: docs` in frontmatter. Link to related principles in other sections where relevant.

### Adding New Principle Subsections
1. Create `content/principles/<subsection>/_index.md` with `type: docs`
2. The Hextra sidebar will pick up the new subsection automatically

### Adding New Top-Level Sections
1. Create `content/<section>/_index.md`
2. Add a menu entry in `hugo.yaml` under `menus.main`
3. Add a feature card on the homepage (`content/_index.md`) if appropriate

### Books Section
Book pages use raw HTML for a styled card layout with cover image, metadata, and description, followed by a "Reflections" heading. Cover images go in `static/images/books/`. Use the `/add-book` skill to create new book pages.

### Styling
Prefer using default Hextra classes and theming. Only add custom CSS when necessary or explicitly requested, and always put it in `assets/css/custom.css`. Goldmark `unsafe: true` is enabled in `hugo.yaml`, so raw HTML in markdown works.

### Navigation
Menu structure is defined in `hugo.yaml`. "Principles" links to `/principles`, and subsections (Quality, Testing, etc.) are auto-generated in the sidebar by Hextra. Books, Blog, and About are top-level menu items.

## Content Guidelines
- Present both benefits and limitations of every idea. Be honest about tradeoffs rather than just selling a concept.
- Link to related principles in other sections where relevant.
- When mentioning a person by name, link to their Wikipedia page or personal website for context. Ask the user to verify the link before publishing.

## Code Examples
- Use Python with pytest conventions
- Always specify the language on fenced code blocks (e.g. ` ```python `) for syntax highlighting
- Keep examples short and focused: one concept per example, ideally under ~20 lines
- Use realistic domain names (e.g. `calculate_discount`, `send_notification`), not `foo`/`bar`
- Use Hextra's `{{< tabs >}}` shortcode for before/after comparisons ("Before" and "After" tabs)
- Only add inline comments when the code alone isn't enough
- Include type hints only when they aid readability, not everywhere
- Stick to standard library or well-known packages (no obscure dependencies)
- Avoid overly trivial examples (adding two numbers) and overly complex ones that distract from the principle
