# Claude Code Mantra Operating System (CCMOS)

A way of working with Claude Code for someone who runs many projects at once and wants little token use and speed:
**one master session per main project, many small child agents with fresh context, a router that picks the cheapest
model that can do each task, and every repeated procedure written down as a skill.** MIT licensed, plain Markdown, no
code to install: copy the templates.

## The idea in six lines

1. The session you talk to is the **master** of one main project. Its context stays lean; it decides and delegates.
2. Each application or aspect (a DAW, a video editor, a website, a phone, a download, a research dig) is a **child agent**
   with a fresh context, briefed in a fixed shape, reporting in ten lines.
3. Independent children run **in parallel**, started in one message. Two children never own the same app or repo.
4. A **router** (a small model judging the task) picks Haiku, Sonnet or Opus per child; the big model only when needed.
5. **Cache discipline**: stable prefix, model and effort fixed at the start, `/clear` between tasks, skills instead of
   long CLAUDE.md, quiet command output, noisy digging in a child.
6. A procedure done twice becomes a **skill** (or a script) the same day; the skill is the source, corrections are
   written back at the close.

Read [`CCMOS.md`](CCMOS.md) for the full description, [`TAKEOVER.md`](TAKEOVER.md) to continue the work,
[`LESSONS.md`](LESSONS.md) for what is learned.

## Templates

| File | Use |
|---|---|
| [`templates/master-CLAUDE.md`](templates/master-CLAUDE.md) | the master's CLAUDE.md: lean, the rules that apply to every task |
| [`templates/child-brief.md`](templates/child-brief.md) | the brief a master gives a child |
| [`templates/SKILL.md`](templates/SKILL.md) | a skill for a repeated workflow |
| [`templates/router.md`](templates/router.md) | the router idea: who picks the model, the table, the learning log |

## Origin

Written 8.11.2026 from a Claude Code team webinar on how tokens are spent (input, output, cached; the prompt-cache prefix;
model and effort as multipliers; subagents as a way to keep the main context small) and from a year of daily
multi-project use. Licence: MIT.
