# siddharth7113.github.io

Personal website. Live at <https://siddharth7113.github.io>.

Built on Jekyll. The original scaffolding came from the [al-folio](https://github.com/alshedivat/al-folio) template — kept for the typography and the publication/project pieces, then trimmed down to what I actually use.

## Local development

The repo ships a Docker Compose setup so I don't have to keep a working Ruby toolchain on every machine.

```bash
docker compose pull
docker compose up
```

Then open <http://localhost:8080>. Live reload is wired up on port 35729; edits to markdown/liquid files re-render without a restart.

If `imagemagick` errors show up the first time, it's because the image variants haven't been generated yet — they'll appear after the first build.

## Layout

- `_pages/` — top-level pages (about, projects, books, blog, …).
- `_posts/` — blog posts.
- `_projects/` — project cards rendered on `/projects/`.
- `_books/` — books I'm reading. Each book is `_books/<slug>.md` (the index page) plus a directory `_books/<slug>/ch-XX.md` per chapter note. Layouts live in `_layouts/book.liquid` and `_layouts/book_chapter.liquid`.
- `_news/` — short announcement items for the about page.
- `assets/img/` — images. Cover images for books go under `assets/img/books/`.
- `_config.yml` — site config. Footer text, social handles, collection setup, scholar bibliography settings, all here.

## Adding a new book

1. Create `_books/<slug>.md` with front matter:
   ```yaml
   ---
   layout: book
   title: <Book title>
   slug: <slug>           # must match the directory name below
   is_book: true
   author: <author>
   status: currently reading   # or: paused | finished
   category: technical          # optional, free-form
   cover: /assets/img/books/<slug>.jpg
   description: <one-line blurb>
   ---
   ```
2. Drop the cover image at `assets/img/books/<slug>.jpg`.
3. For each chapter note, create `_books/<slug>/ch-XX.md`:
   ```yaml
   ---
   layout: book_chapter
   title: "Chapter X — <name>"
   book: <slug>
   order: X
   description: <one-line summary>
   ---

   ... your markdown notes ...
   ```

The book page auto-generates a table of contents from any chapter file with `book: <slug>`, sorted by `order`.

## Deployment

GitHub Actions deploys to GitHub Pages on push to `main` (see `.github/workflows/`).
