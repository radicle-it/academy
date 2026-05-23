# Radicle Academy

> **Status: 🚧 work in progress — initial planning phase.**
> No training content yet. The repository is currently being designed.

## What this will be

A public training repository by [Radicle](https://www.radicle.it), an Italian Oracle-focused software company. It will collect lessons, labs, tutorials, exercises, code samples, and curated references to help newcomers — both Radicle hires and anyone on the internet — build transversal skills on the Oracle stack: **Oracle Database (19c), SQL, PL/SQL, APEX, ORDS, OCI fundamentals, BI/ETL**, and the surrounding tooling.

Content language: **English**.
Primary target database: **Oracle 19c**. Future-facing 23ai topics will live in a dedicated appendix.

## Current state

The repository is **empty of training material**. We are in the syllabus-design phase: defining domains, levels (Junior / Intermediate / Senior), formats, and contribution rules.

The planning conversation is captured under [`planning/`](./planning):

- [`planning/prompt.md`](./planning/prompt.md) — original brief
- [`planning/prompt-review.md`](./planning/prompt-review.md) — first-pass analysis
- [`planning/review_v2.md`](./planning/review_v2.md) — revised taxonomy (21 domains)

Once the taxonomy is approved, content will live under `docs/`, `labs/`, and `exercises/`.

## Contributing

Contribution rules are not finalized. The intended model is:

- **Trainers** (whitelisted Radicle members) push to feature branches and open PRs to `main`.
- **Collaborators** create personal branches named after their GitHub handle for exercise solutions, then open PRs.
- `main` will be protected; reviews via `CODEOWNERS`.

A `CONTRIBUTING.md` will follow.

## License

See [`LICENSE`](./LICENSE). A separate license for code samples is under consideration (Apache-2.0 or MIT).
