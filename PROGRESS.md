# PROGRESS.md

Project diary: where we are, what was decided and why. Updated at the end of every work session.

## Current status

Phase 0 (setup and data exploration) in progress. Repository created; base files in place.

## Next step

1. Write `.gitignore` (must include `.env` and the raw data folder) and `.env.example`, then make the first commit.
2. Explore the ONS hourly energy load dataset by hand (columns, grain, period covered, file format, data quality issues).
3. Bring up PostgreSQL 16 with Docker Compose and connect to it (`SELECT version();`).

## Phases

- [ ] **Phase 0 – Setup and exploration:** repository, base files, understand the source data, Postgres running in Docker.
- [ ] **Phase 1 – Ingestion:** Python script that downloads new ONS data daily and stores it raw. Must be idempotent (running twice does not duplicate data).
- [ ] **Phase 2 – Transformation and tests:** dbt models in layers (raw → staging → marts) with data tests.
- [ ] **Phase 3 – Orchestration:** schedule and chain the steps; a failed step blocks the next ones.
- [ ] **Phase 4 – Packaging and CI:** everything runs in Docker; GitHub Actions runs tests on every change.
- [ ] **Phase 5 – ML in production:** next-day hourly load forecast, run daily, with error monitoring and alerts when the model degrades.
- [ ] **Phase 6 – Delivery:** dashboard and a README explaining the architecture and decisions.
- [ ] **Later – Cloud:** move the platform to a cloud provider.

## Decisions log

| Date | Decision | Why | Alternatives considered |
|------|----------|-----|-------------------------|
| 2026-10-04 | Use ONS open data as the source | Free, public, updated daily, hourly grain; related to the energy sector | Other public datasets |
| 2026-10-04 | PostgreSQL 16 as the database | Free, industry standard, runs in Docker; teaches running a real database server | DuckDB (simpler, but no server to operate) |
| 2026-10-04 | Docker Compose instead of standalone `docker run` | Configuration lives in a versioned file; anyone can reproduce the setup with one command | Manual `docker run` commands |
| 2026-10-04 | Pin image versions (e.g. `postgres:16`, not `latest`) | Avoid silent upgrades that break the setup | `latest` tag |
| 2026-10-04 | dbt-core for transformations | Industry standard for SQL transformations with built-in testing and documentation | Plain SQL scripts |
| 2026-10-04 | Repository content in English | Portfolio project, readable by any recruiter or reviewer | Portuguese |
| 2026-10-04 | Secrets in `.env` (git-ignored) plus `.env.example` | Never leak credentials; still document required variables | Hard-coded values |

## Open questions

- Does ONS publish hourly load by region (subsystem) or only at national level?
- How are the files organized (one per year, per month)? What format (CSV, Parquet)?
- How far back does the data go, and how late does each day's data arrive?
