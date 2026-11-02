# sequentialist-ab: overview and frequently asked questions

English · [Русский](overview.ru.md)

> An embeddable engine that turns experiment events into auditable A/B testing decisions with stated statistical guarantees.

## Who it helps

Product teams and experimentation-platform developers who need consistent variant
assignment, attribution windows, repeated analysis and an explanation of why an
experiment was shipped, stopped or continued. A platform supplies the event
pipeline, user interface and feature-flag execution.

For a reader without a programming background: specify what to compare and what
counts as success or harm; feed the experiment its observations; inspect the
decision, uncertainty and supporting history. Software integration is required
to collect the data and apply a decision.

## Inputs and outputs

| Input | Purpose |
|---|---|
| Experiment specification | Variants, traffic schedule, metrics, risks, attribution window and maximum duration |
| Exposure and outcome events | Which unit saw a variant and what happened within its attribution window |
| Explicit human commands | Record a manual stop with an actor and reason |

Outputs include ship/stop/continue decisions, JSON/Markdown reports, analysis
streams and hash-chained audit entries. Each statistical conclusion retains the
guarantee declared by its underlying procedure.

## A practical workflow

1. Define variants, primary metric, secondary metrics and guardrails.
2. Validate the spec with `sequentialist ab check SPEC`.
3. Integrate unit assignment and event collection; declare engine or external assignment.
4. Process closed attribution windows and inspect decisions plus guardrail evidence.
5. Use the report and audit record to explain the outcome.

The [Hillstrom example](../examples/hillstrom.py) shows a complete experiment on
an external dataset. Its data requirements and validation are documented in the
[README](../README.md); this overview introduces no new benchmark claim.

## Current scope and limits

The checked-in package version is 0.1.0; package availability and installation
instructions should be checked in the [README](../README.md) and
[project metadata](../pyproject.toml). Implemented features include assignment,
attribution, conversion/bounded/ratio/rate metrics, sample-ratio checks, decisions,
planning simulations and A/A checks.

Anytime-valid procedures permit repeated monitoring under their stated
assumptions. They do not repair biased event collection or invalid assignment.
Ratio and group-sequential planned-look results are asymptotic. Multiplicity is
handled by the package's declared e-BH/Bonferroni choices; guardrails form their
own family. Rare conversion events can require impractically large samples for
narrow non-inferiority margins. Current intervals for bounded metrics have
conservative widths. See the README for details and examples.

Automated exploration of new variants across a search graph is an ecosystem
extension, not a claim about this version.

## Frequently asked questions

### Is this a complete hosted experimentation product?

It is an engine intended for integration into a platform. Event collection,
feature flags and the product interface remain application responsibilities.

### Can I look at results after every observation?

Supported sequential methods are designed for repeated monitoring under their
specified assumptions. The selected method, metric and multiplicity correction
still determine the reported guarantee.

### Does a ship decision automatically change production?

The engine records a decision. The platform defines who may act on it and how
feature flags or releases are changed.

### How is the browser assignment kept consistent with Python?

The Python and TypeScript clients use shared assignment vectors. See
[`assign.py`](../src/sequentialist/ab/assign.py), the [client](../ts/ab-client/)
and [vectors](../vectors/).

### Which package should I use for other tasks?

Use [sequential-analysis](https://github.com/andre-a-no/sequential-analysis) for
statistical procedures, [core](https://github.com/andre-a-no/sequentialist-core)
for durable application infrastructure, and this package for A/B experiments.

## Sources and AI-assisted reading

Start with the [README](../README.md), [spec implementation](../src/sequentialist/ab/spec.py),
[analysis routing](../src/sequentialist/ab/analysis.py), [tests](../tests/) and
[contribution instructions](../CONTRIBUTING.md). The [curated index](../llms.txt)
points to source documents for retrieval and attribution.

Coding assistants should read [AGENTS.md](../AGENTS.md) and preserve existing
[owner guidance](../CLAUDE.md). `QWEN.md` and `GEMINI.md` provide entry pointers
for compatible clients. Retrieval tools, including general chat and search
assistants, can use the same readable source pages; client support and indexing
vary. This documentation does not establish automatic ingestion or search ranking.
