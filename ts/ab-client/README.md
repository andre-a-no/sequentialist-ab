# @sequentialist/ab-client

Assignment of units to the variants of an A/B experiment, identical bit for
bit to [sequentialist-ab](https://github.com/andre-a-no/sequentialist-ab)
(Python). No dependencies; runs in browsers, workers and Node.js 16 or later.
Licensed under the Apache License 2.0, so it can go into any front end.

## Place in the Sequentialist ecosystem

This client is an application component of
[sequentialist-ab](../../README.md). Its responsibility is reproducible variant
assignment in a browser or Node.js; the A/B engine owns attribution, analysis,
product decision rules and reporting.

The ecosystem separates mathematical methods (`sequential-analysis` for
inference, `sequential-search` for search and `sequential-games` for participant
interaction), reusable engineering (`sequentialist-core`, `sequentialist-visuals`)
and domain applications such as A/B testing. This client consumes an assignment
configuration and returns an assignment; it does not perform a statistical test,
select an experiment winner or inherit an error guarantee from the naming scheme.

```js
import { assign } from "@sequentialist/ab-client";

// the configuration printed by `sequentialist ab client-config experiment.json`
const config = {
  salt: "checkout-2026-11",
  variants: [{ name: "A", weight: 1 }, { name: "B", weight: 1 }],
  traffic: [{ from: 1793491200, share: 0.1 }, { from: 1793664000, share: 1 }],
  overrides: {},
  start: 1793491200,
  end: 1795910400,
};

const { variant, probability, reason } = assign(config, unitPseudonym, Date.now() / 1000);
// variant: "A", "B" or null (not in the experiment now); report it with the exposure,
// together with `probability`, and the engine checks both against the spec.
```

- `unitPseudonym` is the unit's pseudonym (for example an HMAC of the user
  id under your key), never a raw user id.
- The same configuration, unit and time give the same answer here and in
  the Python engine: both are tested against the shared vectors in
  [`vectors/assignment.json`](../../vectors/assignment.json).
- How it works: a unit takes part if a hash of the salt and the unit, as a
  number in [0, 1), is below the traffic share; raising the share keeps
  everyone already in. A second, independent hash picks the variant in
  proportion to the weights. SHA-256 is computed synchronously in
  JavaScript, so no `crypto.subtle` is needed.
