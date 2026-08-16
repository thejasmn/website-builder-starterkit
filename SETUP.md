# Setup — from nothing to a live website

This gets you from nothing to a **live website on the internet** in about 30 minutes,
without writing any code and with almost no time in a terminal.

You will do three things:

1. Create a GitHub account (5 min)
2. Install the Claude desktop app (5 min)
3. Hand Claude two files and let it build and deploy the site (20 min)

That's it. Claude does the rest — including creating the folder on your computer,
setting up the project, and pushing it live.

---

## What you're building

A fast, static content website — the kind that's mostly pages of writing, images,
and links. Astro (the site builder) turns your text files into a website; Cloudflare
hosts it for free; GitHub stores it and triggers a redeploy every time you save.

You write content in plain text files. Claude handles everything else.

---

## Accounts you need

| Account | What it's for | Cost |
|---|---|---|
| **GitHub** | Stores your site's files; every save auto-publishes | Free forever (unlimited private repos) |
| **Cloudflare** | Hosts the live website | Free tier: 100,000 visits/day — far more than you'll need |
| **Claude** | Claude Code, which does the actual building | **Paid** — Claude Pro (~$20/mo) is enough to start |

**Be aware:** Claude Code is the one thing here that isn't free. GitHub and Cloudflare
genuinely cost $0 at this scale, and you never have to enter a credit card for either.

**Optional, later:** a domain name (`yoursite.com`) — about $10/year. You do *not*
need one to start. Cloudflare gives you a free public URL like
`your-site.your-name.workers.dev` that works immediately. Buy the domain once you
actually like what you've built.

---

## Software you need on your computer

You only have to install **one** thing yourself:

- **The Claude desktop app** — https://claude.ai/download

Everything else (Node.js, Git, the Cloudflare tools) gets installed by Claude during
setup, with your approval, step by step. Don't go install them in advance.

**Optional but recommended if terminals make you nervous:**

- **GitHub Desktop** — https://desktop.github.com — a normal app with buttons for
  saving and publishing your changes, instead of typed commands. Claude can drive
  this same process for you, so treat it as a safety net rather than a requirement.

---

## Step 1 — Create your GitHub account (5 minutes)

1. Go to https://github.com/signup
2. Sign up with your email. Pick a username you don't mind being semi-public.
3. Verify your email address.
4. **Create an empty repository:**
   - Click the **+** in the top-right → **New repository**
   - **Repository name:** something short and lowercase, e.g. `my-site`
   - **Private** is fine (you can flip it to public later)
   - Tick **"Add a README file"**
   - Click **Create repository**
5. On the repository page, click the green **Code** button and copy the HTTPS URL.
   It looks like `https://github.com/yourname/my-site.git`.

**Keep that URL handy — you'll paste it into Claude in Step 3.**

That's all you do on GitHub by hand. From here on, Claude drives it.

---

## Step 2 — Install the Claude desktop app (5 minutes)

1. Download from https://claude.ai/download and install it.
2. Sign in. If you don't have a paid plan yet, upgrade to **Claude Pro** — Claude Code
   needs it.
3. In the app, open **Claude Code** (it's built into the desktop app — look for the
   Claude Code section or a "Code" entry in the sidebar).
4. When it asks for a project folder, let it create a new one, or point it at an empty
   folder you make on your Desktop called `my-site`.

If anything in this step is confusing, just ask Claude in the chat:
*"I'm on the Claude desktop app and I want to start a Claude Code session in a new
folder called my-site. Walk me through it."* It will.

---

## Step 3 — Hand Claude the two setup files (20 minutes)

Two files from this kit do the work:

- `CLAUDE.md` — the rules for your project. Claude reads this automatically every
  session, forever. It's what keeps your site consistent as it grows.
- `.claude/skills/project-setup/SKILL.md` — a **one-time setup guide** for Claude. It
  tells Claude how to scaffold the site, connect to Cloudflare, and get you deployed.
  Used once, then retired.

### 3a. Put them in place

Easiest way — just ask. In your Claude Code session, paste this:

> I have two setup files for this project. I'm going to paste them in one at a time.
> Save the first one as `CLAUDE.md` in the project root, and the second as
> `.claude/skills/project-setup/SKILL.md`. Create any folders you need.

Then paste the contents of `CLAUDE.md`, wait for it to save, then paste the contents
of `SKILL.md`.

*(Alternative: drag both files into the Claude desktop app chat window as attachments
and give it the same instruction. Or, if you cloned this kit's repository, they're
already in the right place — skip to 3b.)*

### 3b. Say the magic sentence

Once both files are saved, send this:

> Run the `project-setup` skill. My GitHub repo is
> `https://github.com/yourname/my-site.git`. My site is about **[one sentence about
> what your site is]**. Ask me anything you need and go step by step — I'm not
> comfortable with the terminal, so explain what each step does before you run it.

Claude will then, checking in with you as it goes:

- Install Node.js and Git if you don't have them
- Connect your local folder to your GitHub repo
- Scaffold an Astro site with a proper content structure
- Create a Cloudflare account connection and deploy the site
- Give you a live URL
- Delete the setup skill, since it's done its job

**Expect to approve a handful of commands.** Claude shows you each one before running
it. When you're unsure, ask "what does this do?" — that's a normal thing to do and
costs nothing.

---

## Step 4 — You now have a website. Change something.

From here on, your entire workflow is talking to Claude in plain language:

> Add a page about composting that covers what it is, why it matters, and how to
> start. Then publish it.

> Change the homepage headline to "Practical composting, minus the jargon".

> The text on phones is too small. Fix it site-wide.

Claude edits the files, runs the build to check nothing broke, and pushes to GitHub.
Cloudflare picks it up and your live site updates in a minute or two.

**One habit worth forming:** end requests with "then publish it" when you want it live,
and "don't publish yet, let me look first" when you don't. Claude can run the site
locally so you can preview before anything goes public — just ask it to.

---

## The one rule that matters most

**Don't let the site grow without rules.** The `CLAUDE.md` file is what stops your
project from turning into a mess after fifty pages. As you make decisions — "always
use this tone", "never use green", "every claim needs a source" — tell Claude:

> Remember this: [your rule]

and it will write it into `CLAUDE.md` where every future session will see it. That
file is the difference between a site that stays coherent and one that doesn't.

---

## When something goes wrong

Almost everything is recoverable, because GitHub keeps every version of every file.

- **The site broke** → *"The build is failing. Read the error and fix it."*
- **I hate the last change** → *"Undo the last change and put it back how it was."*
- **I'm lost** → *"Explain in plain English what state this project is in right now
  and what my options are."*
- **A command failed** → paste the error into Claude. That's the fix, in most cases.

---

## Free tier limits, honestly

- **GitHub Free** — unlimited public and private repositories. You will not hit a limit.
- **Cloudflare Workers Free** — 100,000 requests per day. A small content site gets
  nowhere near this. No credit card required.
- **Custom domain** — optional, ~$10/year. Cloudflare sells domains at cost with no
  markup, and if you buy through them the connection to your site is automatic.
- **Claude Pro** — required, ~$20/month, with usage limits that reset every few hours.
  If you hit a limit mid-project, wait it out or upgrade; nothing is lost.

---

## Quick reference

| I want to… | Say this to Claude |
|---|---|
| Add a page | "Add a page about X covering A, B, C" |
| Change wording | "On the X page, change Y to Z" |
| See it before publishing | "Run the site locally so I can look at it" |
| Publish | "Publish this" / "Push it live" |
| Undo | "Undo the last change" |
| Add a rule permanently | "Remember this: [rule]" |
| Add a picture | "Find a free-licensed photo for the X page and add it" |
| Get a real domain | "I bought yoursite.com — connect it to this site" |
