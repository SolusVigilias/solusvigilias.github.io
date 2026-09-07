# MirrorVerse Loop Lab — research note

Version 1 · 7 September 2026 · deterministic toy study · seed 20260907

## Question and loop

What is the smallest retained representation that supports a specified family of future outputs? When discarded distinctions matter, what evidence justifies changing that representation?

The operational loop is: choose an encoding **C** and requirement **(h, event scope, depth D, tolerance ε)** → issue predictions → detect residuals and incompatible merged states → acquire explicit evidence → change the state estimate or schema → rerun the same requirement and measure the cost. A requirement can instead be narrowed; this must be reported as a different task.

A state update changes values within a fixed schema. A representation change alters what distinctions are retained. A coordinate change can be invertible and lose no information. The lab makes all three visible; it does not assume every boundary requires a schema change.

## The exact pair and two consistency conditions

Let z=(a,b), C(z)=x=a+b, initially h(z)=a+b. States (1,3) and (2,2) both encode as x=4. Compare the routes F̄(C(z),e) and C(F(z,e)):

| Event, e=1 | Exact compressed route F̄(4,1) | Full-state route: world A / world B |
|---|---|---|
| Transfer: (a+e,b−e) | x′=x → 4 | 4 / 4 |
| Add: (a+e,b) | x′=x+e → 5 | 5 / 5 |
| Interaction: (a+eb,b) | No unique exact value from x alone | 7 / 6 |

Output recovery requires **h=ĝ∘C**. A deterministic Markov update requires **C∘Fₑ=F̄ₑ∘C**. These maps exist exactly when h and C∘Fₑ respectively are constant on every fiber of C (a fiber is a set of merged full states). Necessity follows by applying a function to identical inputs. Sufficiency follows by defining the value on each fiber. Together, closure and recovery imply exact future outputs by induction, over the stated invariant domain and event family. Output equivalence for a finite requirement may be weaker than closure of a chosen representation.

Sum recovers h=a+b but not h=b. Retaining (x,b) repairs all three rules: interaction x′=x+eb; transfer x′=x, b′=b−e; add x′=x+e. Since a=x−b, this preserves the entire original pair. It is a coordinate change relative to full state and a schema enlargement relative to sum-only.

Under repeated e=1 interactions, the pair's actual sums are 4+3t and 4+2t. Any common real-valued prediction has worst-case error at least t/2. The displayed sum baseline predicts 4 (fixed b̂=0), with worst error 3t. The latter is a deliberately transparent candidate, not an optimal predictor or a uniquely defined compressed dynamics. Its excess error is avoidable; its exact-prediction impossibility is not. Integer-only decoders may have a larger minimax error than the real-valued lower bound.

## Genuine reduction and smallest-representation bounds

Example 2 extends state to (a,b,u,v). Independent byte counters evolve as u′=(u+37) mod 256 and v′=(5v+1) mod 256. They never affect a, b, h=a+b, or h=b. The two demonstrated worlds are (1,3,17,201) and (1,3,99,8). Retaining (x,b) discards **16 state bits** while remaining exact under every supported event. Full and repaired predictors both have zero error across all 69 tested schedules. A negative control changes the output to u: the reduction immediately fails, because the merged worlds require different outputs. Irrelevance is conditional on the requirement.

On the finite initial domain a,b∈{0,…,7}, safe total-output signatures have 15 equivalence classes; including time zero and one e=1 interaction creates 64 classes. Distinguishing them needs at least ceil(log₂15)=4 and log₂64=6 initial bits before codebook and runtime costs. For output b alone there are 8 classes (3 initial bits). These computed partitions address “smallest” for this domain and requirement; the four practical encodings below are not asserted to be optimal. A specialized lookup code also needs its codebook, temporal state, and event context counted.

## Experiment and metrics

The main experiment runs all 64 initial pairs through 69 schedules of length 16: pure transfer, pure add, pure interaction, alternating transfer/add, two adds then interaction, and 64 mixed sequences. A 32-bit LCG with master seed 20260907 draws a seed for each mixed sequence; each sequence draws rules uniformly from the three choices and e from {−2,−1,1,2}. The full sequence list is saved, so reproducibility does not depend on a PRNG library. This is a finite designed test plus one pseudorandom sample, without a population-level statistical claim.

All four methods use identical states, events, and required output a+b: **17,664 main trajectories**, each including times 0…16. The nuisance experiment runs its two worlds through the same schedules: **552 trajectories**. All transitions are analytic. No fitting, training, or optimization takes place.

The approximate method initially stores q=floor(b/4), decodes b̂=4q, and predicts x using b̂. After transfer, q′=floor((4q−e)/4). Add and interaction keep q unchanged. This simple update deliberately exposes repeated-quantization failures; it is not claimed to be the best use of 24 state bits.

**Supported depth H** is the largest k≤D such that absolute forecast error ≤ε for every tested world at every time 0…k. A time-zero failure gives H=−1; reaching 16 is right-censored at the test cap. Cancellation later does not erase an earlier failure. Temporal depth is independent of event scope. Main results use ε=0; per-schedule CSV also reports ε=2.

MAE averages all times (including the initially exact t=0), worlds, and schedules in a group. Peak error and worst H take maxima/minima over that group. A separate information bound groups initial states by C and computes half their future output range. This is a necessary lower bound on worst-case error for any shared real-valued prediction, not a sufficient condition for a realizable predictor.

### Measured mixed-event results

| Representation | MAE | Peak error | Worst exact H | Mean exact H over schedules | State / total bits |
|---|---:|---:|---:|---:|---:|
| Full (a,b) | 0.000000 | 0 | 16 | 16.000000 | 32 / 149 |
| Sum x | 4.596048 | 44 | 0 | 3.171875 | 16 / 133 |
| Approximate (x,q) | 6.596967 | 67 | 0 | 3.171875 | 24 / 141 |
| Repaired (x,b) | 0.000000 | 0 | 16 | 16.000000 | 32 / 149 |

On transfer, add, and the safe sequence, all four methods predict the total exactly through 16. On pure interaction, sum-only has peak error 112 and the approximate method 48; both have worst exact H=0. With two adds before interaction, both support two exact steps. Full and repaired are exact throughout every scenario.

The coarse method's worse mixed-event MAE is a counterexample to “keeping another coordinate necessarily improves prediction.” For b=3 under transfer e=1, updating the compressed state gives q′=−1 while compressing the true successor gives q′=0. A second test uses initially distinct approximate states yet still produces forecast errors: no initial collision is needed for an update rule to fail. Thus a residual alone cannot diagnose insufficient representation. Fiber incompatibility is an information limitation; extra residual can come from decoder or update design. In learned systems, fitting failure would require separate controls; it is absent here.

## Finite precision and memory

| Retained item | Explicit encoding | Bits |
|---|---|---:|
| Full pair / repaired pair | int16(a), int16(b) / int16(x), int16(b) | 32 / 32 |
| Sum-only | int16(x) | 16 |
| Approximate | int16(x), int8(q), fixed quantum 4 | 24 |
| Nuisance bytes, full example 2 | uint8(u), uint8(v) | +16 |
| Schema/version/decoder code | uint8 ID | 8 |
| Requirement | depth:5, output:1, explicit event-scope code:2 | 8 |
| Metric parameter ε | uint16 fixed point, scale 1/256 | 16 |
| Stored event schedule | rule:2 + signed e:3 per step | 5D |
| Current time index | uint5 | 5 |
| Retained history / learned parameters | none | 0 / 0 |

At D=16 the common overhead is 117 bits. Total pair budgets are 149, 133, 141, and 149. Nuisance full costs 165; repaired costs 149. These are packed logical serialization bits, **not process heap measurements**. The supplied bit packer round-trips every stored main-trajectory state and configuration. Byte arrays add 0…7 terminal padding bits (at D=16: full/repaired 19 bytes, sum/approximate 17/18 bytes, nuisance full 21 bytes). All arithmetic is integer; the permitted initial states, e∈[−2,2], and D≤16 remain inside the declared fields. No saturation or wrapping is applied to a,b,x,q; only nuisance counters wrap.

No learned metric, hidden history, or per-world proxy is retained. Fixed rule code and schema codebook are common implementation overhead. Their source serialization, and evaluator data, are reported separately: `model.mjs`: 6,710 bytes; `encoding.mjs`: 3,318 bytes; `scenarios.json`: 84,612 bytes; `traces.csv`: 11,574,814 bytes. File sizes are not a minimal program description length. The seed, world labels, full-state oracle, saved histories, graphs, and logs belong to the evaluator and are unavailable to predictor update functions. Counting that evaluator state as if it were part of a compressed predictor would defeat the separation being tested.

## Repair requires evidence

“Retain exact b” and “retain coarse b” restart from the explicitly available original source, creating a new encoding at t=0; neither recovers discarded information. “Acquire observation” reads exact current (x,b) from an ideal external sensor in each world: **32 incoming bits/world**, consumed into a 32-bit repaired state, plus a persistent 13-bit old-schema/time audit record. The old forecast at the observation step is scored before the measurement; it remains in the chart and cumulative error. Subsequent forecasts are exact. Temporary computation and the incoming sensor buffer are not retained history.

Reading x as well as b is necessary for this repair protocol because an already-drifting x forecast must be corrected too. This sensor is intentionally generous, not a minimal acquisition policy. A physical observation, noise model, and acquisition cost would be needed outside the toy. Narrowing the task excludes interaction and restores total-output sufficiency; it does not increase event coverage.

## Verification, interpretation, next question

The run passed **15 test groups / 323,710 assertions**, including 11,560 commuting-route checks on a,b∈[−8,8], e∈[−2,2], output recovery, the exact pair, coordinate inversion, nuisance independence and its negative control, cancellation, observed repair timing, PRNG repeatability, partition counts, serialization, and **zero encoded overflows**. Finite tests supplement the algebraic arguments; they do not prove a statement about arbitrary new dynamics. Browser interaction checks and delivery checks are recorded separately in `verification.md`.

Established here: the algebraic consistency conditions; the exact counterexample and invertibility; conditional nuisance irrelevance; and the saved finite-run measurements. Still hypothetical: MirrorVerse as a general account of representation learning, boundary discovery, or efficient adaptive compression. No result here establishes that broader claim.

**Next unresolved question:** under a joint bit and observation budget, can a learner with noisy partial observations identify which discarded distinction matters, choose a repair, and outperform both full retention and a fixed compressed model on held-out event families? A useful next experiment must score observation cost, retained memory, calibration, and failures on new contexts, with an exact-model control to separate representation limits from fitting failure.
