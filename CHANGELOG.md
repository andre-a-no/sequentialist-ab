# Changelog

## sequentialist-ab 0.1.0, @sequentialist/ab-client 0.1.0 — unreleased

First version, on sequentialist-core 0.1.0 and sequential-analysis 0.4.
Developed in the repository that is now
[sequentialist-core](https://github.com/andre-a-no/sequentialist-core),
together with it; moved here with its history.

### sequentialist-ab

- The experiment spec `ab.experiment/1`: variants and weights, ramp-up,
  overrides for QA, attribution windows and lateness, maximum duration,
  primary, secondary and guardrail metrics of four types (conversion, bounded,
  ratio, rate), direction, MDE, margins, alpha, continuous or planned looks.
- Assignment by SHA-256 of the salt and the unit, identical in Python and
  TypeScript (shared vectors), with the probability of each assignment.
- Aggregation per unit over its attribution window; each unit enters the
  analysis once, when its window closes.
- Analysis with the procedures of sequential-analysis, each labelled with its
  guarantee; e-BH or Bonferroni over metrics and treatments; sample ratio
  mismatch on every exposure.
- Decisions (ship, drop a variant, stop) on primary success, guardrail harm or
  non-inferiority, futility, duration, by hand; each in the audit log.
- Reports, planning of power and duration, A/A on the real pipeline.
- Checked against GrowthBook (gbstats 0.8.0) and the mSPRT of Optimizely
  (`references/`), and run end to end on the MineThatData e-mail experiment.

### @sequentialist/ab-client

- Assignment of variants for browsers and Node.js 16+, no dependencies,
  Apache-2.0.
