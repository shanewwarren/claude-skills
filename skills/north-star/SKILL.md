---
name: north-star
description: Print one screen for the current project, with the vision, what's built and open against each goal, and the drift.
disable-model-invocation: true
license: Apache-2.0
---

# North star

Print one screen that answers "where are we against the vision?" for the project in the working directory. A reader who lost the thread five sessions ago finishes it knowing the goals, what serves each one, and what has wandered off.

Every line is a claim you checked against its source. A claim you can't check stays off the screen.

## 1. Find the vision doc

The vision doc says who the product is for, what they get, and how success is judged, ideally with dates. Take the first of these that exists:

1. The docs `CLAUDE.md` or `AGENTS.md` point to for the vision, the bet, the goals, the scope or the milestones. Several docs can split the job, one holding the bet and another the milestones. Read every one of them.
2. A file named like `NORTH_STAR.md`, `VISION.md`, `BET.md`, `PRODUCT.md` or `ROADMAP.md`, at the root or under `docs/`.

A README that only describes what the code does is not a vision doc. With neither, run [INTERVIEW.md](INTERVIEW.md), then continue here.

Done when you can name the vision doc paths and quote the sentence that states the north star.

## 2. List the goals

Goals are the dated or measurable outcomes the vision doc names: milestones, success signals, secondary targets. Use the doc's own ids (`M1`), or `G1`, `G2` when it has none. A success signal that a milestone already proves folds into that milestone. For each goal, record its id, a short name, its due date and the scope it lists.

Done when each goal traces to a line in the doc.

## 3. Gather the work

Run `git fetch --quiet` first so the default branch matches the remote.

- **Built.** `git log --first-parent --format='%h %as %s' -n 300` on the default branch. Squash and merge titles carry the PR number. Past 300 commits, say so in the sources line. The commits that wrote the vision docs are the vision, not built work.
- **Open.** Open PRs from `gh pr list --state open`. Open tracker issues: find the tracker from issue ids in commit titles and branch names (`ENG-65`, `#123`) and from tracker names in `CLAUDE.md`, `AGENTS.md` and the docs. Query it through its MCP server or CLI, scoped to this project. Every state other than done or canceled counts as open.
- **Planned.** Scope a goal lists that has no built work and no open PR or ticket. Unchecked `- [ ]` items in the vision doc and the docs it links are planned too, and go on the Checklist row. A checkbox is a plan, not motion.

A source you can't reach (no `gh` auth, no tracker MCP) is a gap. Name it in the sources line.

Docs go stale faster than git and the tracker. When a doc's status line or checklist disagrees with the record (it says "not built" and a merged PR built it), the record wins. Name the stale doc in the sources line.

Done when every commit in the range and every open item has been read.

## 4. Tie each item to a goal

Tie work to the goal whose text it advances: a ticket that names the milestone, a PR that builds what the goal lists. Enabling work (contracts, fences, CI, docs) ties to the goal it unblocks. Work that ties to no goal's text is **drift**. A tie that needs stretching is drift too.

Give each goal one status:

- **done** when its scope is built or its signal is met.
- **moving** when it has an open PR or a ticket someone has committed to (todo, in progress, in review). A backlog ticket is open but parked. Mark it `(backlog)` in the Open row.
- **idle** when planned scope remains and nothing is moving it. An idle goal is drift.

Done when every item has exactly one goal or sits in drift.

## 5. Print the screen

Print markdown, outside any code block, so the terminal renders the headings, bold and lists. Keep it to one screen: one heading per goal and at most four one-sentence bullets under it.

```markdown
# <Project>

<The north star in one or two sentences.>

**Now:** <What is in flight, the nearest goal, and what stands between it and done. Two sentences at most.>

### <id> · <name> · <Mon D> · <n> days · <done | moving | idle>
- **Built:** <features, named the way a user would name them>
- **Open:** <open PRs and tickets, each with its id>
- **Left:** <scope with nothing built or open>
- **Checklist:** <count of unchecked items, then the soonest and its date> (<file>)

### Drift
- <idle goals, soonest due first>
- <work that serves no goal>

*Sources: <docs read> · <n> commits since <date> · <tracker>, <open> of <total> open · <gaps and stale docs>*
```

- Order goals by due date and count days from today.
- Ids appear only on open items (`#43`, `ENG-66`), where the reader acts on them. The commits behind a built feature stay in your check.
- Drop a bullet that has nothing to report, rather than writing one that says so.

Before printing, check every claim against its source. Each built feature traces to the commits that built it, each open id exists in the state you claim, each date and goal name matches the doc, and each tie traces to the goal's text. Fix or cut what fails, then print the screen and nothing else.
