# Learning Loop

> *Stub. To be drafted in the v1.0 Design Proposal.*

Not full RL. A **decision log + outcome tracker** that *becomes* training data later.

## Decision points (examples)

- Which mode should auto-activate for this session?
- Which KB chunks are most relevant to this query?
- Should we delegate to a sub-agent? (v2+)
- Which model should handle this kind of query?

## Tables (v1)

- `Decision` — decision_id, session_id, type, context_json, chosen_option, alternatives_json
- `Outcome` — decision_id, signal_type (explicit_feedback | implicit_retry | implicit_success), signal_value
- `Weight` — context_hash, option, weight, updated_at — **v1 stub**; `Weigher.score()` always returns 1.0 in v1

## v1 vs v2

- **v1**: Log every decision + capture outcomes. `Weigher` is a pass-through. We're accumulating data.
- **v2**: Background job consumes Decision+Outcome pairs, updates Weight, and `Weigher.score()` starts actually influencing behavior.
