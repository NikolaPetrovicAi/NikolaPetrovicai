## Nikola Petrović

**Applied AI Engineer · Code Migrations & Software Reliability**

I took [Healthchecks](https://github.com/healthchecks/healthchecks) — a 9k★ cron-monitoring SaaS — from Django 4.2.6 to 5.2.16, and compared 770 recorded HTTP request/response pairs, old version against new, byte for byte.

| Component | Identical | Equivalent (normalized) | Different |
| :--- | :---: | :---: | :---: |
| **Application** (686 pairs) | 480 | 206 | **0** |
| Django's bundled admin (84 pairs) | 42 | 1 | 41 |

The one real behavior change it surfaced: `GET /admin/logout/` returns **405 instead of 200** — Django made admin logout POST-only in 5.0. Nobody clicks admin logout during QA, so it normally lands as a broken link weeks after deploy. Here it was one line in a machine-generated report, before merge.

**→ [The harness, the corpus and the full report](https://github.com/NikolaPetrovicAi/differential-upgrade-harness)** · [report only](https://github.com/NikolaPetrovicAi/differential-upgrade-harness/blob/master/demo/report/healthchecks-django-4.2-to-5.2-differential-report.md)

### How it works

- **Record first.** Walk the URLconf, crawl the rendered UI, script the write sequences where the third call depends on what the first two wrote — frozen clock, seeded database, pinned settings.
- **Then the upgrade goes in** and the same corpus replays against the new version.
- **A rulebook strips what always differs** — CSRF tokens, session IDs, admin markup Django itself rewrote. Every masked pair names the rules that masked it, so masking is auditable instead of trusted.
- **What survives is a real change**, with the request that caused it attached. None of it comes from the project's test suite: the corpus is black-box, which is the situation of a codebase with thin tests.

### What this does not prove

- **Route coverage is 74%** — 44 URL patterns uncovered, each listed by name with a reason.
- **The comparison stops at the HTTP request → response boundary.** Database effects are seen only through responses; email and webhooks were never exercised.
- **7 of 7 injected regressions were caught** — a sensitivity floor, not a recall guarantee. I chose those 7.
- **The gate counts the whole corpus, admin included**, so the headline run ends with `FAILS — 41 DIFFERENT`. Those 41 are Django's own template edits, itemized in the report.

### Where this comes from

I'm a reliability engineer, not a Django veteran. A codemod rewrites what its author anticipated; an agent writes the change and sounds certain about it. Neither one measures what the running system did before and after — that measurement is what I build.

It comes out of LLM reliability work: evals, tracing, schema-constrained output, retrieval that has to cite its source. Same tooling, pointed at a codebase instead of a model. The repos below are that side of it.

**Stack:** Python (Django, FastAPI), TypeScript/Node, Next.js, Postgres, Docker, pytest · DeepEval, LLM-as-judge, Langfuse, Pydantic/Instructor.

### Contact

Belgrade, Serbia · remote · [linkedin.com/in/nikolapetrovicai](https://www.linkedin.com/in/nikolapetrovicai) · npetrovicai@gmail.com
