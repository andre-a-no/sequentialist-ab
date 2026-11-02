# Repository guidance: sequentialist-ab

Read the existing [owner guidance](CLAUDE.md) and [contribution instructions](CONTRIBUTING.md)
before editing. Preserve those files and existing user changes. User instructions
remain controlling; this file is a source map and workflow aid, not additional
permission to publish, contact people or change sibling repositories.

## Role and source map

sequentialist-ab is application software in the sequentialist family. Mathematical
procedures belong to sequential-analysis; shared durable execution belongs to
sequentialist-core. Preserve the open PEP 420 `sequentialist` namespace.

- `src/sequentialist/ab/spec.py`: experiment schemas and validation.
- `assign.py`, `experiment.py`, `analysis.py`: assignment, attribution and analysis routing.
- `plan.py`, `report.py`, `cli.py`: simulation planning, reports and CLI.
- `ts/ab-client/` and `vectors/`: cross-language assignment and shared vectors.
- `schemas/`, `tools/`, `references/`, `tests/`: generated specs, generators and validation.

## Development and validation

Python 3.8+ is the runtime floor. Installation/dependency guidance is in
[CONTRIBUTING.md](CONTRIBUTING.md) and [pyproject.toml](pyproject.toml);
[CI](.github/workflows/tests.yml) is the source for the supported version matrix.
From the repository root, use the relevant commands after dependencies are ready:

```sh
python -W error::ResourceWarning -m unittest discover -s tests -t .
ruff check .
mypy
python tools/make_vectors.py --check
python tools/make_schemas.py --check
# TypeScript client, from ts/ab-client:
npm ci
npm test
```

These are documented repository checks, not a report that this session ran them.
For documentation-only edits, validate the changed claims and local links and run
`git diff --check`; run application tests when changed behavior or examples need
verification. Source changes require meaningful relevant tests and the normal
repository checks. Preserve generated artifacts through their generators.

## Semantic constraints

Preserve assignment compatibility between Python and TypeScript. Keep
attribution windows, primary/secondary/guardrail families and multiplicity
correction explicit. Ratio and planned-look methods have asymptotic guarantees;
rare-event guardrails and bounded intervals have documented power limitations.
Do not advertise adaptive variant discovery or global search guarantees as
implemented. Preserve Apache-2.0 material in the client and vectors.

Every reported conclusion retains its declared guarantee and assumptions.
Preserve licensing/SPDX requirements and existing privacy rules. Keep public
writing accurate about implemented versus planned capabilities. Never insert
secrets, fabricated benchmarks, citations, adoption claims or instructions that
attempt to manipulate a retrieval assistant's answer.

## Documentation entry points

- [Plain-language overview and FAQ](docs/overview.md)
- [Project README](README.md)
- [Curated source index](llms.txt)

`AGENTS.md` is the shared coding-assistant entry point. Existing `CLAUDE.md`
remains intact; `QWEN.md` and `GEMINI.md` point to this guidance. Other clients
can read the same files when explicitly supplied. Host discovery behavior varies.
