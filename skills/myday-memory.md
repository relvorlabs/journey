# name: myday-memory

------------------------------------------------------------------
# description
A persistent, cross-project daily engineering journal for tracking what the
user works on, fixes, builds, investigates, learns, struggles with, notices,
and thinks about throughout each day. Use it to preserve the user's real
technical journey and later generate grounded LinkedIn posts, X/Twitter
threads, Instagram content, retrospectives, and portfolio stories.
------------------------------------------------------------------

# Instruction

All MyDay journals, project memories, state files, and content-bank updates must be stored exclusively in the designated private GitHub repository provided through `MYDAY_REPO_URL`; do not persist the journal inside any active project repository or permanent local directory. Authenticate using a GitHub token supplied securely at runtime through the `GITHUB_TOKEN` environment variable or the coding agent’s encrypted secret manager, never by placing the token in this prompt, a command, a file, a Git remote URL, terminal output, commit, log, journal entry, or repository configuration. The token should be a fine-grained GitHub personal access token restricted to the single private repository with only the minimum Contents permissions required to read and write files. At the beginning of each session, read the latest repository state using the GitHub API or an ephemeral temporary clone, pull or fetch the newest default branch before making changes, locate or create the current date’s journal, and preserve existing entries written by previous sessions or agents. After every meaningful checkpoint, project switch, completed fix, important discovery, end-of-session update, or day closure, update the appropriate journal and supporting files, review the diff to ensure that no credentials, secrets, private customer data, generated dependencies, unrelated files, or fabricated claims are included, create a focused commit using a message such as `myday: update 2026-07-09 journal` or `myday: close 2026-07-09 journal`, and push it to the repository’s default branch unless `MYDAY_BRANCH` specifies another branch. Before pushing, synchronize with the remote again and safely merge or rebase non-conflicting journal updates so work from another agent is not overwritten; if the same journal section has conflicting edits, preserve both factual entries, organise them chronologically, and never resolve the conflict by deleting undocumented work. Never force-push, rewrite shared history, delete previous journals, expose the repository publicly, change repository visibility, or modify repository settings. If authentication, repository access, synchronization, commit, or push fails, retain the prepared update only in an ephemeral working directory for the duration of the active session, report the exact failure without revealing credentials, and do not falsely claim that the journal was saved remotely. Once the remote commit and push are verified, remove any temporary clone, temporary journal files, cached credentials, and generated authentication configuration so no persistent local copy of the private journal remains.

# MyDay Memory Tracker

## Purpose

Maintain a factual, evolving journal of the user's day across every project.

This is not merely a task tracker, changelog, commit summary, or productivity report.

It should capture the full journey:

* What the user intended to accomplish.
* What they actually worked on.
* What they built, changed, fixed, removed, or investigated.
* Why they chose a particular approach.
* How their thinking changed.
* Failed attempts and wrong assumptions.
* Challenges, blockers, and unfinished work.
* Technologies, libraries, tools, APIs, and techniques used.
* New things learned or understood more deeply.
* Mistakes and lessons.
* Interesting internet, technology, startup, AI, developer, and social-media discussions relevant to the day.
* Ideas that may become products, experiments, articles, posts, or future work.
* Authentic moments that describe a day in the user's life as a builder.

The journal will later serve as source material for:

* LinkedIn posts.
* X/Twitter posts and threads.
* Instagram captions or carousels.
* Personal retrospectives.
* Portfolio case studies.
* Build-in-public updates.
* Weekly and monthly reviews.

## Journal Location

Use one central journal directory across all projects.

Preferred directory:

```text
myday/
```

Recommended structure:

```text
myday/
├── YYYY/
│   └── MM/
│       └── YYYY-MM-DD--daily-headline.md
├── projects/
│   └── project-name.md
├── content-bank.md
└── .myday-state.json
```

Do not create a separate isolated daily journal inside every project unless the user explicitly requests that structure.

The purpose is to preserve one unified account of the user's day, even when they move between several repositories.

## Daily Filename

Use:

```text
YYYY-MM-DD--short-descriptive-headline.md
```

Example:

```text
2026-07-08--building-a-cross-project-memory-system.md
```

Filename requirements:

* Use the user's local date.
* Use lowercase kebab case for the headline.
* Do not use colons or characters that are invalid on Windows.
* Keep the headline descriptive but reasonably short.
* The headline may evolve during the day if a more representative theme emerges.
* Rename the file safely when the headline changes and update `.myday-state.json`.

## State File

Maintain:

```text
myday/.myday-state.json
```

Suggested structure:

```json
{
  "active_date": "2026-07-08",
  "active_file": "myday/2026/07/2026-07-08--building-a-cross-project-memory-system.md",
  "status": "open",
  "last_updated": "2026-07-08T19:30:00+03:00",
  "projects_touched": [
    "verilo",
    "leja"
  ]
}
```

This file is operational state, not the main journal.

Never place private reasoning, credentials, tokens, secrets, API keys, or sensitive customer information in it.

## Daily Lifecycle

### 1. Start or Resume

At the beginning of every meaningful coding-agent session:

1. Determine the user's current local date.
2. Read `.myday-state.json` when it exists.
3. Search for an existing journal file for the current date.
4. Resume the current day's journal instead of creating duplicates.
5. If the active journal belongs to a previous date, close that journal first.
6. Create the new day's journal.
7. Record the first known objective or activity.

Never create several daily files simply because the user opens multiple repositories or agent sessions.

### 2. Update Throughout the Day

Update the journal after meaningful events, including:

* Starting work on a new project or problem.
* Understanding an existing implementation.
* Making an architectural decision.
* Implementing a meaningful feature.
* Fixing or identifying a bug.
* Discovering that an assumed approach does not work.
* Changing direction.
* Adding or replacing a technology.
* Running an important experiment.
* Completing a milestone.
* Encountering a blocker.
* Learning something reusable.
* Making a mistake worth remembering.
* Finding an interesting idea, discussion, release, trend, or hype relevant to the user's work.
* Ending a substantial work session.

Do not update the journal after every tiny edit, command, formatting change, or routine tool call.

Group related low-level actions into meaningful entries.

### 3. Close the Day

Close the journal when:

* The user explicitly says the day is finished.
* The user runs a close-day command.
* The agent is invoked on a later date and discovers the previous journal is still open.

Closing the day must:

1. Preserve all existing entries.
2. Mark completed and unfinished work.
3. Summarize the main theme of the day.
4. Record the most important decisions.
5. Extract lessons and mistakes.
6. Identify tomorrow's open loops.
7. Produce grounded content seeds.
8. Mark the journal as closed.
9. Update `.myday-state.json`.

Do not pretend the journal was closed automatically at midnight when the agent was not running.

## Core Behaviour

### Preserve Facts

Record only information supported by:

* The user's instructions.
* The current conversation.
* Files inspected.
* Code changes made.
* Commands and tests actually run.
* Tool results.
* Repository state.
* Confirmed external sources.

Never invent progress, outcomes, metrics, emotions, decisions, or lessons.

Use labels when necessary:

* `Confirmed`
* `Observed`
* `User stated`
* `Inference`
* `Unverified`
* `Still investigating`

### Capture Reasoning Without Exposing Hidden Chain of Thought

Record concise decision rationale, not private internal chain-of-thought.

Good:

> Chose NestJS for the main API because the product requires structured modules, background jobs, API integrations, and shared TypeScript types.

Bad:

> A long stream-of-consciousness transcript of every internal thought the agent had.

Capture:

* The decision.
* Alternatives considered.
* Important trade-offs.
* Evidence used.
* Why the selected option fit the situation.

### Preserve the User's Voice

The journal should sound like a clear record of the user's real day, not corporate status-report language.

Avoid phrases such as:

* Leveraged cutting-edge solutions.
* Seamlessly implemented.
* Revolutionised the workflow.
* Successfully optimised, unless success was measured.
* Exciting journey.
* Game-changing innovation.

Prefer concrete language:

> The SportyBet share-code endpoint returned enough structured data to avoid launching Playwright for this flow.

> I initially assumed the iframe approach would work, but the provider blocks embedding through browser security headers.

### Track Cross-Project Context

Whenever work moves to a different project, record:

* Project name.
* Repository or workspace when known.
* Current branch when relevant.
* Goal.
* Relationship to previous work.
* Whether the switch was planned or caused by a blocker.

Do not flatten several unrelated projects into one vague entry.

### Protect Sensitive Information

Never journal:

* API keys.
* Access tokens.
* Passwords.
* Private keys.
* Session cookies.
* Authentication headers.
* Complete environment files.
* Personal customer information.
* Private conversations unless the user explicitly wants them included.
* Confidential company material.
* Full production database values.

Replace sensitive values with descriptions such as:

```text
[REDACTED_API_KEY]
[PRIVATE_CUSTOMER_DATA_REMOVED]
```

### Avoid Log Dumping

Do not paste huge terminal outputs, stack traces, diffs, source files, or chat transcripts into the journal.

Instead record:

* The important error.
* Its likely cause.
* The relevant file or command.
* The resolution.
* A short excerpt only when necessary.

## Commands

Recognise these natural-language commands and slash-style equivalents.

### Start the Day

```text
/myday start
```

Actions:

* Resume today's journal if one exists.
* Otherwise create it.
* Record the current objective.
* Return the file path and a short status.

### Add a Manual Note

```text
/myday note <content>
```

Actions:

* Add the user's note with a timestamp.
* Preserve the user's meaning and wording.
* Categorise it without over-rewriting it.

### Create a Checkpoint

```text
/myday checkpoint
```

Actions:

* Inspect work performed since the previous checkpoint.
* Add a concise factual entry.
* Include progress, reasoning, problems, decisions, and next action.

### Show Today's State

```text
/myday status
```

Return:

* Current headline.
* Projects touched.
* Current focus.
* Completed items.
* Blockers.
* Open loops.
* Journal path.

### Close the Day

```text
/myday close
```

Actions:

* Complete the end-of-day sections.
* Mark the journal closed.
* Extract content seeds.
* Do not generate polished social posts unless requested.

### Generate Content

```text
/myday content
```

Actions:

* Read the requested journal.
* Generate content using only documented facts.
* Separate content by platform.
* Avoid inventing results or pretending unfinished work succeeded.

Optional variations:

```text
/myday content linkedin
/myday content x
/myday content instagram
/myday content all
```

### Review a Period

```text
/myday review week
/myday review month
/myday review 2026-07-01..2026-07-08
```

Actions:

* Read all journals in the period.
* Identify recurring work, decisions, mistakes, improvements, themes, and content opportunities.
* Distinguish repeated activity from meaningful progress.

## Daily Journal Template

Create each daily file with this structure:

```markdown
---
date: YYYY-MM-DD
timezone: USER_TIMEZONE
status: open
headline: ""
projects_touched: []
started_at: ""
last_updated: ""
closed_at: null
tags: []
---

# Day in the Life — YYYY-MM-DD

## Today's Headline

> A concise, evolving description of the day's main story.

## Starting Point

### What I intended to work on

- 

### Context carried into today

- 

### Questions I started with

- 

## Today's Timeline

### HH:MM — Project or activity

**Objective**

What I was trying to achieve.

**What I found**

Relevant facts discovered from the codebase, documentation, tools, tests, conversations, or external research.

**Approach**

The chosen implementation or investigation path.

**Why this approach**

A concise explanation of the decision and major trade-offs.

**Work completed**

- 

**Files, systems, or areas touched**

- `path/to/file`
- Repository, service, API, database, deployment, design, or document.

**Technologies and tools**

- 

**Challenges**

- 

**Failed attempts or changed assumptions**

- 

**Outcome**

- Completed
- Partially completed
- Blocked
- Abandoned
- Still investigating

**Next action**

- 

## Projects Touched

### Project Name

**Today's goal**

- 

**Progress**

- 

**Current state**

- 

**Open loops**

- 

## Problems, Fixes, and Investigations

### Problem title

**Symptoms**

- 

**Initial assumption**

- 

**Actual cause**

- 

**Attempts**

1. 
2. 

**Resolution or current status**

- 

**Reusable lesson**

- 

## Decisions and Trade-offs

### Decision

**Context**

- 

**Options considered**

- 

**Chosen direction**

- 

**Reason**

- 

**Consequences**

- 

**Revisit when**

- 

## What I Learned

### Technical learning

- 

### Product or business learning

- 

### Workflow learning

- 

### Something I understand differently now

- 

## Mistakes and Corrections

### Mistake or wrong assumption

- 

### Why it happened

- 

### How I corrected it

- 

### How I can avoid repeating it

- 

## Challenges and Friction

- Technical blockers:
- Missing information:
- Tool limitations:
- Context switching:
- Decisions still unresolved:

## Interesting Things I Noticed

Use this section for relevant observations from:

- Developer communities.
- AI and technology releases.
- Startup discussions.
- Product ideas.
- Internet and social-media conversations.
- Industry hype.
- Contrarian opinions.
- Things worth testing rather than immediately believing.

### Observation

**What people are saying**

- 

**My current view**

- 

**Connection to my work**

- 

**Status**

- Interesting
- Worth testing
- Mostly hype
- Useful now
- Revisit later
- Unverified

## Ideas Captured

### Idea

- Problem:
- Possible solution:
- Why it appeared today:
- Related project:
- Next experiment:

## Evidence and Proof of Work

Record useful references without exposing secrets.

- Important commit:
- Pull request:
- Screenshot to capture:
- Demo:
- Test result:
- Benchmark:
- Before-and-after behaviour:
- Relevant file:
- Useful external source:

## Content Bank

Do not force every activity into content. Extract only stories with a useful insight, conflict, lesson, result, or perspective.

### Potential LinkedIn angles

- 

### Potential X/Twitter posts

- 

### Potential thread ideas

- 

### Potential Instagram carousel or caption

- 

### Strong hooks or lines from today

- 

### Useful proof, screenshots, or visuals

- 

## End-of-Day Reflection

Complete this section when closing the day.

### What I actually spent the day doing

- 

### Most meaningful progress

- 

### Hardest part of the day

- 

### Most important decision

- 

### Best lesson

- 

### Mistake worth remembering

- 

### What remains unfinished

- 

### Tomorrow's likely starting point

- 

### The honest story of today

Write a short, factual narrative that connects the day's work, changes in direction, challenges, decisions, and learning.

Do not turn it into motivational content. Preserve the unfinished and imperfect parts.

## Final Day Summary

**Main theme**

- 

**Projects touched**

- 

**Completed**

- 

**Partially completed**

- 

**Blocked**

- 

**Learned**

- 

**Tomorrow**

- 
```

## Entry Quality Standard

Each meaningful entry should answer as many of these as are relevant:

1. What was the user trying to do?
2. Why did it matter?
3. What did the existing system look like?
4. What did the user or agent discover?
5. What approach was selected?
6. Why was it selected?
7. What alternatives were rejected?
8. What changed?
9. What failed?
10. What remains unresolved?
11. What was learned?
12. What could become useful public content?

An entry does not need to answer all twelve questions when they are not relevant.

## Automatic Checkpoint Rules

When the code agent modifies a repository, create a journal checkpoint after:

* Completing a feature or substantial subtask.
* Fixing a meaningful bug.
* Making an architectural decision.
* Changing libraries, infrastructure, APIs, or data models.
* Discovering a blocker.
* Abandoning an approach.
* Running tests that materially change confidence.
* Preparing a commit or pull request.
* Switching projects.
* Ending the working session.

Before writing the checkpoint, inspect available evidence such as:

```text
git status
git diff --stat
git diff
recent commits
tests run
build output
changed files
current task context
```

Do not claim that a test passed unless it was actually run and passed.

## Project Memory

Maintain an optional long-term summary for each recurring project:

```text
myday/projects/project-name.md
```

This is not a duplicate of the daily journal.

Update it only when durable project knowledge changes, such as:

* Product purpose.
* Current architecture.
* Important constraints.
* Naming decisions.
* Major technical choices.
* Repeated blockers.
* Current milestones.
* Long-lived open questions.

Each project file should link back to relevant daily entries.

## Content Bank

Maintain:

```text
myday/content-bank.md
```

Only add content-worthy material such as:

* A useful technical lesson.
* A surprising failure.
* A before-and-after result.
* A decision with meaningful trade-offs.
* A product insight.
* A story with genuine tension.
* A strong opinion supported by experience.
* A recurring theme across several days.

Every content-bank item must reference its source journal.

Example:

```markdown
## Avoiding Playwright when structured booking data already exists

- Source: `2026/07/2026-07-08--testing-sportsbook-import-options.md`
- Projects: Verilo
- Format potential: LinkedIn post, X thread
- Core insight: Browser automation should be a fallback when a stable structured endpoint already provides the required data.
- Evidence needed: Endpoint response shape, latency comparison, maintenance cost comparison.
- Status: Needs more testing
```

## Social Content Generation Rules

When later generating posts from the journal:

* Use only events documented in the journal.
* Preserve uncertainty and unfinished work.
* Do not fabricate user numbers, revenue, benchmarks, customer feedback, or successful launches.
* Do not make ordinary work sound revolutionary.
* Do not remove every struggle from the story.
* Do not expose private repository details or secrets.
* Adapt the same event differently for each platform.

### LinkedIn

Prioritise:

* Context.
* Problem.
* Decision.
* Technical or product insight.
* Honest lesson.
* Practical takeaway.

### X/Twitter

Prioritise:

* One sharp observation.
* Useful technical detail.
* Contrarian lesson.
* Short progress update.
* A thread only when the subject genuinely needs one.

### Instagram

Prioritise:

* A relatable day-in-the-life narrative.
* Visual proof.
* Screenshots, diagrams, workspace moments, or before-and-after states.
* Short lessons suitable for carousel slides.
* Human context without fake inspiration.

## Writing Style

The journal should be:

* Specific.
* Honest.
* Chronological where useful.
* Clear enough to understand months later.
* Technical when technical detail matters.
* Personal without becoming overly intimate.
* Reflective without becoming motivational filler.
* Detailed without becoming a raw activity dump.

Use first-person language when writing the user's journal:

* “I investigated…”
* “I originally assumed…”
* “I changed direction because…”
* “I have not resolved…”
* “The test showed…”

Do not repeatedly call the user “the user” inside the actual journal.

## Final Rule

The journal exists to preserve the real journey, not manufacture an impressive one.

Document the unfinished work, failed experiments, confusion, changed opinions, small wins, difficult decisions, and genuine learning. Those details are what make the later content credible.
