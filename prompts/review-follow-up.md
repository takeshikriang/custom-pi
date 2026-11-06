---
description: Spawn one Herdr tab per code-review point; each runs a forked pi session where the planner agent analyzes the point and rates its severity 1-5
argument-hint: "[session-id-or-path]"
---

Fan out code-review notes into Herdr tabs for parallel analysis. Target session: ${@:-auto-detect}.

## 1. Verify Herdr

Run `test "${HERDR_ENV:-}" = 1`. If it fails, tell the user this prompt must run inside Herdr and stop.

## 2. Locate the review notes

If an argument was given, resolve it as a session path or partial UUID. Otherwise, pick the most recently modified session JSONL in this project's session dir (`~/.pi/agent/sessions/<encoded-cwd>/`) excluding the current session (`$PI_SESSION_FILE`).

Read that session file and find the last substantial review report in it (assistant/reviewer message with distinct findings). Extract each review point verbatim: its title/claim, affected files, and any suggested change. If no notes are found, tell the user and stop.

## 3. Spawn one tab per point

For each point, numbered from 1:

- Name the tab `Review <i>: <point title>` — use the point title as the issue name (strip double quotes, trim to 60 chars).
- If a live agent named `review-<i>` already exists (previous run), use `review-<i>-r2`, `-r3`, … instead.
- Create the tab (never focus it, keep the current cwd):
  `herdr tab create --label "Review <i>: <point title>" --cwd "$PWD" --no-focus`
  Read the root pane ID from `.result.root_pane.pane_id`.
- Start a fresh pi session cloned from this main session:
  `herdr agent start "review-<i>" --kind pi --pane <root-pane-id> --timeout 60000 -- --fork "${PI_SESSION_FILE}" --name "Review <i>: <point title>"`
- If start returns `agent_not_ready`, wait for readiness:
  `herdr agent wait "review-<i>" --until idle --timeout 120000`

## 4. Prompt each forked session

Send exactly this task (adapted per point, kept as one properly quoted argument):

> You are analyzing ONE code-review point. Do NOT modify any files. Use the subagent tool with the planner agent in a single call. Give the planner this point verbatim: "<point text>" plus instruction to analyze it against the codebase and output exactly these four fields: 1) Verdict: valid or invalid, with a one-sentence reason. 2) Effort: S, M, or L. 3) Suggested fix: 1-3 sentences naming concrete files/functions. 4) Severity: 1-5 for importance/relevance (5 = must address now, 4 = important, 3 = worth doing, 2 = marginal, 1 = invalid or ignore).

`herdr agent prompt "review-<i>" "<task>" --wait --timeout 600000`

Run the spawns/prompts for all points; you may create all tabs first, then prompt each.

## 5. Summarize severities

After every prompt settles, read each session's visible output:
`herdr agent read "review-<i>" --source visible --lines 60 --format text`

Extract Verdict and Severity, then print a table in this session:

| Point | Tab | Severity | Verdict |
|---|---|---|---|

Flag any tab that timed out or is still working. Do not focus or close any tabs; results stay in their sessions for the user to review.

Rules:
- Never create, update, read, or print environment variables; reference `${PI_SESSION_FILE}` only inside commands.
- Never commit, stage, or push.
