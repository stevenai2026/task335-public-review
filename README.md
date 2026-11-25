# Request for engineering review: Task335 dual-worker prototype (V1.2)

We are seeking critical human engineering review, not endorsements. This prototype and its documentation were developed with AI assistance. No independent production audit or architectural novelty is claimed.

**Main question:** Does a dispatch/review worker A plus execution worker B justify its complexity compared with one workflow using the same models and deterministic checks? Please recommend continue, simplify, or stop, with reasons.

## Downloads
- [Complete V1.2 package: source samples, test receipts and PDF](Task335_Public_Review_Package_V1_2.zip)
- [Chinese technical report (PDF)](Task335_Public_Review_V1_2.pdf)

Extract the ZIP before running the commands below.

## Critical scope distinction
The frozen execution candidate covers CanadaBuys and UK Find a Tender. A research route and hourly schedule are not established. Historical research/model/dedup successes do not validate this candidate. Current production status is UNKNOWN.

## What exists
Historical receipts describe A/B communication and model responses, including a mathematical verification failure after API success. A reread offline delta reports A52/B50 nodes and authentication-reference changes at A18+B16 nodes, totaling 68 field replacements. Those numbers establish neither deployment nor 34 state writers. Write capability requires inspecting method, target, credential scope and reachable path.

## Runnable public examples
01: mock A/B stages on one canvas; A recomputes arithmetic from task operands. Incorrect answer 6 is rejected. No ledger readback is implemented.
02: source/item/version/content-SHA256 dedup; an unchanged version with changed content remains new. Four fixtures yield 2 new, 1 duplicate and 1 source failure. No cross-run persistence.
03: nine admission fixtures covering call count, worst-case money/token bounds, expiry, revocation, STOP, config drift and prior uncertainty. These are synthetic checks, not persistent accounting.
04: Python standard-library SQLite experiment with real competing subprocesses and a loopback mock HTTP server. BEGIN IMMEDIATE wraps conditional claim and budget reservation. The mock server persists success, then the client exits before writing its response. After confirmed client death, recovery marks UNCERTAIN and retains the reservation. No automatic retry.

## Observed verification
Samples 01–03 passed local JavaScript replay; n8n import and execution have NOT been tested. Sample 04 passed 20 rounds per scenario: same-task competition, last-budget competition, and post-response/pre-write crash (60 scenario checks). The mock endpoint recorded 20 requests, one per operation. Paid model calls: zero. No production workflow ran.

Reproduce: `node workflow_samples/verify_samples.js` and `python3 workflow_samples/04_persistent_fault_demo/verify.py`. JSON files can be inspected and imported into an isolated n8n test project, subject to unverified import compatibility. Native executions may consume your plan allowance.

## Explicit limits
Single-host temporary SQLite files survive caller-process death, not a tested host power loss. Production leases, stale-writer fencing, network partitions, provider billing reconciliation and general research validation remain unimplemented in this demo. Cost units are synthetic. Test duration/engineering effort is not zero-cost. No formal human-intervention time measurement was made.

## Review questions
1. Can you produce a race or recovery trace that violates these local guarantees?
2. Which invariants must be enforced at the durable write boundary before n8n integration?
3. How should worst-case paid cost be reserved and reconciled when the provider outcome is ambiguous?
4. Does separating A/B deliver demonstrable privilege or maintenance benefits?
5. What additional tests are necessary before a bounded pilot?

MIT license applies ONLY to newly authored code/docs under workflow_samples. Private production assets are not released. Please distinguish reproduced results from design opinions. The V1.2 archive is preserved as prepared; publication-status statements inside it describe its pre-publication state.

## Comparison we have NOT run
A controlled single-workflow versus dual-worker comparison with identical inputs, models and budget remains outstanding. Useful measures would include wrong acceptances, duplicate paid calls, completed tasks, human-intervention minutes, latency and total cost. Local PASS results do not establish superiority. Please separate code reproduction, design judgment and production experience in your response.
