WEBSITE BUILDER STARTERKIT
==========================

Build and run a static content website by talking to Claude Code. No coding,
almost no terminal. About 30 minutes from nothing to a live URL.

If you only read one other file, read SETUP.md. It is the step-by-step guide.


WHAT YOU'RE BUILDING
--------------------

A fast, static content website -- the kind that's mostly pages of writing,
images, and links.

  Astro       the site builder; turns your text files into a website
  GitHub      stores the files; every save triggers a republish
  Cloudflare  hosts the live site

You write content in plain text files. Claude handles everything else.


WHAT IT COSTS
-------------

  GitHub       Free forever. Unlimited private repositories.
  Cloudflare   Free. 100,000 visits/day, no credit card required.
  Claude Pro   PAID, about $20/month. This is the only cost.
  Domain name  Optional, about $10/year. Not needed to start -- Cloudflare
               gives you a free URL like your-site.your-name.workers.dev
               that works immediately.

Be aware: Claude Code is the one thing here that isn't free. GitHub and
Cloudflare genuinely cost $0 at this scale.


WHAT YOU INSTALL YOURSELF
-------------------------

Just one thing:

  Claude desktop app        https://claude.ai/download

Node.js, Git, and the Cloudflare tools get installed by Claude during setup,
with your approval, step by step. Don't install them in advance.

Optional, if terminals make you nervous:

  GitHub Desktop            https://desktop.github.com

A normal app with buttons instead of typed commands. A safety net, not a
requirement -- Claude can drive the same process for you.


THE FILES IN THIS KIT
---------------------

  SETUP.md      The 30-minute walkthrough. Start here.
  README.md     Overview of the kit and what it produces.
  README.txt    This file.
  CLAUDE.md     The rules for your project. Claude reads this every session,
                forever. This is the important one -- it's what keeps the site
                from turning into a mess after fifty pages.
  LICENSE       Apache License 2.0.

  .claude/skills/project-setup/SKILL.md
                One-time setup instructions for Claude. It deletes itself once
                setup is finished.


THE THREE STEPS
---------------

  1. Create a GitHub account and an empty repository.   (5 min)
  2. Install the Claude desktop app and open Claude Code. (5 min)
  3. Give Claude CLAUDE.md and the setup skill, then say:

       Run the `project-setup` skill. My GitHub repo is
       https://github.com/yourname/my-site.git. My site is about [one
       sentence]. Ask me anything you need and go step by step -- I'm not
       comfortable with the terminal, so explain what each step does before
       you run it.

Full detail, including exactly what to click on GitHub, is in SETUP.md.


EVERYDAY USE
------------

After setup, your whole workflow is talking to Claude in plain language:

  Add a page          "Add a page about X covering A, B, C"
  Change wording      "On the X page, change Y to Z"
  Preview first       "Run the site locally so I can look at it"
  Publish             "Publish this" / "Push it live"
  Undo                "Undo the last change"
  Save a rule         "Remember this: [rule]"
  Add a picture       "Find a free-licensed photo for the X page and add it"
  Connect a domain    "I bought yoursite.com -- connect it to this site"

Claude edits the files, builds the site to check nothing broke, and pushes.
Your live site updates a minute or two later.

Habit worth forming: end requests with "then publish it" when you want it
live, and "don't publish yet, let me look first" when you don't.


WHEN SOMETHING GOES WRONG
-------------------------

Almost everything is recoverable, because GitHub keeps every version of every
file.

  The site broke        "The build is failing. Read the error and fix it."
  I hate that change    "Undo the last change and put it back how it was."
  I'm lost              "Explain in plain English what state this project is
                         in right now and what my options are."
  A command failed      Paste the error into Claude. That's usually the fix.


LICENSE
-------

Apache License 2.0. See the LICENSE file. The sites you build with this kit
are yours.
