# [SITE NAME]

> **Setup note:** the `project-setup` skill fills in the bracketed placeholders in this
> file during first-time setup and then deletes itself. If you're reading this and the
> brackets are still here, setup hasn't finished — run the `project-setup` skill.

**What this site is:** [one sentence — what it covers and who it's for]

**Voice:** [e.g. plain, direct, third person. No hype, no marketing language.]

A static content site. Astro (static output) deployed to Cloudflare Workers.
Everything here is **public and static** — no login, no database, no server code,
no API routes, no secrets. Don't add any of those without being asked.

---

## Who is asking

Requests come from a **non-technical or semi-technical owner** in plain language
("beef up the pricing page", "make the titles better for search"). So:

1. Make the change in the **one correct place**. Never expand a small ask into a refactor.
2. **Explain what you did in plain English** — what changed, on which page, in one or two
   sentences, plus the URL to look at. No file trees, no jargon, unless asked.
3. If a request is genuinely ambiguous, **ask one short question** instead of guessing.
4. Before saying something is done, **run `npm run build` and confirm it exits 0.**

---

## The rules that keep this project from becoming a mess

These exist because sites like this reliably rot in the same handful of ways.

### 1. All content lives in Markdown files. Never in Astro files.

Every page's words live in `src/content/<section>/<slug>.md`. The `.astro` files are
*templates* — they decide how a page looks, never what it says.

**Never** write copy, headings, FAQ text, or lists of links directly inside an `.astro`
file. If you catch yourself typing a sentence of real content into a component, stop:
it belongs in a `.md` file, and the component should read it.

Why: the owner can edit a `.md` file safely. They cannot safely edit an `.astro` file.
The moment content leaks into templates, they lose the ability to change their own site.

### 2. One file per page, and the filename is the URL.

`src/content/guides/composting.md` → `/guides/composting`. Adding a page is adding a
file. **Never create a new `.astro` file to add a page** — each section has one
`[slug].astro` route that renders every page in that section.

### 3. Every content collection has a schema, and the build enforces it.

Schemas live in `src/content.config.ts`. A page missing a required field, or with the
wrong type, **fails the build with a specific error**. That's the safety net — it's the
only thing standing between the owner and a silently broken page.

Extend a schema only when a real page needs a field it doesn't have. Never add fields
speculatively.

### 4. `npm run build` must exit 0 before anything is called done.

There is no other check in this repo. If the build fails, the change isn't finished.

### 5. Reuse the existing components. Don't restyle per page.

The shared components cover every page shape this site has. If a page seems to need a
new look, that's a conversation with the owner about the design system — **not** a
one-off `<style>` block buried in one page. One-off styles are how a site ends up
looking like five different sites.

### 6. All colors, fonts, and spacing come from `src/styles/global.css`.

That file holds the design tokens as CSS custom properties. **Never hardcode a hex color
or a font stack anywhere else.** Changing a token changes the whole site — which is the
point, and also why you confirm with the owner before changing one.

### 7. Every factual claim needs a real source. Never invent one.

No made-up statistics, prices, dates, organization names, or "typical" figures. If you
can't verify it with a web search, **hedge the language or leave it out** — don't state
it as fact. Made-up specifics are the fastest way to destroy a content site's credibility,
and they're invisible until someone notices.

If a number is wanted but unavailable, mark it clearly as pending rather than estimating.

### 8. Images go in `src/assets/`, never in `public/`.

Images under `src/assets/` get optimized at build time. Images in `public/` do not — they
ship at full size and make the site slow. `public/` is only for site-level files: favicon,
`robots.txt`, the social-sharing image.

Use only images you have the right to use. Free-licensed stock (Unsplash's free license,
not Unsplash+) or the owner's own photos. Resize source files to **≤ 1800px wide** before
committing them.

### 9. Stay in scope. Open the fewest files.

Fix what was asked. If you spot something else that's wrong, **mention it and move on** —
add it to `TODO.md`. Don't fix it in the same change unless asked.

---

## Where things live

```
src/
  content.config.ts        ← the schema for every content collection. Start here to
                             understand what fields a page can have.
  content/
    <section>/*.md         ← ALL page content. Filename = URL slug.
  pages/
    index.astro            ← homepage (thin — copy lives in components/home/*)
    about.astro            ← hand-written standalone pages
    <section>/
      [slug].astro         ← renders any page in that section's collection
      index.astro          ← the section's landing page (list of its pages)
  layouts/
    Layout.astro           ← the only layout. <head> (title, description, canonical,
                             social tags) + header + page + footer.
  components/              ← shared, reusable page furniture
  assets/images/           ← every image on the site
  styles/global.css        ← ALL design tokens + base styles. No other tokens file.
public/                    ← favicon, robots.txt, social image. Site-level files ONLY.
astro.config.mjs           ← site URL, adapter, sitemap
wrangler.jsonc             ← Cloudflare deploy config
CLAUDE.md                  ← this file
TODO.md                    ← open tasks + decision log + change history
```

---

## "I want to change…" → edit only this

| The request is about… | Edit only |
|---|---|
| The words, headings, or FAQs on a page | `src/content/<section>/<slug>.md` |
| Adding a new page | a new `.md` in `src/content/<section>/` (+ add its slug to the section landing page's list) |
| A section's landing page (intro text, ordering) | `src/pages/<section>/index.astro` |
| Navigation links / the menu | `src/components/Header.astro` |
| Footer links | `src/components/Footer.astro` |
| Colors, fonts, page width | `src/styles/global.css` — **affects the whole site; confirm first** |
| How every article page looks | the shared article components — one edit updates all pages |
| Homepage | `src/pages/index.astro` + `src/components/home/*` |
| Page titles, meta descriptions, social tags | `src/layouts/Layout.astro` + each page's `title`/`description` |
| The live domain, sitemap | `astro.config.mjs` |

If a request isn't in this table, read the map above before opening anything.

---

## Deploy

```bash
npm run dev      # preview locally at localhost:4321
npm run build    # must exit 0 before any change is done
npm run preview  # preview the built site
```

Push to `main` → **Cloudflare Workers Builds** auto-builds and deploys. There is no
separate deploy step and no GitHub Actions.

| | |
|---|---|
| Live site | [LIVE URL] |
| Repository | [GITHUB URL] |

---

## Keeping this file current (do this without being asked)

Update `CLAUDE.md` **in the same commit** whenever you:

- Add, remove, or change a content collection's schema → update the map and tables above
- Add a component, page directory, or config file → add it to the map
- Change the deploy target, build command, or domain → update *Deploy*
- Add a deliberate exception to one of the rules above → write it down here

**Don't** touch this file for content-only edits (copy, FAQs, image swaps).

---

## When the owner says "remember this"

They won't say where it belongs. You decide, before writing anything:

1. **A rule that applies to every change** → *The rules* section above, 1–2 lines. Should
   grow rarely.
2. **A design or writing-style rule** (color, type, layout, voice, page template) →
   `STYLEGUIDE.md`, in the matching section. Create that file the first time one is needed.
3. **A decision, a "we tried X and chose Y", or a follow-up task** → `TODO.md`, dated.
4. **A detailed procedure for a workflow that keeps recurring** (e.g. "how to add a page
   to this section") → a skill under `.claude/skills/<name>/SKILL.md`. Don't create one
   for a single instruction.
5. **Already visible in the code or one of these files** → don't save it twice; say where
   it already lives.

**One fact lives in one place.** If a new note updates an existing one, edit that note
rather than adding a second. After saving, confirm in plain language what you recorded
and where.

---

## Known limitations — deliberate current state, not a cleanup list

_(Add to this as the site grows. Anything here is a known trade-off, not a bug to
opportunistically fix.)_

- [e.g. "the search box is a visual stub — a real search index comes later"]
