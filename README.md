## Nikola Petrović

**Applied AI Engineer · Code Migrations & Software Reliability**

I took [Healthchecks](https://github.com/healthchecks/healthchecks) — a 9k★ cron-monitoring SaaS — from Django 4.2.6 to 5.2.16 and compared 770 recorded HTTP request/response pairs, old version against new, byte for byte. The 686 that exercise the application's own code came back **0 different**. The one real change it surfaced: `GET /admin/logout/` returns 405 instead of 200 — found before merge, rather than as a broken link weeks after deploy.

**→ [The harness, the corpus and the full report](https://github.com/NikolaPetrovicAi/differential-upgrade-harness)**, including a section on what it does not prove.

I'm a reliability engineer, not a Django veteran. A codemod rewrites what its author anticipated; an agent writes the change and sounds certain about it. Neither one measures what the running system did before and after — that measurement is what I build. It comes out of LLM reliability work: evals, tracing, schema-constrained output. Same tooling, pointed at a codebase instead of a model — the other repos are that side of it.

**Stack:** Python (Django, FastAPI), TypeScript/Node, Next.js, Postgres, Docker, pytest · DeepEval, LLM-as-judge, Langfuse, Pydantic/Instructor.

Belgrade, Serbia, Remote
Email: npetrovicai@gmail.com
