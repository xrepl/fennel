---
project: project01-mvp
status: draft-for-operator-review
depends-on: [cayrn-bounded-native-contract, xrepl-shared-terminal-ux]
blocks: [fennel-xrepl-mvp-acceptance]
related: [cayrn-project02-core-hardening, xrepl-project10-shared-repl-architecture, xrepl-X-UX-01, cayrn-C-ADAPTER-01]
---

# Project01 — Fennel xREPL MVP

Version 0.1, 2026-10-20. Draft by Sofie for Operator discussion. This records
the conversation's proposed plan in the newly created `xrepl/fennel` repository,
with a collaboration-framework, Fennel and Rust guidance refresh. No project
scope, architecture choice, implementation assignment or upstream schedule is
approved by saving this draft.

## Intended result and first useful increment

Deliver a local Fennel terminal REPL built on Cayrn and the shared Rust xREPL
terminal. The main user benefit is Reedline's everyday editing experience:
Emacs bindings, recall and searchable persistent history, inline history hints,
live syntax highlighting, multiline editing and tab completion.

The first useful increment runs a real persistent Fennel session through the
existing Cayrn host and xREPL terminal. Define a local and a function, detach,
reattach, and use both again; an independent session remains isolated. It ships
with usable local entrypoints, docs and actual runtime tests. It is an early
development capability, not the completed editing milestone or a distributable
release. The main editing milestone follows in Arc02.

Fennel evaluation stays in a thin Fennel/Lua runtime agent. Shared terminal code
is reused rather than copied into a separate frontend. Source, launchers, tests
and user documentation belong on this repository's implementation branch;
shared terminal improvements belong with their xREPL owner, and shared host,
protocol and SDK changes belong in Cayrn.

## Current status, workflow and planning home

The Operator authorized the usual orphan `planning` branch/worktree and saving
this draft for review on 2026-10-20. The home is
`.worktrees/planning/project01-mvp/`; supporting evidence produced by a future
slice defaults to that slice's `artifacts/` directory. Never merge the orphan
planning branch into the implementation branch.

All arcs are proposed and none is active. Slice sketches below are roadmap
decomposition, not open sets or implementation-ready assignments. No arc/slice
directories, prompts, closing reports or verification verdicts are opened now.
The sibling [ledger](ledger.md) is also draft; every row is open.

This drafting/setup work is performed by Sofie in the current context, with
ordinary self-checks. It supplies no independent implementation acceptance.
Future implementation defaults to the framework's Two-Contributor Workflow
(CDC planning/review and CC implementation), subject to Operator selection;
no contributors are assigned or dispatched now. No Autonomous/Expedited Mode or
special workflow grant transfers from Cayrn, xREPL Project09 or Project10.
Resolve implementation staffing, source branch and consequential held decisions
before opening the first slice. Save later substantive plan changes in dated
version-history entries without rewriting issued prompts or historical evidence.

## Observed baseline and evidence limits

These are checked-out inputs inspected on 2026-10-20, not claims about the newest
remote state. Recheck drift before any assignment or dependency acceptance.

| Component | Observed source | Observed planning | Relevant state |
| --- | --- | --- | --- |
| `xrepl/fennel` | `main`, `8863c5a` | New orphan `planning` worktree | Initial LICENSE and .gitignore only; no implementation |
| LFE xREPL | `release/0.2.x`, `cf1f1a2` | `planning`, `eedd817` | Cayrn terminal consumer and native LFE agent exist; core migration remains incomplete |
| Cayrn | `codex/core-session-lifecycle`, `4aea43a` | `planning`, `68b557a` | Native4 contract and sustained-use slice results exist; formal arc composition gates remain pending |

The current xREPL workspace uses Reedline 0.38 and Rust 1.95. Its editor
initialization is essentially `Reedline::create()` with a default prompt.
Persistent history, hints, completion, highlighting and a language-aware
validator are not wired there. Do not infer those features from older LFE shell
documentation, the Reedline dependency alone or historical Rust frontend plans.

The current Cayrn native wire is version 4, using bounded length-prefixed JSON;
client RPC is independently versioned. It is not the historical Postcard
proposal or the legacy xREPL MessagePack transport. Native4 already requires
authority/context fencing, explicit outcome-preview metadata, output ACKs and
honest close/control behavior. A Fennel agent must prove these independently;
accepted LFE results are precedent, not Fennel conformance.

## Boundaries and proposed architecture

| Home | Owns |
| --- | --- |
| Cayrn | Shared host, admission and identities, lifecycle, journal/transcript, protocol and client SDK |
| Shared xREPL terminal | Reedline editor configuration, history/hints, menus, terminal presentation and language-profile interfaces |
| `xrepl/fennel` | Fennel/Lua runtime agent, Fennel editing profile, launcher/configuration, integration tests and Fennel user docs |

The runtime environment remains opaque to Cayrn. No second durable session
authority is added to the native agent. Reattachment to a living environment
and replay of a durable transcript are different capabilities; history never
implies heap recovery or automatic reexecution of code.

Use a persistent `fennel.repl` with configurable input/result/error hooks rather
than unrelated `fennel.eval` calls per submission. Preserve REPL locals and
compiler/macro scope, multiple results and normal development commands. Define
one-buffer/one-operation completion explicitly, including multiple forms and
return to the input boundary; an individual `onValues` callback is not by itself
proof that the entire submission completed.

The agent must separate protocol transport from evaluated program I/O.
Replacing REPL callbacks does not automatically capture `print`, `io.write`,
`io.read` or explicit native devices. Declare supported capture, handle output
as ordered bounded chunks, ACK only committed output, and distinguish bounded
value previews from complete supported streams. Unsupported runtime input fails
explicitly until a correlated-input implementation exists.

The control owner must remain responsive during nonterminating evaluation and
output pressure. Per-session worker processes are the preferred direction for
discussion, not a selected implementation. Resolve supervisor placement, IPC,
Lua libraries, teardown and cancellation strength before issuing dependent work.
Advertise only demonstrated capabilities. Destructive interruption must report
environment loss, and one session's cancellation must leave peers usable.

No new language-specific Rust host is proposed. Whether shared terminal reuse
needs a crate extraction from `xrepl-core`, and where that package should live,
remain shared xREPL design decisions. No repository move or extraction is
authorized by this document.

## What Reedline supplies and what needs integration

| User experience | Reedline machinery | Work needed |
| --- | --- | --- |
| Emacs editing and recall | Editing commands, key bindings, history navigation | Shared defaults and terminal checks |
| Persistent history and reverse search | History backend and search interaction | Private path, save/merge policy, bindings and configuration |
| Inline suggestions | History-based hinter | Enable and style; semantic prediction is outside scope |
| Completion menu | Completer/menu interfaces | Candidate source, replacement spans and bindings |
| Live highlighting | Highlighter interface | Fennel lexer and standard-form classification |
| Multiline input | Editable buffers and validator interface | Fennel completeness rules and continuation behavior |
| Fennel prompt | Prompt interface | Shared configuration and language identity |

Fennel-aware behavior is not obtained merely by enabling Reedline. Use
`fennel.syntax()` as classification data alongside a lexer, not as a tokenizer.
Editing analysis must tolerate incomplete source without evaluating it or
expanding user macros. Lexical highlighting is the MVP; fully scope-aware
semantic highlighting is not promised.

## Cross-repository coordination

Existing future arcs are roadmaps; the UX slice labels below are **proposed**
delivery requirements, not existing xREPL assignments or reserved project IDs.

| Requirement | Existing owner / planning anchor | Proposed bounded delivery and Fennel consumer |
| --- | --- | --- |
| Reusable terminal configuration/profile | xREPL X-UX-01 | UX-A01/S01 configurable editor; Fennel Arc02 consumes the accepted reusable interface |
| History/search/hints and bindings | xREPL X-UX-01 | UX-A01/S02 everyday editing/history; Fennel Arc02/S01 supplies its namespace/configuration |
| Highlighting/completeness hooks | xREPL X-UX-01 | UX-A02/S01 language editing interface; Fennel Arc02/S02 implements and tests its profile, with an LFE consumer/regression control |
| Completion/menu integration | xREPL X-UX-01 | UX-A02/S02 completion integration; Fennel Arc02/S03 supplies static candidates |
| Live-session language queries | Cayrn shared protocol/SDK and each runtime project | New bounded language-service slice needs an agreed upstream owner/placement; Fennel Arc03/S01 implements its counterpart |
| Catalog, names, metadata | Cayrn Project02/Arc02; xREPL Project10/Arc03 | Consume accepted administration APIs when needed; initial explicit-ID attach does not require names/catalog |
| Correlated program input | Cayrn Project02/Arc03; xREPL Project10/Arc04 | Fennel counterpart follows accepted ownership/cancellation/EOF/secret-data contract |
| Multiline/multiform payload | xREPL Project10/Arc04 for LFE; Fennel agent for its semantics | Shared terminal must preserve source bytes/whitespace; each runtime establishes its own submission boundary |
| Startup/readiness/package contracts | Cayrn Project02/Arc04–05; xREPL Project10/Arc06 | Fennel Arc03/S03 consumes the bounded operational and distribution results it actually needs |

X-UX-01 currently re-enters after core contracts/composition are accepted and the
Operator opens the UX project. This draft does not change that gate. An earlier
shared UX start needs an explicit scheduling decision. Planning Fennel's consumer
requirements is not permission to implement that deferred upstream work.

Fennel Arc01 can use bounded existing Cayrn interfaces without waiting for all
of Project10. LFE helpers, story/graphics support, legacy client conversion and
snapshot migration are not prerequisites. Likewise static completion and lexical
highlighting do not wait for live language-service RPCs. No whole-project
dependency cycle should be created in place of these bounded inputs.

## Arc roadmap and candidate slice sequence

All three arcs are roadmap-only. Keep checks/docs/entrypoints with each usable
increment; detailed sizing and open sets follow Operator review and actual
upstream returns. Split any candidate slice that cannot fit implementation and
review headroom without dropping its obligations.

| Arc | Useful outcome and delivery home | Actual prerequisites |
| --- | --- | --- |
| Arc01 — persistent Fennel session | Real agent and local launcher/docs/tests in this repository, running through existing Cayrn and xREPL terminal | Accepted bounded native contract; resolved runtime/process/control design and first-slice open set |
| Arc02 — Reedline experience | Fennel profile/configuration here; reusable editor features with xREPL | Arc01 session; approved shared UX activation and actual accepted editor/profile interfaces |
| Arc03 — live development and daily launch | Session-aware completion, normal Fennel development flow and reproducible installation | Arc01/02; agreed query contract for dynamic completion; relevant operational/package contracts |

### Arc01 candidate slices

1. **Evaluate through the existing terminal.** Persistent REPL, isolated sessions,
   native4 handshake/fencing, results/errors, supported output and detach/reattach.
   Include the minimum honest close/control/capture behavior required for the
   first supported profile immediately; conformance cannot be deferred to S02.
2. **Reliable submission and control boundaries.** Multiple-form buffers, exact
   multiple-value behavior, pressure/size handling, nonterminating evaluation,
   close races and worker-loss behavior compose into a usable persistent runtime.
   Demonstrate real cancellation strength, bounded preview reporting and peer
   preservation against the actual chosen supervisor/IPC implementation.

### Arc02 candidate slices — the main user-facing milestone

1. **Everyday editing/history.** Emacs bindings, arrow recall, reverse search,
   persistent private Fennel history and history-based inline hints. Local
   editable history remains separate from Cayrn's durable transcript. Configure
   location/disable behavior and disclose that disabling local history does not
   erase server transcripts; exercise simultaneous terminal attachments.
2. **Highlighting/multiline.** Strings/comments/numbers/delimiters/symbols,
   colon-prefixed strings and standard forms; continuation and pasted buffers.
   Test delimiters in strings/comments, escapes, mismatches, Unicode and cancelled
   drafts. Invalid complete input is submitted for an error rather than trapping
   the user in endless continuation. Preserve whitespace through submission.
3. **Initial completion.** Standard Fennel vocabulary and supported comma
   commands through the shared menu, with correct replacement spans. Dynamic
   session bindings are visibly unsupported until Arc03; do not imply they exist.

### Arc03 candidate slices

1. **Session-aware completion.** Bindings/functions/macros and explicitly
   supported table/module members through an agreed bounded query contract.
   Current Cayrn RPC has no completion operation. Queries are not disguised
   evaluated submissions. Scope them to session/generation and buffer position,
   reject stale responses, bound time/results, and return a safe snapshot or
   temporary unavailability while busy. Avoid arbitrary evaluation/metamethods.
2. **Normal Fennel development.** Verify supported help/doc/reload, result
   variables, module loading and startup configuration. Establish command
   ownership: current xREPL's bare `quit`/`exit` interception must not silently
   swallow valid Fennel source. Coordinate terminal commands with comma commands.
   Add correlated program input only after its shared contract is accepted;
   otherwise retain the explicit limitation and follow-up owner below.
3. **Reproducible daily launch.** Install/start/attach without development-only
   sibling paths; verify clean-environment runtime/compiler dependencies,
   readiness errors, terminal restoration, persistence and supported versions.
   Compose all delivered capabilities against actual installed dependencies.
   Publication is a separate explicit action, not a project-completion side effect.

## Language-guided implementation constraints

- **Fennel/Lua:** preserve nil versus false and result cardinality, including
  trailing nils, using explicit counts at raw-value boundaries. Do not use a
  sparse table's length or `ipairs` as the count of returned values. Test normal
  function/macro definitions and failures without losing unrelated bindings.
- **Runtime choice:** Fennel 1.6.1 with PUC Lua 5.5 is a candidate initial pair,
  not a verified project baseline. Select compiler/VM, socket/JSON/process
  libraries, loading/AOT strategy and checks before implementation. Preserve
  protocol decimal-string identities exactly; avoid lossy numeric conversion.
- **Strings:** Lua strings are bytes. Specify malformed/binary output behavior,
  UTF-8 chunk boundaries and byte-based protocol budgets separately from terminal
  cursor/display width. Test multibyte and combining input.
- **Distribution:** AOT compilation of the agent does not remove its Fennel
  REPL/compiler dependency. Keep `.fnl`, Lua and macro search paths distinct,
  enable explicit global checking for agent builds and execute emitted artifacts
  in a clean environment. Use ordinary modules rather than unnecessary macros.
- **Rust:** respect actual consumed-crate MSRV (currently 1.95), typed errors and
  fallible external input. Keep blocking Reedline work off Tokio workers, use
  bounded channels/caches, and retain ownership/join paths for threads/tasks and
  subprocesses. Test shutdown and cancellation races, not just successful input.
- **Terminal responsiveness:** no synchronous unbounded runtime query per
  keystroke. Refresh/cache bounded language data safely; profile changes must not
  corrupt an in-progress draft. Current buffered output between prompts is an
  acceptable baseline; immediate redraw is not required for MVP.

The implementation open sets must load the current Fennel/Rust topic guides and
inspect source before prescribing APIs. These constraints do not constitute an
implementation recipe or a finished design.

## Proposed definition of done and validation

The [project ledger](ledger.md) carries the proposed acceptance rows. The full
roadmap delivers persistent Fennel semantics, the Arc02 editing milestone,
session-aware completion and supported development commands, plus a reproducible
local installation. The Operator may select Arc01+Arc02 as an earlier useful
delivery milestone; this does not silently remove Arc03 from the project.

Run real Fennel semantics and agent/host integration checks, PTY interaction
checks, and clean-install composition. Cover nil/false/multiple values, local
and macro persistence, errors, output pressure, malformed frames, long sessions,
nonterminating work, peer isolation, history restart/search, completion spans,
Unicode and multiline editing, incoming output during a draft, and terminal
restoration after failures. Carry LFE/fixture regressions with shared changes.
Choose exact commands/assertions at slice preparation; no test runs are claimed
by this draft. File/link/structure checks are not behavioral acceptance.

Each slice returns its rows, actual source/dependency pins, failures/limits and
evidence strength. Independent review remains distinct from implementer checks.
Arc closure tests child composition and bubbles findings up; project closure
tests the whole definition of done and needs independent and Operator gates.
Saving or committing this plan closes no implementation or upstream gate.

## Deferred scope and held decisions

| Item | Owner | Re-entry / decision |
| --- | --- | --- |
| Shared UX activation and reusable terminal package home | Operator and shared xREPL design | Respect X-UX-01 gate or approve a changed sequence before dependent implementation |
| Agent supervisor/worker/IPC design and initial runtime/library set | Fennel design with Operator | Resolve before Arc01 open set; demonstrate responsive control, cleanup and exact native4 contract |
| Live query protocol and safe binding access | Cayrn/shared terminal/Fennel owners | Resolve before Arc03/S01; public Fennel API first, version-scoped plugin/internal access only by explicit decision |
| Program input | Cayrn input owner and Fennel runtime owner | After correlated input arbitration, secret-data persistence, EOF and cancellation contract is accepted; unsupported input remains explicit until then |
| Names/catalog and terminal resume conveniences | Cayrn administration/shared UX owners | After bounded administration APIs; explicit session-ID attach remains the initial path |
| Fully semantic highlighting, automatic indentation, delimiter matching enhancements and optional Vi-mode configuration | Operator/shared UX and Fennel profile owner | Consider after lexical highlighting/multiline/Emacs milestone; not silently included as MVP requirements |
| Remote access, editor plugins, debugger, rich browsers, story/graphics, embedded runtimes, additional Lua VMs | Future scoped projects | Separate use case and plan; no dependency on these for local MVP |
| Heap recovery and automatic source replay | Future architecture decision | Transcript persistence does not authorize or imply either |
| Registry/release publication | Operator | Explicit publication authority after packaging and acceptance; no push/publication in this setup task |

## Source and guidance references

- [xREPL Project10 current plan](/Users/oubiwann/lab/lfe/xrepl/xrepl/.worktrees/planning/project10-shared-repl-architecture/project-plan.md), especially its roadmap and X-UX-01; planning input `eedd817`.
- [Cayrn Project02 current plan](/Users/oubiwann/lab/oxur/cayrn/.worktrees/planning/project02-core-hardening/project-plan.md), especially its roadmap, C-UX-01 and C-ADAPTER-01; planning input `68b557a`.
- [xREPL editor](/Users/oubiwann/lab/lfe/xrepl/xrepl/crates/xrepl-core/src/input.rs), [Cayrn consumer](/Users/oubiwann/lab/lfe/xrepl/xrepl/crates/xrepl-core/src/cayrn.rs), source `cf1f1a2`.
- [Cayrn native contract](/Users/oubiwann/lab/oxur/cayrn/docs/native-agent.md), [protocol source](/Users/oubiwann/lab/oxur/cayrn/crates/protocol/src/lib.rs), source `4aea43a`.
- [Fennel 1.6.1 public API](https://github.com/bakpakin/Fennel/blob/1.6.1/api.md) and [REPL implementation](https://github.com/bakpakin/Fennel/blob/1.6.1/src/fennel/repl.fnl); public APIs are the intended integration surface, internals are evidence rather than a stability promise.
- Installed guidance refreshed for drafting: collaboration-framework 3.0.1,
  project-management 3.0.0, work-verification 2.8.0, fennel-guidelines 1.0.0 and
  rust-guidelines 2.0.0. Follow topic-specific loading at implementation time.

Local reference links identify the inspected workspace; the repository/path and
pins above allow relocation. Existing upstream records are linked, not copied or
reclassified as new accepted evidence.

## Version history

- **v0.1 — 2026-10-20:** Save the conversation's proposed three-arc plan for
  Operator review in `xrepl/fennel`, `project01-mvp`. Refresh framework/language
  guidance; make bounded dependencies, draft authority, first-slice conformance,
  value cardinality, source/command preservation and unresolved process design
  explicit. No arcs opened, upstream schedules changed or implementation accepted.
