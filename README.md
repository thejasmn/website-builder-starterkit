# Website Builder Starterkit

A starter kit for building and running a **static content website with Claude Code** —
for people who don't write code and would rather not spend time in a terminal.

You describe what you want in plain language. Claude scaffolds an [Astro](https://astro.build)
site, deploys it to [Cloudflare Workers](https://workers.cloudflare.com), and keeps it
consistent as it grows.

**New here? Start with [SETUP.md](SETUP.md)** — it takes about 30 minutes to go from
nothing to a live URL.

---

## What's in the kit

| File | What it does |
|---|---|
| [`SETUP.md`](SETUP.md) | The 30-minute walkthrough: accounts, install, first deploy |
| [`README.txt`](README.txt) | Plain-text version of the essentials — costs, accounts, everyday commands |
| [`CLAUDE.md`](CLAUDE.md) | The project rules Claude reads every session. This is the important one |
| `.claude/skills/project-setup/SKILL.md` | One-time setup instructions for Claude. Deletes itself when done |
| [`LICENSE`](LICENSE) | Apache 2.0 |

Only `CLAUDE.md` and the setup skill are strictly needed to bootstrap a project. The
rest is documentation for you.

---

## What you end up with

A fast, static content site — pages of writing, images, and links:

- **Astro** in static output mode, no server code and no database
- **Content in Markdown**, one file per page, filename = URL
- **Schema-validated** content collections, so a broken page fails the build instead of
  shipping quietly
- **Design tokens in one CSS file**, so the whole site restyles from one place
- **Deploys on push** to `main` via Cloudflare Workers Builds — no CI config, no manual
  deploy step

Running cost: $0 for hosting and source control. Claude Pro (~$20/mo) is the one paid
piece. A domain is optional and about $10/year.

---

## The idea behind `CLAUDE.md`

Sites built by chatting with an AI rot in predictable ways: copy leaks into templates,
every page gets its own one-off styling, invented statistics creep in, and small
requests turn into refactors.

`CLAUDE.md` is a set of rules that prevents each of those specifically. Claude reads it
at the start of every session, so the fiftieth page is built the same way as the first.
It's meant to be edited — when you make a decision about your site, tell Claude
*"remember this: …"* and it gets written down.

Read it before you start. It's the part of this kit that does the real work.

---

## Everyday use, after setup

```
"Add a page about composting covering what it is, why it matters, how to start."
"Change the homepage headline to 'Practical composting, minus the jargon'."
"Run the site locally so I can look at it before publishing."
"Undo the last change."
```

Claude edits the files, runs `npm run build` to confirm nothing broke, and pushes.
Cloudflare picks it up and the live site updates in a minute or two.

---

## License

Apache License 2.0 — see [LICENSE](LICENSE). The sites you build with it are yours.
