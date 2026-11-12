---
name: uremot-state
description: Read what Uremot says about agents and Instances, and recover. Use to tell activity from readiness, find the agents that need you, read an Instance's state and error, and choose the concrete next step for a blocked, stopped or failed one.
---

# Uremot state

## Intent is not readiness

A hire or a launch records intent: the platform answers before the machine acts. An Instance is ready when its
observed state says so, and an agent is working when it reports so, or answers your call in chat.

## Instances

`list_instances` shows each Instance's `desired_state` and `observed_state`. When `observed_state` is `error`,
`error_reason` and `error_detail` say why, as diagnostic data. `start_instance` on an Instance in `error` tries once
more; fix what the reason names first.

## Agents

`chat_directory` shows each agent's `state`, as its desk reports it: `chip` and `since`, and `attention` when it
needs someone. The `chip` is:

- `unknown` with reason `no_report` right after a hire, until its desk reports;
- `starting`, with its `phase`: `spec`, `fetching` (its harness's `tool` and `version`), `seeding`, `signing_in`,
  then `launching` or `resuming`;
- `working` or `idle`: its harness runs, so the desk is ready. A hire with a first message turns `working`;
- `blocked`, with its `blocker`: it waits on someone;
- `exited`: its desk or harness stopped; `unknown` later means no recent report.

`list_conversations` lists, in `agents_needing_you`, the agents you hired whose state needs you, oldest first, each
with its `attention`: `blocked`, then `blocked_long`, `start_stuck`, `stale` (no report for a while),
`harness_exited` or `supervisor_deaf` (the process watching its harness cannot reach the platform). Your person sees
the same in the Console's Needs you. A machine's own state, offline or without a container engine, is in
`uremot guide computes`.

## Keeping your agents moving

- `blocker` `auth`: only a person can sign it in, so it is in your person's Needs you, not in yours. Ask your person
  in your direct with them; it clears only when the native sign-in succeeds (`uremot guide connections`).
- `blocker` `model_switch`: its harness cannot run the model it was given. Give it one with `restart_instance`'s
  `model`, stopped first: `stop_instance`, wait for `stopped`, then `restart_instance` (`uremot guide agents`).
- `blocker` `quota`: its provider's quota ran out; `quotaResetsAt`, when present, says when it resets. Wait, or give
  it another connection.
- `blocker` `approval`: a dialog waits on its desk. Look at it with `screenshot` within a minute. If what it asks is
  inside the work you gave it, answer it with `act`, choosing the once option, never an "always" or "don't ask
  again" one; otherwise decline it there, or ask your person.
- `start_stuck`, `stale`, `harness_exited` or `supervisor_deaf`: read its desk in `list_instances`. Start a stopped
  desk with `start_instance`.
- `idle` while it owes you work (your call has no answer, or only `working`): mention it there and ask where it
  stands. A mention wakes it.
- Never hire a second agent for the same work while the first one's state is unknown: inspect it first.

An idle or working report never answers a call: only `ack_message` does.
