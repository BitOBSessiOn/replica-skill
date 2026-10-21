# Replica for Hermes

Eleven workflow skills that clone any app, clean-room, plus six
standard-library Python tools. Adapted for [Hermes Agent](https://hermes-agent.nousresearch.com)
as a portable Agent Plugin. No signup, no API key, nothing to connect.

One reverse-engineers the app you want to clone from public sources. One
rebuilds it. One tests it for bugs. And one is the Entrepreneur: it reads what
the app's users hate and fixes it in yours, so you have an app you can sell.

It rebuilds what an app does, never what it owns. Features and flows,
clean-room style. Not its code, its logo, its copy or its content. The fine
print is at the bottom, and the skills enforce it.

## Install

```
hermes plugins install replica
```

From a checkout of this repository instead:

```bash
git clone https://github.com/BitOBSessiOn/replica-skill.git ~/.hermes/plugins/replica
hermes plugins enable replica
```

Skills load on the next Hermes start. The pack is a pipeline: run the skills
in order, and each one reads what the last one wrote from a `replica/` folder
in your project.

## The eleven

| Skill | What it does |
| --- | --- |
| `replica-recon` | Reverse-engineers any app's product surface: screens, flows, components, inferred data model. From public pages, screenshots, store listings and your own account. |
| `replica-architect` | Plans the stack, database schema and API for your clone. |
| `replica-design` | Rebuilds the design system as tokens: colours, type, spacing, components. Your own assets. |
| `replica-build` | Rebuilds the app screen by screen from the recon map. |
| `replica-backend` | Auth, database, payments and integrations. |
| `replica-test` | Writes the test plan from the flows, clicks through every one, logs bugs by severity. |
| `replica-diff` | Scores your clone against the original: feature parity plus a layout diff that ignores colour, so the rebrand does not count against you. |
| `replica-entrepreneur` | Reads real user reviews, ranks what they hate with counts and linked quotes, turns it into fixes and a positioning angle. |
| `replica-brand` | Names and rebrands your version, then sweeps the codebase until nothing of the original is left. |
| `replica-launch` | Landing page, pricing and store listing. |
| `replica-deploy` | Preflight, production setup, DNS records, ships when you say go. |

Pipeline: `recon -> architect -> design -> build -> backend -> test -> diff ->
entrepreneur -> brand -> launch -> deploy`. Any skill also works on its own.

## The tools

Six scripts, all Python standard library, none touch the network:

```bash
python3 skills/replica-diff/imgdiff.py original.png clone.png --out diff.png
python3 skills/replica-diff/parity.py replica/features.csv
python3 skills/replica-entrepreneur/reviews.py replica/reviews.csv
python3 skills/replica-design/contrast.py replica/design/tokens.json
python3 skills/replica-brand/sweep.py . --avoid "Original App"
python3 skills/replica-launch/listing.py replica/launch/listing.json
```

**`imgdiff.py`** reads PNGs with no libraries, turns both screenshots into
edge maps and compares where things are, so your new colours do not count
against you. **`parity.py`** weights must, should and could, counts partial as
half, and never scores what you left out on purpose or what you added.
**`reviews.py`** drops every review without a link: every quote it prints came
from a row you gave it, with that row's URL. **`sweep.py`** finds the
original's name, domain and colours anywhere in your code and blocks the
deploy until it is clean. **`listing.py`** checks store limits and copycat
risks. **`contrast.py`** runs WCAG checks on your design tokens.

Tests: `python3 -m unittest discover -s tests -v`.

## What this fork changes for Hermes

- Skills live under `skills/` and the plugin is manifest-driven
  (`plugin.json`), so Hermes installs it by name like any catalog plugin.
- `replica-recon` triggers are anchored to rebuild intent ("clone this app",
  "build my own version of X"). Generic "reverse engineer X" requests stay
  with REA, which covers binaries, packages and runtime evidence. Replica
  only reads public surfaces and never decompiles or instruments anything.
- README and install path target Hermes. The Claude Code layout stays
  available through `.claude-plugin/` for anyone arriving from the original.

## Clean-room rules, enforced by the skills

- Public sources and your own account only. Never a login that is not yours,
  never past a paywall, never private APIs or scraped JavaScript bundles.
- Features and flows only. Not the content, the network or the licences an
  app owns. You can rebuild a music app's player, playlists and sharing. You
  cannot clone its catalogue.
- Always rebrand before launch. `replica-deploy` will not ship until the
  sweep is clean.
- The Entrepreneur never invents reviews, quotes or numbers. Every claim has
  a link and the sample size is stated.

Check before you sell: run the trademark checks `replica-brand` lists, read
the terms of anything you used, and talk to a lawyer if there is money on the
line. This is not legal advice.

## Disclosure

- The six Python tools are standard library only and make no network calls.
- The plugin registers no agent tools, no hooks, no middleware, and reads no
  environment variables.
- The skills instruct the agent to read public web pages of the app you want
  to clone (help centers, pricing pages, store listings) and, if you choose,
  your own logged-in account through your existing browser tools. They never
  instruct login to third-party accounts, scraping behind auth, decompilation
  or traffic interception.
- Working files (`replica/` with recon maps, feature matrices and reference
  screenshots) are written inside your project directory, never uploaded
  anywhere.

## Credit

Original project: [The Replica skill](https://github.com/Jakeschincariol/replica-skill)
by Jake Schincariol, [opusjake.ai](https://opusjake.ai), MIT. This fork
restructures it as a Hermes Agent plugin and anchors the recon triggers; the
method, the tools and the clean-room rules are the original author's work.
Siblings by the original author: [Arena](https://github.com/Jakeschincariol/arena-skill),
[X](https://github.com/Jakeschincariol/x-agent-skill),
[LinkedIn](https://github.com/Jakeschincariol/linkedin-agent-skill),
[Instagram](https://github.com/Jakeschincariol/instagram-agent-skill).

## Fine print

It rebuilds functionality and UX patterns, clean-room style. It studies what
the app does and how people move through it, then writes everything fresh. It
never copies the target's source code, proprietary assets, logos, trademarks,
copy or private APIs.

It only reads what you are allowed to read. Public pages and your own account.
It never scrapes behind a login or against a site's terms, never logs into
anyone else's account, and never gets past a paywall. If your account's terms
forbid using it to build a competitor, it says so and sticks to public
sources.

No guarantee of a "perfect" clone. Results depend on how complex the app is. A
booking tool is weeks. A spreadsheet engine is not. `replica-recon` sizes it
honestly before you start, and `replica-diff` gives you a real number, not a
vibe.

License: MIT. Take it, change it, ship it.
