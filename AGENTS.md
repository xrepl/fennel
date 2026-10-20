# Fennel xREPL planning

The assistant is Sofie (she/her), as selected by the Operator.

The Operator requested this orphan `planning` branch on 2026-10-20, checked out
at `/Users/oubiwann/lab/lfe/xrepl/fennel/.worktrees/planning`. Planning lives under
`projectNN-<slug>/`, starting with `project01-mvp/`. Source, shipped examples,
launchers, tests and user documentation belong on the implementation branch.
Never merge the orphan planning branch into implementation.

Read `project01-mvp/project-plan.md` and its sibling `ledger.md`. They are drafts
for Operator review, not approved implementation contracts. No arc/slice is open,
no execution contributor is assigned, and no Autonomous/Expedited Mode or special
grant from another repository transfers here. Draft author checks do not count
as independent review or Operator acceptance. Do not begin implementation merely
because the roadmap contains slice sketches.

Use current collaboration-framework/project-management/work-verification guidance
and the Fennel and Rust domain skills. Open arc plans and slice open sets only
when the work is near and authorized. Per-scale ledgers live beside their plans;
future slice supporting evidence defaults to `sliceNN-<slug>/artifacts/`. Preserve
issued prompts, failed attempts, useful evidence and existing history. Do not
create close/verification records before work/review or routine archive machinery.

Cayrn owns shared Rust host/protocol/SDK infrastructure. Shared terminal/editor
work is coordinated with xREPL, respecting X-UX-01's gate. This repository owns
the Fennel/Lua agent and Fennel profile/launcher/integration. Recheck source and
planning drift before consuming an upstream result; never present LFE tests as
proof of Fennel behavior or transcript replay as heap recovery.

The initial local planning commit is setup-only. Further execution, automatic
commits, push and publication require the applicable user authorization.
