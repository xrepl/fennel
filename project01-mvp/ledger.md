# Project01 MVP — draft project ledger

Version 0.1, 2026-10-20. Proposed criteria for [project-plan.md](project-plan.md).
All rows are open, with no implementation or independent-verification evidence.
Verification routes below are proposed observable demonstrations; executable
commands and test names are set in future approved slice open sets.

| ID | Criterion | Verify | Significance | Origin | Status | Evidence | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- |
| FMVP-01 | Arc01 delivers an accepted real persistent Fennel runtime | Arc01 child returns plus real local/function/macro persistence, detach/reattach and isolated-peer demonstration | correctness-grade | Plan Arc01 | open | — | LFE proof does not establish Fennel conformance |
| FMVP-02 | Actual Fennel agent obeys the bounded native contract | Real handshake/fencing, malformed frame, output ACK, close barrier, busy/cancel/loss and long-session checks | serious | Plan architecture and Arc01 | open | — | Advertise only demonstrated control strength |
| FMVP-03 | Submitted source and result semantics survive the whole path | Multiform/whitespace/comma-command cases, nil/false/trailing nil and error recovery tests | correctness-grade | Plan Arc01/03 and language constraints | open | — | Result callback alone is not operation completion |
| FMVP-04 | Arc02 editing/history increments compose | PTY Emacs keys, arrow recall, reverse search, history hints, restart persistence and simultaneous-attachment cases | correctness-grade | Plan Arc02/S01 | open | — | Private local history and durable transcript have separate policies |
| FMVP-05 | Live lexical highlighting and multiline behavior work | PTY and lexer/validator cases for partial/invalid input, strings/comments/delimiters, paste, Unicode and cancelled draft | correctness-grade | Plan Arc02/S02 | open | — | No code evaluation or macro expansion while editing |
| FMVP-06 | Initial completion is useful and source-correct | Static forms/comma commands and menu tests proving correct replacement spans | correctness-grade | Plan Arc02/S03 | open | — | Do not claim live bindings before Arc03 |
| FMVP-07 | Session-aware completion uses an accepted bounded query contract | Actual-runtime live binding/macro/member cases, generation/buffer staleness, busy/timeout and no-side-effect negative controls | correctness-grade | Plan Arc03/S01 | open | — | Current Cayrn RPC has no completion request |
| FMVP-08 | Supported Fennel development commands behave as documented | Help/doc/reload/result variables/module-loading cases and terminal/Fennel command-collision controls | correctness-grade | Plan Arc03/S02 | open | — | Unsupported program input stays explicit with its re-entry owner |
| FMVP-09 | Installation and daily launch are reproducible | Clean-directory/install run without development sibling paths; compiler/VM/library/readiness checks | correctness-grade | Plan Arc03/S03 | open | — | Publication remains separate |
| FMVP-10 | Shared changes preserve supported existing consumers | Matching LFE/fixture/shared SDK regressions and actual dependency-pin verification | serious | Plan ownership and coordination | open | — | No copied terminal or second session authority |
| FMVP-11 | Complete system preserves output and terminal usability | Mixed sustained session, large previews/complete output, lagged observation, draft-preservation and terminal-restoration scenarios | serious | Plan project composition | open | — | Child passes alone do not prove composition |
| FMVP-12 | Arcs and findings are reconciled against the project DoD | Arc closing/bubble-up row walks, scope comparison and every held/deferred item dispositioned with owner/re-entry | correctness-grade | Plan validation and deferred scope | open | — | No silent removal of Arc03 after an Arc02 milestone |
| FMVP-13 | Independent and Operator project gates are satisfied | Actual independent composition review and separately recorded Operator acceptance of the same candidate | serious | Plan workflow and completion | open | — | Draft/setup self-checks are not independent acceptance |

No row is closed by creating this ledger. No closing or verification files exist
until the corresponding work and review actually occur.
