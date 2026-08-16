---
name: project-setup
description: ONE-TIME first-run setup for this site — installs prerequisites, connects the GitHub repo, scaffolds the Astro content site, configures Cloudflare Workers deployment and MCP/docs tooling, fills in CLAUDE.md, and then deletes itself. Load only when the site has not been set up yet. If src/content/ already exists with pages in it, setup is done and this skill should be deleted rather than run.
---

# One-time project setup

You are setting up a brand-new content website for someone who is **not comfortable
with the terminal**. They have a GitHub account and an empty repository. Everything
else is your job.

**Stop and delete this skill if setup already happened.** If `src/content/` exists and
contains `.md` pages, say so and offer to remove this skill file — do not re-run it.

## How to behave during this whole process

- **Explain before you run.** One sentence, plain English, before each command:
  *"This installs Node.js, which is the tool that builds your site."*
- **Batch approvals sensibly.** Don't ask permission thirty separate times; group
  related commands and describe the group.
- **Never assume they know a term.** Repository, branch, commit, deploy — define each
  the first time you use it, in one clause.
- **Check in at the numbered milestones below**, not between every command.
- **If something fails, fix it yourself first.** Only surface an error to them if you
  genuinely need a decision or a credential.

---

## Step 0 — Gather what you need (ask, then proceed)

Ask for these in one message if you don't already have them:

1. **GitHub repository URL** (`https://github.com/name/repo.git`)
2. **What the site is about** — one or two sentences
3. **A name for the site** — how it should appear in the header and browser tab
4. **Roughly what sections it should have** — if they don't know, propose 3–4 based on
   the topic and let them adjust. Don't stall on this; a reasonable default beats an
   interview.

Then tell them: *"Give me about ten minutes. I'll check in when there's something to
look at."*

---

## Step 1 — Prerequisites

Check what's already installed before installing anything:

```bash
node --version    # need v22.12 or higher
git --version
```

- **Node.js missing or too old** → install the current LTS. On macOS, prefer Homebrew
  (`brew install node`) if Homebrew exists; otherwise direct them to
  https://nodejs.org and have them run the installer, then re-check.
- **Git missing** → macOS: `xcode-select --install`. Windows: https://git-scm.com.

Configure git identity if it isn't set (`git config --global user.name` / `user.email`) —
use their GitHub username and email. Without this, saving changes fails later with a
confusing error.

**Milestone 1:** report versions, confirm both are good, move on.

---

## Step 2 — Connect the folder to GitHub

Working in their project folder:

```bash
git init                                  # if not already a repo
git remote add origin <THEIR_REPO_URL>    # or set-url if origin exists
git fetch origin
git checkout -b main origin/main          # or pull, if main already exists locally
```

**Authentication:** the first `git push` will need credentials. The smoothest path for a
non-terminal user is the **GitHub CLI**:

```bash
gh auth login    # choose GitHub.com → HTTPS → login with a web browser
```

Install it first if needed (`brew install gh`, or https://cli.github.com). It opens a
browser, they click approve, and git works from then on with no tokens to manage.

If `gh` isn't available, fall back to walking them through GitHub Desktop
(https://desktop.github.com) — sign in, **Add** → **Clone repository** — which handles
auth the same way with a GUI.

**Milestone 2:** confirm the folder is linked to their repo and they can push.

---

## Step 3 — Scaffold the site

Create a **minimal Astro static site**, by hand rather than by an interactive template
picker (interactive prompts are a bad experience here). Target this shape:

```
src/
  content.config.ts        ← one schema-validated collection per section
  content/<section>/*.md   ← all page content
  pages/
    index.astro
    <section>/[slug].astro
    <section>/index.astro
  layouts/Layout.astro
  components/              ← Header, Footer, and the shared article furniture
  assets/images/
  styles/global.css        ← ALL design tokens as CSS custom properties
public/                    ← favicon, robots.txt only
```

Concretely:

1. `npm create astro@latest . -- --template minimal --no-install --no-git --yes`
   (or write the files directly if that's cleaner)
2. Install: `npm install` then
   `npx astro add cloudflare sitemap --yes`
3. Set `output: "static"` and the `site` URL in `astro.config.mjs`.
4. **Set the Cloudflare adapter's image service to
   `imageService: { build: "compile", runtime: "passthrough" }`.** Without this, `astro dev`
   crashes on images, because the local dev sandbox can't load Sharp's native bindings.
   This is a known sharp edge — set it now, don't wait to hit it.
5. Write `src/content.config.ts` with a shared base schema (`title`, `description`,
   `summary`) that each section's collection extends. Keep it small — fields get added
   when a real page needs them, never speculatively.
6. Write `src/styles/global.css` with design tokens as custom properties: 2–3 colors,
   a system font stack, one page width. **System fonts only** unless asked — webfonts
   are a performance cost the site hasn't earned yet.
7. Create **2–3 real example pages** as `.md` files in the sections they named, with
   actual (if placeholder) prose — not lorem ipsum. These are the templates they'll
   copy from, so they should look like the real thing.

Rules that must hold in what you build, because everything downstream depends on them:

- **No page copy inside any `.astro` file.** Templates render content; they never contain it.
- **One `[slug].astro` per section**, rendering every page in that collection. Adding a
  page must never mean adding an `.astro` file.
- **No hardcoded colors or fonts** outside `global.css`.

Then: `npm run build` — **it must exit 0.** Fix anything that fails before continuing.

**Milestone 3:** run `npm run dev`, give them the `localhost:4321` link, and ask them to
look at it. Wait for their reaction before deploying.

---

## Step 4 — Tooling: MCP servers and skills

Set the project up so future sessions have current documentation rather than relying on
your training data. Framework and platform APIs change; guessing at them is the single
most common source of broken builds in projects like this.

1. **Check what's already available.** Some Claude installs ship Cloudflare skills or
   plugins already. Look before installing anything, and don't duplicate.

2. **Cloudflare MCP.** Cloudflare publishes hosted MCP servers — including a
   documentation server and a bindings/management server.
   **Look up the current list and endpoint URLs at
   https://developers.cloudflare.com/mcp-server/ (or search "Cloudflare MCP server")
   rather than using a URL from memory — these have moved before.** Then add the
   documentation server, and the management server if they want Claude able to inspect
   their Cloudflare account:

   ```bash
   claude mcp add --transport sse <name> <url>
   ```

   Management servers require an OAuth approval in the browser. Walk them through it.
   **If you cannot verify a current endpoint, skip it and say so** — a fabricated MCP
   URL is worse than none. Fetching docs pages directly works fine as a fallback.

3. **Astro documentation.** Check whether Astro currently publishes a docs MCP server
   (search the Astro docs). If yes, add it. If not, note in `CLAUDE.md` that Astro
   questions should be answered by fetching https://docs.astro.build rather than from
   memory.

4. **Create the ongoing skills this project will actually reuse.** At minimum, write
   `.claude/skills/add-page/SKILL.md` documenting the exact procedure for adding a page:
   which folder, the required frontmatter fields with an example, where the slug also
   has to be registered so it shows up in navigation, and the "`npm run build` must exit
   0" check. Derive it from the site you just built, so it matches reality.

   Add more skills only when a workflow genuinely recurs. One instruction is not a skill.

**Milestone 4:** tell them in one sentence what tooling you connected and what it buys them.

---

## Step 5 — Deploy to Cloudflare

Two parts: get it live once, then make it automatic.

**5a. First deploy (immediate, gives them a URL):**

1. If they don't have a Cloudflare account, send them to
   https://dash.cloudflare.com/sign-up — free, **no credit card needed**.
2. Write `wrangler.jsonc`: a `name` (their site name, lowercase-hyphenated), a current
   `compatibility_date`, `main` pointing at the Astro Cloudflare adapter's server
   entrypoint, and an `assets` block pointing at `./dist`.
3. `npx wrangler login` — opens a browser, they click approve.
4. `npm run build && npx wrangler deploy`
5. **Give them the live `*.workers.dev` URL.** This is the moment the project becomes
   real to them — lead with it.

**5b. Automatic deploys on every change:**

In the Cloudflare dashboard: **Workers & Pages** → their Worker → **Settings** →
**Build** → connect the GitHub repository, branch `main`, build command `npm run build`.
This is a browser task — give them numbered clicks, and offer to check afterwards that
a push triggers a build.

From then on: saving a change to `main` publishes the site. No deploy step to remember.

**Domains:** they don't need one. If they want one later, Cloudflare Registrar sells at
cost and connects automatically. Note in `CLAUDE.md` that `site:` in `astro.config.mjs`
points at the workers.dev URL until a real domain exists.

**Milestone 5:** confirm the live URL works and that a test change auto-deploys.

---

## Step 6 — Finish and retire this skill

1. **Fill in every bracketed placeholder in `CLAUDE.md`:** site name, what the site is,
   voice, the live URL, the GitHub URL. Update the file map and the "I want to change…"
   table to match the sections you actually created. **Leave the rules section intact** —
   it's the part that keeps the project coherent.
2. **Create `TODO.md`** with a dated first entry recording the setup decisions you made:
   sections chosen, Cloudflare config, MCP servers added, anything deliberately deferred.
   This file is the project's decision log from here on.
3. **Commit and push everything.**
4. **Delete this skill** (`rm -rf .claude/skills/project-setup`) and commit that too.
   It has done its job; leaving it around invites a destructive re-run.
5. **Hand off in plain language.** Tell them:
   - their live URL
   - that they now just describe changes in chat, and "publish it" makes them live
   - that "remember this: [rule]" permanently teaches the project a preference
   - one concrete example of a first change to try

Keep the handoff to a short paragraph. They've read enough by this point.
