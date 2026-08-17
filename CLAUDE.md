# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A 150–180 minute teaching lab (VinUni Cohort 3, Day 17) on **multi-memory systems for agents**, built on the **Zep Cloud V3 SDK**. Everything runs inside Docker; local Redis + Qdrant exist only as baselines to contrast against managed memory. Prose in `README.md` / `LAB.md` is Vietnamese without diacritics — keep that style when editing those files.

Students edit exactly one file: `src/memory_student.py` (4 `LAB TODO` markers). `src/memory_reference.py` is the instructor solution for the same interface — keep the two in sync when the retrieval contract changes, and never "solve" the student file unless explicitly asked.

## Commands

All commands run through `docker compose run --rm app` (see `Makefile` for the full list). Requires `.env` with `ZEP_API_KEY` (copy from `.env.example`).

```bash
make build && make up          # build image, start redis + qdrant
make smoke                     # connectivity check
make seed                      # reset Zep users + semantic graph, ingest all stages
make demo                      # evaluate --impl reference --reuse-seeded
make student                   # evaluate --impl student  --reuse-seeded
make baseline                  # evaluate --impl no_memory
make compare                   # reports/comparison.md from the benchmark JSONs
make test                      # pytest -q
make ui                        # streamlit demo on :8501 (needs GEMINI_API_KEY)
make clean                     # docker compose down -v + delete reports/
```

Single test / single case:

```bash
docker compose run --rm app pytest -q tests/test_privacy.py::test_pii_minimizer
docker compose run --rm app python -m src.evaluate --impl student --reuse-seeded --only-layer semantic
```

Modules are always invoked as `python -m src.<name>` from `/workspace` with `PYTHONPATH=/workspace` (set in the Dockerfile and compose env).

## Architecture

Four memory layers, each with its own retrieval path and its own token budget:

| Layer | Backing store | Retrieval entry point |
| --- | --- | --- |
| `short_term` | in-process `ShortTermMemory` (`src/short_term.py`) | `evaluate.short_term_text()` — never touches Zep |
| `long_term` | Zep user graph + thread Context Block | `retrieve_long_term()` |
| `episodic` | Zep **user** graph, `scope="episodes"` | `retrieve_episodic()` |
| `semantic` | Zep **standalone** graph (`ZEP_SEMANTIC_GRAPH_ID`) | `retrieve_semantic()` |
| `mixed` | all of the above, merged | `assemble_context()` |

Flow: `src/evaluate.py` is the spine. It loads `data/sessions.json`, walks eval cases grouped by `after_stage`, ingests user sessions stage-by-stage (unless `--reuse-seeded`), dispatches each case to the memory impl by `expected_layer`, and writes `reports/benchmark*.{json,md}`.

`src/zep_common.py` holds every Zep primitive shared by seeding, evaluation and the demos: `ensure_user`, `recreate_thread`, `ingest_user_stage`, `ensure_semantic_graph`, `render_graph_search`, and `wait_for_search` (polls until an expected marker string appears — Zep ingestion is asynchronous, so *any* new write must be followed by a readiness probe or the benchmark flakes).

`src/context_budget.py` implements the 10/4/3/3 slide budget with priority `short_term → long_term → episodic → semantic`. Trimming keeps the **head** of each layer because both graph search and the short-term render put the most salient content first.

`src/privacy_guard.py` gates durable writes: `ingest_user_stage` calls `require_memory_consent()` against `data/consent.json`, and all message content passes through `minimize_pii()` before reaching Zep. `src/forget.py` is the deletion drill.

## Zep V3 gotchas that the benchmark depends on

- **Query length**: `graph.search` rejects queries >400 chars. Wrap every query in `cap_query()` (`src/utils.py`) — golden-set queries exceed the limit.
- **Scope choice for semantic search**: use `scope="episodes"` on the standalone graph. It returns raw document text that preserves literal markers (`PAYMENT-RULE-3`, `CONN-POOL-FIRST`). `scope="auto"` returns extracted facts that *drop* those codes, and scoring is literal substring matching. Fall back to `scope="nodes"` only on exception.
- **Context Block needs a primed thread**: `thread.get_user_context()` only ranks relevance against a current thread slice, hence `prime_eval_thread()` recreating the eval thread with the query (`ignore_roles=["user"]`) before every long-term retrieval.
- **Episodic verbosity**: raw session episodes crowd out concise marker-bearing reflections under the 3% budget; `render_graph_search(..., episode_char_cap=180)` is the lever for user-scoped episodic search only.

## Scoring model

`score_case()` does normalized substring matching of `must_contain_all` / `must_not_contain` against the retrieved text — no LLM judge, so retrieval failures cannot be papered over by a chat model. Practice set: 11 cases (E01–E11) in `data/sessions.json`, target ≥9/11. The hidden 20-case golden set lands at `data/golden_eval.json` (gitignored; `data/golden_eval.example.json` shows the shape) and must be 20/20 for the bonus. Never commit `.env` or `data/golden_eval.json`.

## control_plane/

Markdown-only "agent constitution" for the lab (`SOUL.md`, `AGENTS.md`, `CONTEXT_LAYERS.md`, `MEMORY_SCHEMA.md`, `TASKS.md`). Not imported by any code — it is the design contract the Python must uphold (route before retrieve, thread-scoped short-term, user-scoped facts, domain knowledge only in the shared graph, provenance on durable writes). Consult it before changing retrieval or write semantics.
