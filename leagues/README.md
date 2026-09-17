# League working directories

One directory per league, each acting as its own project root:

```
leagues/
  README.md            <- this file, the only thing here that is committed
  <your-league-slug>/
    leagues.md         <- that league's config, written by fantasy-league-setup:league-config
  <another-league>/
    leagues.md
```

## Why one directory per league

Every skill in this marketplace reads `leagues.md` from the project root and
honors the `(default)` marker when a file defines several leagues. That marker
exists to break ties, and a tie is a chance to guess wrong: ask "who should I
start" from a combined file and the answer silently comes from whichever league
carries `(default)`, which may not be the one you meant.

Splitting the leagues means each file holds exactly one `## League:` section,
which is trivially its own default. Run a skill from inside a league's
directory and there is nothing to disambiguate — the scoring, roster slots, and
waiver rules in scope are the only ones present. Two leagues with different
formats (say a superflex league and an all-skill three-flex league) never
cross-contaminate each other's advice.

The trade-off: a question that genuinely spans leagues ("which of my teams
needs a running back more?") has to be asked with both files in context, or
asked twice. That is the less common case, and it fails loudly rather than
quietly.

## Adding a league

From the repository root:

```bash
mkdir -p leagues/<your-league-slug>
```

Then, working in that directory, say **"set up my league"** and answer the
interview — or copy
[`leagues-template.md`](../plugins/fantasy-league-setup/skills/league-config/leagues-template.md)
to `leagues/<your-league-slug>/leagues.md` and fill it in by hand. Mark the
single section in each file `(default)`.

Anything you cannot confirm goes in as `unknown`, not as a plausible default.
A wrong value here silently biases every recommendation that reads it, while
`unknown` just makes a skill ask you when it matters.

## Nothing in here is committed

`leagues.md` files name your real league, your team, and the other managers in
it. `.gitignore` keeps every path under `leagues/` out of the repository except
this README, so the directories you create stay local to your checkout.

Run `git status` after adding a league — if a league directory shows up as
untracked, the ignore rule is not doing its job and you should fix that before
committing anything.
