# digital-badges-poc
🏴󠁧󠁢󠁳󠁣󠁴󠁿 Project tracker for a Scottish digital badges proof of concept

This repository holds **documentation and deployment orchestration** for the PoC.
Application services (for example ORCA and the DCC stack) ship as **published
Docker images**; compose, ingress, and env configuration here reference those
images rather than embedding primary application source code.

## First run

Each service has a `.env.<role>.example` template at the repo root
(`.env.transaction.example`, `.env.signing.example`, `.env.postgres.example`).
For each one, copy to `.env.<role>` and fill in real values before starting
the stack. Rendered `.env.<role>` files are gitignored.

Bring the stack up with:

```bash
docker compose up -d
```

### Postgres lifecycle

The `postgres` service stores its data in a named volume (`postgres-data`),
so:

- `docker compose down` preserves the database; the next `up` reuses the
  existing data and **does not** re-run the schema init script.
- `docker compose down -v` deletes the named volume, which **wipes the
  database**. The next `up` will run the init script against an empty data
  directory.

For an ad-hoc `psql` session against the running container (no host port is
published):

```bash
docker compose exec postgres psql -U orcaadmin orca
```
