# CCMOS in full

## What a request costs (the mental model)

Every request sends the system prompt, the tool definitions, CLAUDE.md, the skills' names and descriptions, and the whole
conversation so far. A prompt is many requests: the model asks for a tool, the harness runs it, the result is sent back.
Tokens come in three prices: input, output (the most expensive, written one by one) and cached (a small fraction).
The server reuses only an **identical prefix** from the start of the request; one changed token and everything after it is
processed at full price. Therefore:

- Changing the model or (on most models) the effort breaks the cache; `/clear` and `/compact` change the prefix too.
  Editing files or the permission mode does not. CLAUDE.md is read once at the start.
- The cache lives about an hour on a subscription plan, about five minutes on API billing, five minutes for subagents.
  Compact or switch before stepping away, while it is warm. A resumed old session pays one full read.
- Context only grows. A long session carries task one through task three on every request.
- Command output stays in the context for the rest of the session; a test runner printing 400 lines taxes every later request.

## Master and children

| | Master | Child |
|---|---|---|
| Count | one per main project | many, parallel |
| Context | lean, long-lived: project CLAUDE.md, state, decisions | fresh: only its brief; ends with its report |
| Does | understands, splits, briefs, collects, talks to the human | one application or aspect |
| Never | digs, reads many files, runs noisy commands | talks to the human directly |

Rules: split **by application** (each tool that owns state has one child); start independent children in one message;
do small things yourself (a child costs a brief and a report); the master reads reports, not transcripts.

## The brief

Stable text first (cache), in this order: goal (one sentence, finished state); the skill to read first; files to read and
files it may touch; the hard rules that bite here; the report format (10 lines, paths, hashes, no logs); what to do when
a human hand is needed (one message to the master naming the hand, then continue); the close (write the correction back
into the skill, push what it owns). See `templates/child-brief.md`.

## The router

A small model (Haiku) judges the task and names the model. In CCMOS this happens **per child at briefing time**, so the
model is chosen before the child's first token and no cache is lost.

| Task shape | Model |
|---|---|
| Mechanical and exactly describable (rename, grep, poll, summarise a log, format) | Haiku |
| Generalist work with known files (a feature, a page, a script from a spec, most checks) | Sonnet |
| Subtle bug, unfamiliar ground, system design, an unanchored review, anything that failed twice a step down | Opus |

A child that fails on a cheaper model is repeated one step up and the miss is logged so the router learns. Effort is a
second dial: if the result is wrong because it did not know, raise the model; if it skipped a step, raise the effort.
Default medium. See `templates/router.md`.

## Skills instead of repeating

CLAUDE.md keeps only what applies to every task. A procedure for one kind of work is a skill: only its name and description
are always loaded, the body loads when the task matches. When a workflow is done twice by hand, write the skill the same
day, register it in an index (`SKILLS.md` in your own manifest), and let the child that uses it write corrections back.
When the procedure is deterministic, make it a script and let the skill say "run this".

## Keep the context quiet

Write the two or three commands used all day into CLAUDE.md with their quiet flags. Point at files with `@path`. Remove
instructions that newer models no longer need. Switch off MCP servers a session does not use, or use a CLI. Send noisy
digging to a child.

## One task, one session

Pick model, effort and tools at the start and leave them alone. `/clear` between tasks (rename the old session first if you
may come back). Compact before a break, never after it. Close each child with its report; close the master by writing the
project's state to a file in the repository, so the next session starts from it.

## Hub

Use a git repository and a cloud drive as the message board between machines and sessions; move files server-side, not
through the laptop; keep large binaries out of git.
