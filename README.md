### Emilios Richards

Full-stack and AI engineer, TypeScript and Python. For over a year I was the sole developer at a small B2B software company and built its entire technical stack: backends, data pipelines, browser automation, monitoring, and the AI tooling on top.

The repositories below are independent reimplementations of patterns I designed and ran in production, built against synthetic data.

### Start here

Each repo is built around one problem. Open the named file first.

| Repo | The one idea | Open this file first |
| --- | --- | --- |
| **interval-state-tracker** | A `setInterval` poller double-counts durations when a tick hangs. A monotonic generation number makes the late tick unable to matter. | [`test/tracker-concurrency.test.ts`](https://github.com/EmiliosRichards/interval-state-tracker/blob/main/test/tracker-concurrency.test.ts). It reproduces the double-count with the guard off, then shows it gone with the guard on. |
| **agent-orchestration-framework** | An AI agent that both decides and reviews its own work is unreviewable. Separate the roles, declare each one's write boundary, and check the boundaries mechanically. | [`examples/rate-limiting/`](https://github.com/EmiliosRichards/agent-orchestration-framework/tree/main/examples/rate-limiting). One cycle end to end. It is a fabricated illustration and says so. |
| **zero-cost-pipeline** | A service that must never spend money should not be the thing deciding whether it does. The free lane holds no grant on the paid table, so Postgres refuses the statement whatever the code says. | [`tests/test_privileges.py`](https://github.com/EmiliosRichards/zero-cost-pipeline/blob/main/tests/test_privileges.py), `test_the_archiver_cannot_write_the_paid_lane`. It inserts a real row first, so a dead connection cannot pass the next step for the wrong reason. Then the paid insert has to fail with SQLSTATE 42501. |
| **case-study-matcher** | A reranker calls a match strong and quotes the sentence that proves it. The sentence is not always in the document, so a verdict only stands once its cited spans are found verbatim in the two documents the judge was shown. | [`tests/test_evidence_gate.py`](https://github.com/EmiliosRichards/case-study-matcher/blob/main/tests/test_evidence_gate.py), `test_the_gate_downgrades_fabricated_evidence_and_keeps_real_evidence`. Both directions in one test. A gate written as `return "partial"` would pass either half on its own. |
| **llm-fit-scorer** | A model told to search at most three times may search thirty. The instruction is a request, so the allowance here is a counter that refuses the search once it is spent. | [`tests/test_budget_enforced.py`](https://github.com/EmiliosRichards/llm-fit-scorer/blob/main/tests/test_budget_enforced.py). A run that stays inside its allowance finishes, and the one that would exceed it is refused, so the guard cannot pass by refusing everything. |
| **host-health-alerting** | A machine that dies leaves behind a last row saying it was fine, and a threshold loop reads that row and agrees, every time it looks. So a node whose newest row is older than the horizon is treated as unobservable, whatever that row says. | [`tests/test_verdict.py`](https://github.com/EmiliosRichards/host-health-alerting/blob/main/tests/test_verdict.py), `test_a_node_that_died_looking_healthy_is_not_reported_healthy` and the critical-looking twin beside it. Both directions, so the rule cannot pass by calling every node unheard. |
| **resilient-scraper** | A hostname that does not resolve now will not resolve in four seconds either. Retrying it spends attempts on an answer that cannot change, so a name that fails to resolve gets respelled and asked again instead. | [`tests/test_reckoning.py`](https://github.com/EmiliosRichards/resilient-scraper/blob/main/tests/test_reckoning.py). It closes the hyphens in the registered label and leaves them alone in a leading one, so a rewrite that stripped every hyphen it saw would fail. |
| **search-query-parser** | What the user meant and how the store answers it arrive together as one function call, and they are two questions. Split at a typed boundary, the emitted statement text becomes a function of the filter's shape alone. | [`test/sql.spec.ts`](https://github.com/EmiliosRichards/search-query-parser/blob/main/test/sql.spec.ts), "emits identical statement text for every term of the same shape". Twelve terms, `'; DROP TABLE contacts; --` among them, collapse to one string and differ only in bound parameters. |

### What each one demonstrates, and what it does not

- **interval-state-tracker** is working code with a test suite that runs offline, on synthetic data, with no credentials.
- **agent-orchestration-framework** is a specification, not an application. It asserts a method. It is not empirically validated: there is no benchmark and no A/B comparison. What the build checks is narrower, that the specs do not contradict themselves.
- **zero-cost-pipeline** is working code, and the privilege boundary is checked against a real Postgres in CI rather than mocked. Transcription in it is a deterministic stub that hashes a file into words from a fixed vocabulary. No speech recognition happens anywhere in the repository. The containerised demo has been run; CI parses the compose file rather than starting the stack. No cost saving is measured or claimed.
- **case-study-matcher** runs offline in CI with no API key, and the evidence gate is exercised in both directions. The default reranker is a deterministic stub, not a language model: it derives verdicts from a hash, so its agreement with the labels is chance and the eval harness prints that number. The corpus and the rubric were invented for the repository, and the reported metrics come from those fabricated labels. They say nothing about accuracy on real documents.
- **llm-fit-scorer** runs offline in CI with no API key. The scorer that ships is a deterministic stub reading fixture pages, not a language model and not the live web. An optional model-backed path is included and no build has executed it. The venues, the rubric and the reference ratings are all invented here, so the reported error measures agreement with a fixture.
- **host-health-alerting** is working code, and CI runs three things: the decision suite, the live-store suite against a real PostgreSQL with skipping turned into a hard failure, and a boot of the whole container composition. The fleet is invented. Each reading is a straight line plus a sine wave with a per-series phase offset, and the outage is a boolean on a segment. The reporter's accelerator path has never run against real hardware, because no accelerator was available.
- **resilient-scraper** is working code, and its demonstration crawls a site bundled inside the repository, so no live host is ever contacted. Fetching the browser build beforehand is a one-time install step that does use the network. The failing hostname and the respelling that rescues it are both fixtures, and nothing here measures how often a real list carries that artefact.
- **search-query-parser** runs offline with no credentials. Where the native SQLite binding will not build, the integration and conformance suites skip loudly rather than failing, so a green run does not quietly cover less than it appears to. Every contact, company and postcode in it is fabricated.

Also public: [**spec-manager**](https://github.com/EmiliosRichards/spec-manager), a client
application I delivered in 2025, published with synthetic data standing in for the client's. Its
README records how it was built, including which parts came from an agent scaffold and which are
mine by git blame.

The eight repositories above are MIT licensed.

### Contact

[LinkedIn](https://www.linkedin.com/in/emiliosrichards/)
