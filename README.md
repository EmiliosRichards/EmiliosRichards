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

### What each one demonstrates, and what it does not

- **interval-state-tracker** is working code with a test suite that runs offline, on synthetic data, with no credentials.
- **agent-orchestration-framework** is a specification, not an application. It asserts a method. It is not empirically validated: there is no benchmark and no A/B comparison. What the build checks is narrower, that the specs do not contradict themselves.
- **zero-cost-pipeline** is working code, and the privilege boundary is checked against a real Postgres in CI rather than mocked. Transcription in it is a deterministic stub that hashes a file into words from a fixed vocabulary. No speech recognition happens anywhere in the repository. The containerised demo has been run; CI parses the compose file rather than starting the stack. No cost saving is measured or claimed.
- **case-study-matcher** runs offline in CI with no API key, and the evidence gate is exercised in both directions. The default reranker is a deterministic stub, not a language model: it derives verdicts from a hash, so its agreement with the labels is chance and the eval harness prints that number. The corpus and the rubric were invented for the repository, and the reported metrics come from those fabricated labels. They say nothing about accuracy on real documents.
- **llm-fit-scorer** runs offline in CI with no API key. The scorer that ships is a deterministic stub reading fixture pages, not a language model and not the live web. An optional model-backed path is included and no build has executed it. The venues, the rubric and the reference ratings are all invented here, so the reported error measures agreement with a fixture.

Also public: [**spec-manager**](https://github.com/EmiliosRichards/spec-manager), a client
application I delivered in 2025, published with synthetic data standing in for the client's. Its
README records how it was built, including which parts came from an agent scaffold and which are
mine by git blame.

The five repositories above are MIT licensed.

### Contact

[LinkedIn](https://www.linkedin.com/in/emiliosrichards/)
