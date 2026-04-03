# Squid-Master — Session State

> Primary source of truth for session continuity. Written by Squid-Master at session end
> and on [SV save-session]. RAG agent_sessions is the secondary sync layer.
> If sidecar and RAG disagree, sidecar is always authoritative.

```yaml
schema_version: "1.0.0"

session_id: null
branch: null
workflow_state: null
track: null
workflow_step: null
execution_mode: null

linked_feedback_ids: []
linked_roadmap_ids: []

sprint_plan_paths: []
sprint_start_date: null
sprint_end_date: null

blocked: false
blocked_reason: null
blocked_since: null

pmr_gate_status:
  small: null
  medium_pre: null
  medium_post: null
  large_planning: null
  large_design: null
  large_per_epic: {}

ar_loop_counts: {}

qa_environment:
  branch_verified: false
  containers_rebuilt: false
  lock_file: null
  lock_acquired_at: null

claude_md_hash: null
docs_hashes: {}

pending_rag_syncs: []

last_updated: null
```
