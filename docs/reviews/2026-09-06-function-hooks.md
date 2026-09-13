# Claude function adapter qualification

Development changes based on `7da3b17440b43fb49e152acbffb30f082ae72ed7`.
Version remains 3.12.2 pending the coordinated release. No installation,
enablement, user-settings changes or commits were made by this implementation
agent. The user's actual disabled marker was confirmed by read-only describe.

## Invariants exercised

The fixture executes the **actual installed Claude Code 2.1.263 binary**. Only
the Anthropic provider is replaced with a deterministic loopback HTTP fixture.
Each run uses new temporary Git, home, Claude config and Signet directories;
it does not load user hooks, MCP servers or API credentials.

| Case | Observable invariant |
| --- | --- |
| Active Signet plugin, synthetic prompt/Bash secret | Model requests and fresh tool results contain the mask, not the synthetic values |
| Disabled Signet | Plugin is neutral; no false claim of sanitation |
| Signet + Kindex, either plugin order | TaskCreate and TaskList share one durable task in the actual worktree `.kin/local/kindex/kindex.db`; no Claude native task directory |
| Explicit semantic target denial | TaskCreate returns an error; zero task rows |
| Signet disabled after session selects it | TaskCreate returns an error; no fallback mutation |
| Native subject rule | Original source field/value is evaluated, despite target title redaction; zero task rows |
| Unicode title | Rust/Python receipt digests agree; native result schemas succeed; reported outcome reaches Signet |
| Settings-file feature flag | Modules load and redact without a parent environment flag or command-line settings override |

Representative artifact directories under the temporary root printed by
`tests/claude_function_host.py` during qualification:

- `.qwx1rblu`: strict Unicode/source-input receipt validation.
- `.a37kg4s9`: raw native subject policy denial, zero task mutation.
- `._g78r2a_`: reversed plugin order.
- `.i4gawgw7`: selected Signet disabled before task execution.
- `.rvrrsa8k`: explicit target denial.
- `.75qw1873`: settings.json-only feature activation.
- `.wa_csj3h`: disabled plugin neutrality.

All are prefixed `signet-function-host` in the OS temporary directory and include
`debug.log`, `stdout.jsonl`, `api-requests.json`, and isolated databases. They are
local development evidence, not distributed fixtures or a claim about every
Claude mode/platform. The checked-in script reproduces these cases.

Final `cargo test --locked`: 247 passed (169 unit, 10 CLI, 55 hook,
10 protocol, 3 installer). The embedded installer tests cover
foreign-sibling/disabled preservation, wrapper refusal and linked/unowned targets.
`cargo fmt --check` and `git diff --check` passed. `cargo package --allow-dirty
--list` includes the adapter, protocol, sanitizer and host fixture. Release build:
`cargo build --release --locked` passed; installation is a separate owner step.
Final development binary SHA256:
`e4bab003039b0d4d7baadf277fa62b8ca322a3a398fb99d158f91cf54ba464d9`.

The final distribution step added `integration install-modern`, embedding all
three runtime assets in the Rust binary. It was executed only against temporary
configs: `.ss6serxn` proves skills-dir loading/redaction without a source plugin
path, `.zowxeybx` preserves disabled neutrality, and `.ssdm1axp` proves the installed
Signet adapter plus Kindex accepts Unicode receipts and persists one task. Exact
owned legacy handlers are retired, foreign siblings retained, backups kept
outside plugin discovery. No user installation was performed by this agent.

## Red probes and dispositions

Actual host execution caught two Kindex load failures: importing runtime JSON
was parsed as JavaScript, and passing `$` to a helper nested inside `register`
violated the host's static checker. The Kindex owner replaced the JSON module
with TypeScript and moved helpers to module scope. Both then loaded in the real
host. These failures remain in the local artifact directories `.a65o6w6r` and
`.7rfzjhcm`.

A fresh replacement of a failed builtin result still encounters its output
schema. Signet now returns sanitized error text through the host error channel
instead of presenting that text as a successful typed output.

Source-only policy criteria were initially evaluated against translated target
fields. The protocol now requires and digest-binds original native source input,
evaluates source and target separately, and never persists their raw plaintext.
Operation IDs were initially globally unique; the ledger now namespaces them by
project/session/agent/profile while rejecting changed input in the same scope.

The existing settings self-protection treated native reads as edits. It now
exempts only exact `Read`, `Grep`, and `Glob`; mutation tools and arbitrary shell
text still require policy approval. Other binary/directory protections remain.

## Adversarial framing and simplification review

The user-requested Sim tool reviewed the final claim, and the ask-cpa skill's
four lenses were applied in order: simplicity, evidence, conventions, process.
Predicted feedback and disposition:

1. [ESTABLISHED] "Show the real path, not only a test double." The actual Claude
   binary loaded both plugins; the model provider alone is mocked. Raw/failed
   probes were retained, and effects were checked in the real SQLite databases.
2. [ESTABLISHED] "What does this receipt actually prove?" Intent is committed
   before effect; Kindex commits effect/result/outbox atomically. Signet's later
   outcome is labeled caller-reported, never verified execution. No new daemon,
   policy engine, secret reinsertion layer or host-state authority was introduced.
3. [GUESS] "Don't imply the hashes hide short secrets." Documentation now states
   low-entropy digest guessing, plaintext process-memory limits and lack of
   hash-chain/tamper-evidence. Adding cryptographic receipt infrastructure is not
   required to deliver the scoped cooperative integration.

Predicted verdict: "Keep the evidence and limits explicit." This is a prediction,
not human approval or independent certification.

Sim also asked about Unicode, ordering, revision identity and the verifier.
The contract uses compact sorted-key UTF-8 JSON, without Unicode normalization;
tool and identifier admission is exact. Policy revision is a content digest of
the effective rules and implementation revisions. Kindex validates admission
fields/digests before effect; Signet checks exact receipts on outcome delivery.

Host loader failure, future API drift, earlier raw prompt queue records,
assistant output, third-party telemetry, old transcripts/backups and arbitrary
undetectable secrets remain outside the guarantee. See the adapter README's
controlled-surface inventory. Historical cleanup is not silently claimed.
