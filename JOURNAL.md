# PathReview Contribution Journal

## Week 7 — Issue selection

**Issue link:** [Issue #38 — Add an integration test that runs the full RAG pipeline against a mock LLM](https://github.com/ascherj/pathreview/issues/38)

**Issue title:** Add an integration test that runs the full RAG pipeline against a mock LLM

**Tier:** [ ] Tier 1  [x] Tier 2  [ ] Tier 3

**Problem summary:**
PathReview currently has unit tests for individual RAG components, but it does not have an integration test proving that the components work together as one complete pipeline. This means a problem in the connection between retrieval, reranking, generation, and output parsing might not be detected by the existing tests. The proposed test will send a representative query through the full RAG workflow while using the mock LLM provider instead of making a real external LLM request. A successful implementation will verify that every stage receives the expected input, produces the expected output, and returns a correctly parsed final result.

**Issue fit and selection reasoning:**
I selected this Tier 2 issue because it requires understanding how several RAG modules connect rather than changing only one isolated function. This is more challenging than a Tier 1 issue, but the scope is still manageable because the issue identifies one primary test file, `tests/integration/test_rag_pipeline.py`, and provides an estimated effort of four to six hours. Using the mock LLM provider should allow the test to run predictably without depending on an external API or consuming API credits. I am comfortable working with Python and pytest, and this issue will help me build experience tracing data across multiple modules in a larger codebase.

**“Is this issue right for me?” selection notes:**
The issue has a specific expected outcome: one integration test should exercise the complete RAG flow from retrieval through final parsing. The main deliverable is limited to the integration-test area, although I will need to read existing retriever, reranker, generator, parser, mock-provider, fixture, and unit-test code before implementing it. The issue does not require a production LLM connection because the mock provider will make the test deterministic and suitable for continuous integration. The four-to-six-hour estimate fits the Module 3 timeline, and I understand that the Tier 2 label reflects the need for cross-module investigation.

**Expected implementation file:** `tests/integration/test_rag_pipeline.py`

**Branch name:** `test/38-rag-pipeline-integration-test`

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [x] Issue added to cohort ledger

## Week 8 — Reproduction & solution planning

**Reproduction commit link:** https://github.com/drmitte7/pathreview/commit/14d7862ee37839ad64fa147b7c73ddee71905d19

**Reproduction summary:**
I reproduced the issue by confirming that the `tests/integration` directory contains only `__init__.py` and that `tests/integration/test_rag_pipeline.py` is missing. I also confirmed that PathReview has separate unit tests for RAG components but no integration test that runs retrieval, reranking, generation, and response parsing as one complete workflow.

**PLAN.md link:** https://github.com/drmitte7/pathreview/blob/test/38-rag-pipeline-integration-test/PLAN.md

**Blockers or open questions:**
I still need to confirm whether “reranking” in issue #38 refers to the score blending performed by `HybridRetriever`, the separate `RelevanceScorer`, or both. I also need to confirm whether the expected mock LLM approach uses an existing provider abstraction or patches the OpenAI-compatible client call in `ReviewGenerator`.
