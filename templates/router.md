# The router idea

**Who decides:** a small model (Haiku) reads the task and answers one word: haiku, sonnet or opus. Optionally a human
confirms; in auto mode it just goes on.

**When:** at briefing time for each child (so no cache is lost), and for the master only at session start.

**Table** (starting point, tune it from the log):
| Shape | Model |
|---|---|
| mechanical, exactly describable | haiku |
| generalist, known files | sonnet |
| subtle, unfamiliar, design, failed twice a step down | opus |

**Prompt for the judge (one line):** "Pick the cheapest model that will do this well. Answer haiku, sonnet or opus. Task: <task>"

**Learning log** (`learned.jsonl`, one line per decision): `{"date":..., "task":..., "picked":..., "outcome":"ok|too_weak|too_strong", "better":...}`.
A review now and then (by the biggest model, rarely) turns the log into new lines for the table.

**Dials:** model = knowledge; effort = thoroughness (default medium). Do not change either mid-session.
