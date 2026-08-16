# Website Builder Starterkit

Build and run a static content website by talking to Claude Code. No coding, almost
no terminal.

## Start here

1. Clone this repository:

   ```
   git clone https://github.com/thejasmn/website-builder-starterkit.git
   cd website-builder-starterkit
   ```

2. Open Claude Code in this directory — run `claude code` in your terminal from
   inside this folder, or open the Claude desktop app and point Claude Code at it.

3. Say:

   > help me build a website, what should i do first?

Claude takes it from there. It'll ask what your site is about, install what's missing,
and walk you to a live URL. [SETUP.md](SETUP.md) covers the same ground in detail if
you'd rather read ahead.

---

## What's in the kit

| File | What it does |
|---|---|
| [`CLAUDE.md`](CLAUDE.md) | The project rules Claude reads every session. The important one |
| `.claude/skills/project-setup/SKILL.md` | One-time setup instructions for Claude. Deletes itself when done |
| [`SETUP.md`](SETUP.md) | The 30-minute walkthrough: accounts, install, first deploy |
| [`LICENSE`](LICENSE) | Apache 2.0 |

Only `CLAUDE.md` and the setup skill are needed to bootstrap a project. The rest is
documentation for you.

## What you end up with

A fast, static content site — pages of writing, images, and links:

- **Astro** in static output mode. No server code, no database
- **Content in Markdown**, one file per page, filename = URL
- **Schema-validated** content, so a broken page fails the build instead of shipping
  quietly
- **Design tokens in one CSS file**, so the whole site restyles from one place
- **Deploys on push** to `main` via Cloudflare Workers Builds — no CI config, no manual
  deploy step

## What it costs

| | |
|---|---|
| GitHub | Free — stores the files, triggers each deploy |
| Cloudflare Workers | Free — 100,000 requests/day |
| Claude Pro | ~$20/month — the one paid piece |
| A domain | Optional, ~$10/year. You get a free `*.workers.dev` URL without one |

## Why `CLAUDE.md` matters

Sites built by chatting with an AI rot in predictable ways: copy leaks into templates,
every page gets one-off styling, invented statistics creep in, small requests turn into
refactors.

`CLAUDE.md` is a set of rules that prevents each of those specifically. Claude reads it
at the start of every session, so the fiftieth page is built the same way as the first.
It's meant to grow — when you decide something about your site, say *"remember this: …"*
and Claude writes it down there.

## Everyday use

```
"Add a page about composting covering what it is, why it matters, how to start."
"Change the homepage headline to 'Practical composting, minus the jargon'."
"Run the site locally so I can look at it before publishing."
"Undo the last change."
```

Claude edits the files, runs `npm run build` to confirm nothing broke, and pushes.
Cloudflare picks it up and the live site updates in a minute or two.

## License

Apache License 2.0 — see [LICENSE](LICENSE). The sites you build with it are yours.
