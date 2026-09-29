## Commit 2d830c7 — Section 83b.6: section-83b2 fallback plan drafted for the case of hypothesis B -- measure the value of SELECTION itself without touching physiology or core code, by generating K=3 trajectories per seed through the existing --eta knob (0.06 / 0.12 / 0.24) and scoring the population as the max of K; if K=3 lifts the median markedly (e.g. +67.6% -> +80%), the section-82.7 conclusion (the lever is the selection policy) gets direct confirmation and the next step is K bodies inside the engine


---

## Commit 3cce95d — Section 83b.4: births verified and the first like-for-like comparison at tick 10k -- a rich newborn body is NOT worse than the embryonic one and on seed 1 it is BETTER (0.3042 vs the base 0.3399); on seed 2 it is par (0.2897 vs 0.2841). Compute cost measured: 28868 tps (base) -> 580 tps (N=60, 50x slower) -> 141 tps (N=120, 200x slower), so N=60 costs about 7 min per run (all 9 seeds feasible) while N=120 costs about 30 min (point checks). The newborn rich body self-trims 45% of its connections by tick 9.8k while KEEPING all 92 somas -- the biology that sections 80-82 tried to replace with an external tax


---

## Commit cff80ef — Sections 83a + 83b: correlation cardiogram (89 runs) and the causal do(N) test -- 83a REFUTES the hyperplasia hypothesis: body size correlates POSITIVELY with the score (+0.882 pooled, +0.878 within 77, +0.651 within 79b), while the [K1] collision fraction (-0.13), hidden-sign (-0.13) and coherence (+0.27) do NOT predict the score at all (the sanity check holds: body mean L1 gives -0.93). So sections 80-82 were aimed at an ALLY: we starved the body believing it was a tumour. 83b implements the intervention: BIRTH_NODES=N makes the body be born with N nodes, built by the engine's own growth code (same tissue, same silent candidates), pure section-77 physics, all metabolic brakes off; guard byte-identical (0.0807), cargo test = 57/57. First run (seed 2, target 120): born with 121 nodes / 92 somas / 1253 connections, and it immediately resorbed 1110 connections (45%) by tick 9.8k -- remodelling never seen in small bodies. Compute cost: ~75 min per 250k-tick run at N=120 (10x slower), so the plan is N=120 on seed 2 + N=60 on seeds 1/2/4, then the full ensemble for the winning N. Decision rule pre-registered: hypothesis A (size is causal -> engineering lever) vs hypothesis B (correlation was a selection artefact -> population selection)


---

## Commit 2ece87c — Section 82.7: VERDICT on 82b -- ALL THREE pre-registered criteria FAILED (seed 4 lost the floor: -1.3% vs +39.8%; seed 1 ceiling still down: +26.5% vs +61.8%; bud survival 1/168 = 0.6% vs the >=20% target). Medians on these three seeds: section 77 = +67.6%, 79b = +13.1%, 82 = +26.4%, 82b = -1.3%. This is the SEVENTH consecutive attempt (64a, 69, 71, 73, 75, 79, 80, 82) with the same signature: the mechanism moves, the score reshuffles. Conclusion: structure is controllable (18-58 nodes vs 119, zero revivals, credit distribution, energy budget) but the ensemble score remains a lotter of developmental trajectory -- section 82 healed seed 4 by +28.8 points and 82b, changing only the FAIRNESS of the judgement, lost those same 41 points. Therefore the next experiment must target SELECTION itself (population of bodies per seed, or an explicit correlation check between any structural metric and the score), not another neural/physical knob. Everything gated, default historical, guard 0.0807, cargo test = 57/57


---

## Commit c078a1e — Section 82b: probation length from the body's OWN error-drift + judgement on the BEST achievement during the trial (a long window cannot be judged by one final sample). Guard byte-identical (0.0807), cargo test = 57/57. MEASURED on seed 2: -4.4%, body 18.0 nodes, acceptance only 1/168 -- the window stayed at the 182-tick floor because THIS body drifts ~8 of its own noise steps per window, so 'time to see a change' = 1 window. Lesson: the drift rule measures the OBSERVER's visibility timescale, while the exam needs the NEWBORN's maturation timescale -- different quantities (observer property vs cell property). Next step (82c): use the clock the engine already has -- NodeGene::age_ticks vs evol.juvenile_ticks (the body's own biological clock for when a cell stops being young), so the trial ends when the newborn has itself become an adult cell by the body's own standards, plus one measured improvement epoch


---

## Commit 16090d0 — Section 82.4/82.5: full 3-seed picture -- tentative sprouting RAISES THE FLOOR but LOWERS THE CEILING: seed 4 heals from +11.0% to +39.8% (+28.8 points, the stuck dead-ends no longer accumulate) while seed 1 falls from +61.8% to +26.4% (-35.4 points, useful cortex cannot prove itself in 182 ticks). Structure goal achieved on all three (34.8-58.1 nodes vs the 119-node tumour, zero revivals). Cause of that exact shape: the window is the same 'too-short exam' -- it filters harmful growth and equally kills useful growth. Section 82b pre-registration: tie the probation length to the body's OWN learning scale (time for the error to drift by k * its own noise floor, measured from window_err_history); three joint criteria: seed 4 >= +39.8%, seed 1 >= +61.8%, body <= 60 nodes with an acceptance rate >= 20% (now 4.5-11%)


---

## Commit 9f947b0 — Section 82: TENTATIVE SPROUTING -- new tissue gets a probation window (2 x the task's own cycle length, zero hardcode) and is judged on MARGINAL value against the pre-birth error, using the engine's own measured noise floor (improvement_epsilon.max(2*window_noise)); failure amputates the snapshot back and sets a refractory cooldown. Gated by TENTATIVE_GROWTH=1, default byte-identical (guard 0.0807), new test, cargo test = 57/57. MEASURED on seed 2: the size goal is ACHIEVED (34.8 nodes vs the 119-node tumour, 0 revivals) but the score FELL below section 79b (-6.7% vs +13.1%): 191 of ~200 trials were rejected because the 182-tick window is orders of magnitude shorter than the body's own learning timescale (50-100k ticks), so almost every innovation is killed before it can pay off -- the window must be tied to the body's measured learning scale, not to the stream's cycle length


---

## Commit b8d31b5 — Section 81.3: second budget profile (seed 2, the COLLAPSE, +13.1%) -- ROI is POSITIVE AGAIN (x1.7): a node costs 2.64e-3 E/tick and brings 4.44e-3. The budget does NOT discriminate the good seed from the collapsed one (both profitable; the collapsed one is even richer per node at 4.44e-3 vs 3.27e-3). Therefore: the 'the tumour is unprofitable' hypothesis is REFUTED, and the diet can only move a body from rich to less-rich (no behavioural change) or to starved (the strong dose) -- there is no intermediate regime where the tax selectively takes margin from bad tissue. Directive for the next step: the size regulator must be NON-ENERGETIC (champion selection policy / metric / plasticity dynamics) or the economy must be rebalanced by an order of magnitude


---

## Commit 88c9720 — Section 81.2: FIRST BUDGET PROFILE (seed 1, mild diet, +71.6%) -- growth PAYS FOR ITSELF: a node costs 1.49e-3 E/tick and brings 3.27e-3 (ROI x2.2); income 100417E vs costs 49098E; the bank saturates at max_capacity 1000E, so the 20x surplus simply vanishes -- which is exactly why no tax ever bit (to bite it had to starve, i.e. the strong dose). Flux dominates costs (32.6k of 49.1k) with cortical flux 2x the reflex; growth is only 2.8%. Consequence: energy does NOT limit growth, so the size regulator must be non-energetic (selection policy/metric) or the economy must be rebalanced by an order of magnitude


---

## Commit 63fc3f8 — Section 81: ENERGY BUDGET TELEMETRY -- passive bookkeeping counters in the metabolism (income gross/tithe/penalty, basal, wire, spike, flux reflex/cortical, dormant, growth) plus a BudgetSummary with costs/margin/upkeep-share, and a diagnostics report that answers the ROI question per node-tick. Counters are pure measurement: a test proves the energy trajectory is identical with and without them and that the decomposition is exact. cargo test = 56/56, physics untouched


---

## Commit 3388d7c — Section 80.12: the INCOME TITHE SATURATES -- at N/N_ref ~ 4-9 the share beta*N/N_ref (beta=0.5) hits the 0.9 cap, so the body hands over 90% of its income: seed 4 shrinks to 55 nodes but suffers 60 revivals (energy 43-60E) and stays at +11.0%. All three pressure designs (tax, suppression, tithe) hit the same wall: we set PRESSURE without knowing the body's ENERGY BUDGET (income/tick vs cost/tick) -- that ratio, not size, makes the dose per-seed. Next step is therefore a MEASUREMENT (telemetry-only): budget per tick for champion vs live trajectory, then calibrate the dose from the body's own budget so that a margin turns negative only when it grows WITHOUT income growth


---

## Commit 105f3fe — Section 80.11: dose/suppression MATRIX measured -- the growth suppression does NOT heal the collapses (seed 2 mild+suppress = +13.1%, seed 4 strong+suppress = +11.0% even though its body DID shrink to 56.8 nodes with zero revivals), and the dose sensitivity is PER-SEED (seed 1 needs mild, seed 2 needs strong), so no fixed dose can win both. Hence the income tithe: a self-scaling cost that needs no global dose choice. Tithe run (beta=0.5 + mild diet + suppression) launched on seeds 4, 2, 1


---

## Commit 2fec102 — Section 80 step 2b: INCOME TITHE -- the body pays a SHARE of every reward for its own size (share = beta * N / N_ref, capped at 0.9), instead of an absolute charge that either goes unnoticed (3% of income at alpha=0.1) or starves a good body (alpha=1.0). A runaway tumour (N/N_ref ~ 9) hands over ~90% of its income and starves ITSELF; a compact body keeps almost everything. Neutral by default (share=0 -> byte-identical reward), behind DIET_TITHE. New test: neutrality + binds + cap. cargo test = 55/55. Diagnosis of why the suppression alone did not shrink the body: the threshold 0.5*trivial_error is only crossed after ~100k ticks, by which time most somas are already grown -- the missing mechanism is the REVERSE path (shrinkage pressure), not the growth crank


---

## Commit 777ee0b — Section 80 step 2: RELATIVE GrowthDrive suppression -- grow only while the body has not yet beaten its OWN measured bar with margin (window_err > 0.5 * trivial_error, both measured by recalibrate, so the threshold is dataset- and scale-free). Gated behind GROWTH_SUPPRESS=1, banner shows the state. Measured so far: the mild dose is a NO-OP on the collapsed seeds 4 and 6 (+11.0% and +14.7%, identical to 79b, energy min ~99E, zero revivals) while the strong dose heals seed 2 but starves seed 1 -> the dose is seed-dependent and must be tied to the body's own economics


---

## Commit db9e755 — Section 80 step 1 MEASURED: the diet heals the collapsed seeds (seed 2: +13.1% -> +67.6%, +54.5 points) but the DOSE is decisive -- strong (alpha=1.0, x3) starves the body (energy 327-674E vs 1000E) and drops seed 1 to +19.1%, while mild (alpha=0.1, x1.5) gives the best seed-1 result of the session (+71.6%) with ZERO revivals and mean L1 0.1937 < 0.2243. Honest: the structural marker (<=60 nodes) was NOT met (85-94) -- the diet worked through selection dynamics, not by literally shrinking the body. Dose knobs DIET_ALPHA/DIET_SURCHARGE added; cargo test = 54/54


---

## Commit e27c2fc — Section 80 step 1: METABOLIC DIET -- allometric (superlinear) upkeep scaled by the body's OWN measured periphery (sensor count, not a human-chosen N0) + cortical flux surcharge (soma-sourced traffic costs more than the direct reflex). Neutral defaults (alpha=0, surcharge=1.0) reproduce the historical math byte-for-byte (guard: seed 1 = 0.0807), behind DIET=1. New test: neutrality + superlinearity + x3 surcharge. cargo test = 54/54


---

## Commit 576b86d — chore: stop tracking the scratch build directory target-v2 (used to build v2 in parallel with a locked release binary)


---

## Commit 837d045 — Section 79c: v2 VERDICT measured separately -- (1) instant motor is a NO-OP (byte-identical to 79b: the body has essentially no delayed motor edges) and (2) the static-chain skip is HARMFUL (hidden credit 35.1% -> 0.3%, score +61.8% -> +12.5%). Final section-79b ensemble (n=8): median +47.27% but SPREAD 72.2 points vs section 77's 39.6 -- hyperplasia confirmed as the wall; section 80 plan drafted


---

## Commit b2c11fe — Section 79b ensemble (5 of 9 seeds): median +40.67% vs section 77's +56.36% on the same seeds -- criterion 4 FAILED; the key finding is the SPREAD (58.3 vs 25.0 points): the fixed credit re-shuffles the lottery instead of raising the bar, which is exactly what section-79 v2 targets


---

## Commit 77efc77 — Section 79 v2 (auditor's plan): INSTANT ACTUATOR + STATIC CHAIN -- IMPLEMENTED, GATED, MEASUREMENT PENDING

WHY IT IS PHYSICS, NOT A HACK. The auditor's argument: the output to a motor must be INSTANT.
Delays are needed only for CONTEXT RECOGNITION (sensor->soma, soma->soma); once a soma has
recognised a bigram it hands the command to the muscle in the SAME tick. With `conn.to == Motor =>
delay = 0`:
1. the motor's error arrives at the soma instantly, in the current tick t;
2. the soma's incoming synapses update by the ALREADY VERIFIED one-step rule of section 62:
   dw = eta * pre_A(t - tau) * sigma'_B(t) * err_B(t);
3. the multi-hop time-alignment problem DISAPPEARS -- no BPTT buffers at all.
This is a physical property of the actuator (a muscle has no tau-tick memory), universal for any
stream; it forbids no topology, only the latency of the LAST edge.

IMPLEMENTATION (behind INSTANT_MOTOR=1, default OFF):
1. BrainGenome::motor_instant -- a property of the BODY (not a global flag), serde-defaulted.
2. BrainGenome::effective_delay(motor_instant, to_is_motor, delay) -- the single point of truth used
   by BOTH the delayed tract and the instant cascade, so the rule also holds for checkpoints born
   before v2.
3. Edge birth (mutator.rs): an edge that ENTERS a motor is born instant.
4. The chain (node_errors): `if motor_instant && delay > 0 { continue; }` -- the static error does not
   flow back through a delayed edge (a delayed edge carries the signal into the FUTURE, so the current
   error cannot explain it). The edge's own weight still learns -- from node_errors[to] and pre(t-tau).

PRE-REGISTRATION (one extra metric on top of the four criteria of 78.6): the ensemble SPREAD must
FALL -- median >= +67.6% (criterion 4) AND a min-max spread <= 25 points (in section 79b it is 49
points over 3 seeds). Physical meaning: if the chain's last tick is aligned, the body must not
"win or lose the lottery" depending on which shape it happened to grow.

WHY v2 IS NEEDED: the interim section-79b ensemble (3 of 9 seeds: +61.85 / +13.14 / +40.67 against
section 77's +51.59 / +67.61 / +45.17) gives median +40.67% vs +51.41% on the same seeds -- criterion 4
FAILED. The mechanism became correct (sign 66-82%, credit distributed: hidden static 35.1% of the
gradient norm, motor-temporal anti-phase gone) but the score did not follow; section 77 had a 22-point
spread on those seeds, section 79b has 49 points. The new physics re-shuffled the lottery instead of
raising the bar -- exactly what v2 is meant to fix by removing the last misaligned tick.

CODE: compiles (cargo check --examples clean), cargo test = 53/53 with the feature OFF (baseline
untouched). Measurement runs will follow once the background section-79b sweep releases the binary.

Code + doc in this step; numbers pending.

---

## Commit 85bcf80 — Section 79 v2 plan CORRECTED: queues for pre*sigma' would mix the firing tick with the arrival tick (auditor's point); the 79b fix already places each factor on its own tick -- what remains is the MULTI-HOP credit, not a factor queue


---

## Commit 16157b7 — Section 79b: AUDIT OF COMMIT 8e68764 -- THE BUFFER BUG AND THE FIX THAT RESTORED THE PHASE

79.5 THE BUG. The auditor found the defect in the buffer maintenance (brain.rs:1091 of 8e68764):
    if self.output_hist.len() != self.nodes.len() { clear(); resize(nodes.len(), empty); }
`output_hist` is a VecDeque<Vec<f32>>, so output_hist.len() is the TIME DEPTH (<= 8), not the soma
count. Comparing it with nodes.len() (59-118) was TRUE EVERY TICK, so the buffer was cleared every
tick, rows for tau >= 2 never existed, and `unwrap_or(pre_now)` SILENTLY fell back to the historical
credit. So the section-79 v1 measurement (and its "anti-phase") concerned tau == 1 ONLY, while ~40%
of the grown delays (1..5) behaved as 0.
FIX (as the auditor proposed): check the WIDTH of the last row instead of the time depth; only a
structural body change (growth/resorption reassigns node ids) may legitimately drop the history.
SO THAT THIS CLASS OF DEFECT CAN NEVER BE INVISIBLE AGAIN: a counter `delay_credit_fallbacks` (how
often the delayed credit could not read the history) is printed as "history read misses per run",
plus TWO regression tests: test_section79_delay_history_buffer_survives_ticks (after warmup the misses
must be exactly 0; depth 8, width = soma count) and
test_section79_delay_credit_keeps_linear_alphabet_convergence (criterion 3 IN THE ENABLED MODE -- the
canonical task must still converge below 0.05). cargo test = 53/53.

79.6 MEASUREMENT AFTER THE FIX: THE ANTI-PHASE IS GONE AND THE HIDDEN LAYER STARTED LEARNING.
Seed 1: +61.8% (0.0636) against section 77's +51.6% (0.0807) -- +10.2 points. Seed 9: +40.7% (0.0989)
against +45.2% (0.0914) -- -4.5 points.
Learning distribution per section 65.3 (seed 1, credit ENABLED) vs section 77: motor/static +0.017 with
61.2% of the gradient norm (was 98.9%); motor/TEMPORAL +0.279 with 3.7% (was +0.002 / 0.1%);
hidden/static -0.001 with 35.1% (was 0.8%); hidden/temporal -0.079 with 0.0%.
1. THE MOTOR-TEMPORAL ANTI-PHASE IS GONE: it is now +0.279, the SAME sign as the static +0.017, and
   those synapses became the LARGEST movements in the body.
2. THE CREDIT IS FINALLY DISTRIBUTED: the hidden static layer receives 35.1% of the gradient norm
   (was 0.8%); RMS|dW| hidden/motor = 1.759 (was 0.391).
3. HISTORY MISSES: 807 (seed 1) and 1023 (seed 9) per 250k ticks -- only the 8-tick warmup after each
   structural change, not a hidden degradation of tau >= 2 into tau = 0.
CRITERIA 78.6 (preliminary, 2 seeds): sign >= 80% -- 66.3% (seed 1) FAIL, 81.9% (seed 9, was 52.2%)
PASS; mean L1 < 0.2243 -- 0.2073 (seed 1) PASS, 0.2867 (seed 9) FAIL; guard test in the enabled mode
-- 53/53 PASS; 9-seed median -- still open.

79.7 STATUS. The mode stays behind the switch (DELAY_CREDIT=1) because criterion 4 (the ensemble
median) is not closed yet; the default without the variable is the historical physics, and the guard
run confirmed it byte-for-byte (0.0807 / +51.6%). Seeds 2-8 are being collected by
target/sweep79b.ps1.

Core change (gated, default OFF), two new regression tests. cargo test = 53/53.

---

## Commit 8e68764 — Section 79: DELAY-AWARE CREDIT (pre(t-tau)) -- TRIED AND REVERTED (the mechanism moved, the score fell)

79.1 WHAT THE CODE AND SECTION 65.3 SAID. The backward pass (brain.rs:886-922:
credit = err_to * conn.weight * slope, accumulated as node_errors[from] += credit * 0.5) does NOT
read delay_ticks at all, and the update (brain.rs:975: delta_w = eta * pre * slope_to * eff_err) uses
the CURRENT tick's pre. But the signal of a synapse with delay tau reached its target tau ticks ago,
so the error it is being blamed for was produced by the SOURCE's output tau ticks earlier. Section
65.3 confirmed this with numbers (seed 1, section 77): motor/static dW = -0.140 with 98.9% of the
gradient norm; motor/TEMPORAL +0.002 with 0.1%; hidden/static -0.156 with 0.8%; hidden/TEMPORAL
+0.069 with 0.2%. Almost all learning lives at delay == 0; the temporal ones (which is where
context/bigrams live) are dead AND anti-phase.

79.2 IMPLEMENTATION (conserving, mode-gated). (1) BrainGenome::output_hist -- a ring buffer of soma
outputs (depth 8; the largest delay the mutator ever grows is 5), serde-defaulted so old checkpoints
load and refill within 8 ticks. (2) For a synapse with delay tau the factor `pre` is taken from tick
t - tau; the ERROR IS NOT SPLIT between channels (section 75's lesson) -- it is only placed on ITS OWN
tick, so the credit stays conserving. (3) LearningContext::delay_credit (default FALSE = historical
physics) plus a DELAY_CREDIT=1 switch in the example. GUARD: the control run (delay_credit = false)
reproduced exactly the historical 0.0807 / +51.6%, proving the switch really disables the new physics
and the whole project history stays byte-for-byte reproducible.

79.3 RESULT: THE MECHANISM MOVED, THE SCORE FELL. Seed 1: +51.6% (0.0807) -> +14.1% (0.1431), -37.5
points. Seed 9: +45.2% (0.0914) -> -5.7% (0.1762), -50.9 points. Seed 4: +70.2% (0.0497) -> +59.7%
(0.0671), -10.5 points. Three seeds out of three WORSE (section-79 median about +14.1% against
section 77's +67.6%) -- preregistered criterion 4 failed. The section-78 ablation on seed 1 confirmed
layer 2 still carries the answer (live 0.1552 vs direct-only 0.4101, -62.2%), so the change did not
"switch off" the cortex -- it made it worse. The body also ballooned (59 -> 118 nodes).
WHY (the key point): the temporal share of the gradient norm did rise x18 (0.1% -> 1.8%), but the
ANTI-PHASE REMAINED: motor/static +0.008 vs motor/temporal -0.004, hidden/static -0.010 vs
hidden/temporal +0.014. Only the `pre` factor was fixed, while the CREDIT side (the target error and
sigma' inside the chain) stayed blind to the delay -- so we AMPLIFIED A HALF-TRUTH: we did not cure
the direction, we scaled up the wrong direction.

79.4 REVERTED + PREREGISTRATION FOR v2. The default is back to historical physics
(delay_credit = false); the machinery stays behind the switch for experiments. One-line lesson: A
HALF-FIXED CREDIT IS WORSE THAN AN UNFIXED ONE, because it scales the error without changing its sign.
SECTION 79 v2 -- make the WHOLE CHAIN delay-consistent, not just one factor. Since bus.send(to, val,
tau) means "left at t, arrived at t+tau", the stimulus/consequence pair is pre(t - tau) with the error
of the tick when the consequence appeared (done). For the inner chain we need a DELAYED UPDATE: the
factor pre * sigma' * eta/(1+mass) is queued on the synapse and applied tau ticks later, together with
the error that has then arrived -- so sigma' is evaluated at the very tick the soma actually emitted,
and the anti-phase disappears PHYSICALLY rather than by fitting. Pre-registration stays the same (the
four criteria of 78.6) PLUS two mechanism metrics: (a) the temporal sign must BECOME THE SAME as the
static one (currently opposite); (b) the temporal share of the gradient norm must reach >= 5%
(currently 1.8%).

Core change, default OFF, guard-verified. cargo test = 51/51.

---

## Commit a53aa1f — Section 78: WHO CARRIES THE ANSWER -- LAYER 2 IS ALIVE (and my section-77.7 hypothesis is refuted by measurement)

78.1 INSTRUMENT (observer-only, 0 core lines): channel ablation on CLONES of the champion body --
turn off one motor channel and evaluate a full cycle with the engine's own measure (median L1), over
3 cycles. Modes: DIRECT-ONLY = all soma->motor disabled; SOMAS-ONLY = all receptor->motor disabled.
Plus the shares of |recent_flux| -- the very quantity the engine uses to measure a synapse's work.

78.2 VERDICT (one-hot). Seed 1: live 0.0990 | direct-only 0.3242 (+227%) | somas-only 0.6621;
flux S->M 13.4%, H->M 6.1%, S->H 28.7%, SOMA->SOMA 51.8%. Seed 9: live 0.0838 | direct-only 0.2390
(+185%) | somas-only 0.3556; flux S->M 8.5%, H->M 3.3%, S->H 10.9%, SOMA->SOMA 77.3%.
THREE FACTS. (1) LAYER 2 CARRIES THE ANSWER: removing soma->motor roughly triples the median error,
so the "the somas are noise" hypothesis is false and section 77 switched nothing off. (2) THE DIRECT
CHANNEL ALONE IS WORSE THAN THE TRIVIAL BAR (0.24-0.32 vs 0.1667): it is not "a table row", it is
tuned TO the somas (cancelling part of their output). This finally refutes my section-77.7 story.
(3) THE BULK OF THE WORK IS soma->soma RECURRENCE (51.8% and 77.3% of the flux, while H->M is only
3-6%): the body computes internally and hands the motor a finished result.

CHECK B -- TIME VS CONVERGENCE: DOUBLING THE TIME GIVES NOTHING. Seed 1: 0.0807 at 250k and 0.0807
at 500k (+51.6% both). Seed 9: 0.0914 and 0.0914 (+45.2% both). The per-epoch body trend over ten
50k epochs (seed 1): 0.3432, 0.3340, 0.3296, 0.3359, 0.3326, 0.3458, 0.3416, 0.3369, 0.3363, 0.4164
-- NO TREND over half a million ticks. So this is not "not enough time": the surface is frozen, the
preregistered ">=15% means annealing" branch did NOT fire (0.00% << 5%) -> the lever is Layer 2
(credit or representation). Bonus determinism check: 500k and 250k give the same champion error to
the fourth decimal, and the ablation on the champion to the third (0.0990/0.3242/0.6611 vs
0.0990/0.3242/0.6621).

CHECK C -- DID ONE-HOT "SWITCH ON" THE SOMAS? NO, THE STRUCTURE OF THE SOLUTION IS THE SAME.
Linear (section 62 base), seed 9: live 0.1565 | direct-only 0.2841 (-44.9%) | somas-only 0.3801;
flux S->M 2.0%, H->M 17.4%, SOMA->SOMA 53.5%. One-hot: live 0.0838 | direct-only 0.2390 (-64.9%).
The somas are load-bearing in BOTH encodings, so section 77 did not "enable" the cortex -- it
strengthened what already worked and gave the direct channel more weight (2.0% -> 8.5% of the flux).

78.5 WHERE IT FROZE, AND THE "THE METRIC IS TO BLAME" HYPOTHESIS IS RULED OUT UP FRONT. On the same
input (section 41.1, seed 1): the table "input -> conditional median" reaches median L1 0.0000 and
MEAN L1 0.2243; the constant bar is 0.1667 / 0.3315; the body is 0.0807 / 0.3075. The table beats the
body ON BOTH MEASURES, so the hypothesis "the engine optimises the MEAN while the criterion is the
MEDIAN, so it wastes capacity on the unpredictable transition" FALLS: even on the mean the table
(0.2243) is better than the body (0.3075). The body sits in a LOCAL OPTIMUM that is worse than what
is reachable on both measures -- the lever is the CREDIT INSIDE LAYER 2, not the choice of metric.

78.6 PREREGISTRATION FOR SECTION 79 (Layer 2 credit) -- four checks: (1) hidden-layer sign >= 80%
(was ~60%); (2) body MEAN L1 < 0.2243 (the table's bar) -- otherwise we have not left the local
optimum; (3) the guard test test_linear_alphabet_rapid_convergence (< 0.05) must still pass --
section 75's lesson: any non-conserving credit double-counts and overshoots; (4) the 9-seed median
must not fall below the section-77 level (+67.6%): the change must be an ADDITION to section 77, not
a replacement.

PROPOSAL (a) "1 soma = 1 sensor + delay" IS REJECTED, and not because it is inelegant: (i) it
violates the project's own Bans 2 and 3 (no artificial topology limits, no hardcoded layering) --
it is hand-madetask architecture; (ii) it contradicts the fresh measurement -- 51.8-77.3% of the
flux runs through soma->soma and receptor->soma gives only 10.9-28.7%, so a single-input soma could
do neither recurrence nor combination (we would kill exactly the link that carries the answer);
(iii) detector collisions ([K1]: 9 of 16) do not stop the somas from being useful (the ablation
proves it), so collision is NOT the binding constraint. In short: (a) would cure the symptom at the
price of the disease. PROPOSAL (b) a clean error signal to the somas is acceptable IN PRINCIPLE
under two conditions: the credit must be CONSERVING (channels sum to e) and check B must have passed
(it has: the surface is frozen, so credit is a legitimate target).

Instrument only in this step (0 core lines). cargo test = 51/51.

---

## Commit 2158030 — Section 77.8: control run with the fixed observer -- engine score byte-identical (0.0807), corrected section-41.1 oracle reads [M4] TABLE HELPS (+100% median / +32.3% mean headroom)


---

## Commit 5a2572e — Section 77: POPULATION ENCODING OF THE INPUT -- ORTHOGONAL CLASSES INSTEAD OF ONE AXIS

77.1 WHAT WAS BROKEN. cradle/stream.rs:132-137 encodes num_unique independent categories as ONE
scalar (-1.0 + 2*idx/(n-1)). This IMPOSES a metric on the classes: neighbour letters become
neighbour points of a single axis, so neighbour detectors catch neighbour classes and produce
OPPOSITE gradient signs. Section 74 measured it: C = 9.46% (cancellation 11:1) on seed 5 and
C = 19.65% (5:1) on seed 9. That is not a credit problem -- it is a CARRIER problem.

77.2 THE FIX HAS THREE PARTS (all interface, not architecture).
1. ENCODING (2 fields, mode-gated): DataStreamCradle::one_hot: bool + alphabet: Vec<char>
   (characters in order of appearance). Default false -- behaviour is BYTE-FOR-BYTE what it always
   was (guard run: seed 9 = 0.1541 / +7.56% EXACTLY). With true, each lag is presented as K
   ORTHOGONAL lines (K = alphabet size); the number of lag BLOCKS is decided by the body's own
   periphery: blocks = n_sensors / K.
2. THE TARGET STAYS A SCALAR -- therefore the L1 metric, the bar (0.1667), the table oracle and all
   section 40/41/41.1/41.2 reports stay byte-for-byte comparable with the whole history. That is why
   section 77 is shaped this way: we change the CARRIER, not the task.
3. A DIRECT PHYSICAL LINE receptor -> actuator (brain.rs, genesis_multimodal). Without it the embryo
   has NO path from input to output that avoids a NONLINEAR soma (the founding cell is a random
   program), so the output is a monotone function of one number and a class->value table is
   physically unreachable for ANY weights. The added synapse has a RANDOM weight (gene pool) and is
   left for plasticity; the hidden paths are untouched. This is not task architecture -- it is the
   same "minimum conductivity" the function's own comment already declares.

77.3 THE BUKVAR ALPHABET IS 13 CLASSES (the audit was right). data/bukvar.txt has exactly 13 unique
characters: '\n', ' ', А, В, Д, И, Л, М, О, Р, С, У, Ь. So the audit's "13-channel one-hot" is
literally this stream's class count, not an invented number.

77.4 GUARD CHECKS BEFORE THE SWEEP. cargo test = 51/51 (49 old + 2 new section-77 tests: code
orthogonality, and "empty code when there are fewer lines than classes"). Linear mode with the new
binary: seed 9 = 0.1541 / +7.5% -- an exact match with history.

77.6 RESULT (9 seeds, Bukvar, 250k ticks). Seed: one-hot vs base section 62 vs control (13 linear):
1: +51.6 / +0.00 / +8.5 | 2: +67.6 / +4.44 / -7.8 | 3: +51.2 / +0.00 / -2.8 | 4: +70.2 / -13.98 /
-11.5 | 5: +61.1 / +4.50 | 6: +71.8 / -1.26 | 7: +84.8 / -1.80 | 8: +75.0 / -1.20 | 9: +45.2 / +7.56.
ENSEMBLE (n=9): median +67.61%, mean +64.27%, min +45.17%, max +84.76% -- ALL NINE positive, all
>= +45%. For comparison the base over the project history gave median +0.00% / mean -0.19% / max
+7.56%; section 73 (global loudness) gave median -3.18%. PREREGISTERED CHECK 3 (median >= +5%): PASS
with a 13x margin.

77.7 WHAT ACTUALLY DECIDED IT -- AND WHAT MY PREREGISTRATION FAILED TO PREDICT (honestly).
Check 3 PASS (13x margin); check 4 PASS (the gross motion SUM|G_indiv| did not fall -- it GREW 8x to
60x, from 9.3e-4..9.1e-3 to 1.4e-2..6.0e-2); check 1 FAIL (C mean 20.6%, max 61.3% on seed 4, not
>= 50%); check 2 FAIL (hidden sign mean 62.4%, range 52-72%, not >= 80%).
THE MECHANISM IS NOT THE ONE I PREREGISTERED. I predicted one-hot would raise the bells' coherence
to >= 50%. It rose (9.5-19.7% -> 20.6% mean; 61.3% on seed 4) but did NOT reach the threshold, and
the hidden-layer sign stayed ~60% (detector collisions did NOT disappear: telemetry [K1] still sees
9 of 16 detectors with contradictory motor requirements). The bells did not "coordinate" -- and they
are not what won.
THE WIN CAME THE OTHER WAY, the one that is a mathematical identity rather than a correlation: with
orthogonal class codes the DIRECT channel (sensor -> motor, added in 77.2 item 3) becomes a TABLE
ROW: y = w_k * 1 = w_k. Since w_k is learned by the delta rule on the FULL error, that single link
CAN express "class -> conditional mean", which no weight could do on the linear code. The bells
simply became UNNECESSARY for this task, and their low coherence stopped being the wall.
THE CONTROL CLOSES THE ONLY ALTERNATIVE. The same periphery (13 lines from birth) but LINEAR coding
gives median about -5% (seed 1 +8.5%, seed 2 -7.8%, seed 3 -2.8%, seed 4 -11.5%) -- so the gain is
NOT from the number of sensors but from the orthogonality of the code.

INSTRUMENT ARTEFACT FOUND AND FIXED: section 41.1 bucketed input classes by oracle.sens[0]; under
one-hot that is merely a "class 0" bit, so "the table does not help" was a ruler artefact, not a
fact. The class key is now the hash of the WHOLE receptor vector (mode-conditional: on the linear
path the old numbers stay byte-for-byte identical).

77.8 STATUS. KEPT IN THE CORE (not reverted): encoding, direct line, observer fix. The mode is off by
default, so the whole project history stays reproducible byte-for-byte.

Instrument + interface in this step. cargo test = 51/51.

---

## Commit 33deaa1 — Section 74: "COORDINATION GAIN" MEASUREMENT -- PLATEAU REFUTED (but CANCELLATION found) Section 75: Residual Credit Decoupling -- IMPLEMENTED AND REFUTED BY THE GUARD TEST IN 10 SECONDS

74.1 WHAT WAS MEASURED. On every probe of a live-brain snapshot, for all synapses whose SOURCE is a
HIDDEN soma (i.e. bell outputs into a motor):
  * sum |G_indiv| -- the sum of absolute loss changes from INDIVIDUAL shifts w_i -> w_i + eps;
  * sum G_indiv (signed) -- the same sum WITH signs (first order);
  * G_joint (signed) -- the loss change from shifting ALL of those weights BY eps SIMULTANEOUSLY
    (a shared clone run, run_many).
Two honest metrics follow: A = G_joint / sumG_indiv (ADDITIVITY: |A-1| <= 0.5 means the surface is
LINEAR, no synergy, no plateau in the strict sense), and C = |sumG_indiv| / sum|G_indiv|
(COHERENCE: how much the individual moves AGREE).
Trap the instrument first showed as "gain 8.83x": the ratio G_joint/|sumG_indiv| is NOT a gain -- it
inflates automatically when the individual moves cancel each other. The report now prints A and C.

74.2 RESULT (seed 5 and seed 9, 250k, live snapshot).
seed 5: sum|G_indiv| = 9.341e-4, sumG_indiv = +8.841e-5, G_joint = +8.855e-5, A = 1.002,
        C = 9.46% (cancellation 11:1).
seed 9: sum|G_indiv| = 9.073e-3, sumG_indiv = -1.783e-3, G_joint = -1.040e-3, A = 0.583,
        C = 19.65% (cancellation 5:1).
VERDICT: THE COORDINATION PLATEAU IS REFUTED. The shared move of the bell weights equals the SIGNED
SUM of the individual ones (A = 1.00 on seed 5 to three decimals; 0.58 on seed 9 with mild
second-order saturation). So MY OWN SECTION 73.4 HYPOTHESIS WAS WRONG: the surface is not "flat in
every coordinate" -- it is ADDITIVE, and a step-wise rule is in principle able to descend it. The
audit's "coordination plateau" hypothesis is likewise NOT confirmed by measurement.
WHAT WAS FOUND INSTEAD, and it is stronger: the individual bell gradients CANCEL each other -- the
gross motion sum|G_indiv| yields only 9.5-19.7% net (5:1 to 11:1 cancellation). Every bell has its
OWN strong gradient (this refutes my "contribution is about 0" as well), but the signs are
contradictory, so the aggregated error signal is 5-11x weaker than the total motion.
The cause is exactly what the audit named (Part 1, item 1), and it is VISIBLE IN THE CODE:
cradle/stream.rs:132-137 compresses num_unique independent categories into ONE scalar
(-1.0 + 2*idx/(n-1)). Adjacent classes sit on adjacent points of a single line, so adjacent bells
catch adjacent classes and give OPPOSITE signs -- hence cancellation, not flatness.

74.3 SECTION 75 (Residual Credit Decoupling) -- IMPLEMENTED AND REFUTED BY THE GUARD TEST.
Implementation (75a, split the ERROR): the direct channel (sensor->motor) plus the motor bias were
declared the BASIS; y_base was measured on a clone of the motor through the real path
(base_output_only: update(base_net) -> compute_output()); the direct channel and motor bias received
the FULL error, the hidden layer the RESIDUAL e_res = tgt - y_base.
RESULT -- POSITIVE FEEDBACK, NOT DECOUPLING: the first version (subtracting the basis in output
units via *input_sensitivity()) gave final_avg_err = 0.957 against a 0.44 baseline; the corrected
version (exact y_base through a clone) gave 0.493 -- still a failure. The EXISTING guard test
test_linear_alphabet_rapid_convergence (requires < 0.05) failed.
THE REASON IS MATHEMATICAL, NOT ENGINEERING: y = basis + hidden, therefore e = tgt - y IS ALREADY the
residual for BOTH channels. Giving the basis the full error and the hidden layer the residual counts
the basis TWICE: the hidden channel starts carrying the whole target (e_res ~ tgt while the basis is
still zero) and the total output overshoots.
REVERTED (git checkout), the base reproduced, cargo test = 49/49. LESSON: the guard test on the
canonical one-step linear task caught an architectural error that the ensemble score might never have
shown (0.44 -> 0.49 looks like "noise", not like "double counting").

74.4 SECTIONS 76-77 -- WHAT TO DO NEXT, AND WHY EXACTLY THIS.
The audit's order (75 first, 77 only later) had to be REVERSED -- on the basis of measurement:
section 74 showed the wall is neither coordination nor the DC component of the error, but the
CANCELLATION of adjacent bells' signs, which is produced by the 1D SCALAR ENCODING of categories
(stream.rs:132-137). Credit cannot help here: the cancellation arises in `pre` (the bells'
activations), not in the error.
WHAT THE ENGINE ALREADY HAS (verified by reading): multi-channel input IS ALREADY PLUMBED --
step.inputs is a Vec<f32>, and BrainGenome::sensor_ids + step_multi_io(&step.inputs, ...) feed each
number to its OWN sensor port (the diagnostics already do this). So section 77 is a replacement of
one inputs.push(scalar) by a one-hot over num_unique lines (about 10 lines in stream.rs), plus
re-measuring the bar (the table oracle on one-hot is a different "trivial strategy").
PRE-REGISTRATION for section 77: (1) the coherence C from section 74 must rise from 9.5-19.7% to at
least 50% (this IS the "category decoupling" metric); (2) hidden-layer sign agreement at least 80%;
(3) ensemble score median at least +5.0% against the RE-MEASURED bar; (4) the gross motion
sum|G_indiv| must not fall below about 30% of the current value (otherwise we merely switched the
bells off instead of coordinating them).

Instrument (diagnose.rs) only in this step. cargo test = 49/49.

---

## Commit d47de38 — Section 73: GLOBAL loudness matching -- TRIED AND REVERTED (but the calibration finally landed)

73.1 IMPLEMENTATION: two ORGANISM-WIDE (not per-soma!) EWMA credit scales -- one for synapses
targeting a MOTOR, one for those targeting a HIDDEN soma. The hidden channel's step is multiplied by
the RATIO of these scales (with a relative floor so a channel with no history cannot get an unbounded
boost); for motor targets the multiplier is exactly 1.0 -- the motor is the loudness reference.
Section 69 exploded because it normalised by EACH SOMA'S OWN scale; here the scale is SHARED.

73.2 THE FOUR PRE-REGISTERED CHECKS -- 2 OF 4.
(1) RMS|dw| hidden/motor: target ~1.0; measured 0.796 mean (0.110...1.199) -- PASSED.
(2) hidden-layer sign agreement: target >=65%; measured 73.5% (68.9...79.1%) -- PASSED.
(3) score median: target >= +0.00%; measured -3.18% (was +0.00%) -- FAILED.
(4) downward Section 54 trend: target >=3 seeds; measured 2 of 9 -- FAILED.
HISTORIC WITHIN THIS STEP: the hidden credit became UNIFORMLY RELIABLE ON ALL 9 SEEDS -- 68.9-79.1%,
NOT A SINGLE COIN FLIP (Section 62 spanned 56.5-82%, Section 71 spanned 32-91%). And the calibration
landed: 0.80 against a target of 1.0 -- WITHOUT the 3.66 overshoot of Section 69.
REVERT: everything removed (git checkout -- the Section 62 base is reproduced BYTE-FOR-BYTE: seed 9 =
0.1541 versus the bar 0.1667 = +7.56%). cargo test = 49/49.

73.3 SYNTHESIS OF THE FOUR ATTEMPTS -- AND THE CONCLUSION THAT FINALLY CLOSES THE CREDIT LINE.
Section 64a (slope convention in the chain): hidden gradient share 0.8% -> 15%, score median -10.50%.
Section 69 (normalising by each soma's own scale): RMS|dw| 0.1 -> 3.66 (overshoot), score -13.20%.
Section 71 (one correct traversal, Jacobi): sign 64% -> 72%, score -1.68%.
Section 73 (global loudness matching): calibration AND reliability -- 0.80 and 73.5% with every seed
above 68% -- score -3.18%. THE HIDDEN LAYER'S CREDIT CAN NOW BE MADE BOTH RELIABLE (69-79% on every
seed) AND CORRECTLY CALIBRATED (0.80) -- AND THE SCORE STILL DOES NOT FOLLOW. So THE REMAINING WALL
IS NOT THE CREDIT.

73.4 SECTION 74 -- the last hypothesis, and it follows naturally from everything measured: THE
COORDINATION PLATEAU. Why does the reader not use the bells even though their credit is now reliable?
Because the answer requires a SIMULTANEOUS move: each bell is 1 of 13 classes, so its own
contribution to reducing the error is about ZERO (flat in every individual coordinate), while the
error falls only along the SHARED DIAGONAL (all the needed weights together). A step-wise rule that
moves synapses ONE AT A TIME sees a flat surface and CANNOT CROSS THE PLATEAU -- whereas the table
oracle takes +16.6% precisely because it makes a NON-CONTINUOUS (class-based) move: it reads the
class median rather than a per-coordinate gradient.
SECTION 74 (measurement, 0 core lines): measure the COORDINATION GAIN -- the loss change along a
SHARED shift (all bell-to-motor weights +delta at once) versus the change from shifting EACH weight
alone. If the shared shift produces a large drop while each individual one produces about zero, the
plateau is confirmed, and then the right fix is a ONE-SHOT FIT OF THE READOUT when a bell is born
(what imprint_newborn attempted in Section 47, and what the table does by construction).

Instrument + log only. cargo test = 49/49. This is the ninth core change tried and reverted.

---

## Commit 13e8d38 — Section 72 CORRECTION: the task is NOT empty -- headroom +16.6% (mean) / +100% (median), and the body leaves it on the table

72.5 WHAT I GOT WRONG (and what the measurement actually showed). My "the task is empty" conclusion
is REFUTED by the very run I quoted. In those same logs there is the TABLE oracle ("input ->
conditional median" -- i.e. the maximum that can be taken from the current input at all):
"TABLE (nonlinear maximum of the input) beats the constant: median +100.0% | mean +16.6%" and
"[M4] THE TABLE HELPS: the median measure has +100.0% of headroom that linear models and the current
body DO NOT take". IDENTICAL ON ALL 9 SEEDS (the table is built on the STREAM, not on the body -- it
is a property of the task). So: the task is NOT empty and is fully solvable -- a mean gain of +16.6%
and a median gain of +100% (more than half of the test ticks are predicted EXACTLY by a table on the
current input); but a LINEAR model takes nothing (lag oracle depth 1: +0.0%) because the mapping
"symbol -> next symbol" is NONLINEAR (in the text "МА МА МО МУ МИ..." the letter М precedes both А
and О, so a LOCAL detector feature is needed, not a line); and MEMORY is not needed (depths 2-8:
+0.2...1.9%), because the target is already determined by the current input.

72.6 THE REAL WALL -- A CREDIT ASYMMETRY BETWEEN TWO PATHS (a local minimum). Putting two
measurements together: the task's headroom (table oracle) +16.6% mean / +100% median; the hidden
layer's credit versus the direct layer (Section 68.2) 11-31x SMALLER; the bodies that win are those
where the hidden layer is weaker (Section 72.1) r = -0.84. THE BODY HAS THE FEATURES IT NEEDS!
Section 33 measured 19-25 narrow "bells" in the body -- exactly the local detectors a table is built
from. But: (1) the DIRECT path (sensor -> motor) has a credit about 30x LOUDER, so within a few ticks
it saturates the readout with the trivial solution (a constant plus a little periphery); (2) the
HIDDEN path (bells -> motor) has a credit about 30x QUIETER, so the bells' weights barely grow and
the reader never reaches for them; (3) Section 72.1 confirms it: the score is higher where the hidden
layer is WEAKER -- the body simply "chooses" the easy path. THIS IS THE MECHANISM OF THE LOCAL
MINIMUM, and it is measured: the useful path (bells) is about 30 times slower than the useless one
(the direct path), so the body always settles on the constant and LEAVES +16.6% ON THE TABLE.

72.7 SECTION 73 -- SAFE RATE MATCHING (the goal of Section 69 was right, the implementation was too
crude). Section 69 had the RIGHT GOAL (matching the layers' speeds) and a TOO CRUDE implementation
(an EWMA per SOMA -- and a soma with no history exploded). Now the goal is backed by a number from
the task (+16.6%), and the implementation must be GLOBAL and SAFE: normalise the hidden layer's
credit by a SHARED (organism-wide) measured credit scale rather than by each soma's own EWMA.
PRE-REGISTERED CHECKS FOR 73: (1) RMS|dw| hidden/motor from about 0.1 to about 1.0 (as in Section 69,
but without the 3.7 overshoot); (2) the hidden layer's sign agreement must NOT fall below 65%;
(3) the score median must be no worse than +0.00%; (4) NEW AND DECISIVE: the Section 54 curve must
show a DOWNWARD TREND ([T2]/[T1]) on at least some seeds -- because now there IS something to learn
(+16.6% of headroom) and the reader must start using the bells.

Instrument + log only. cargo test = 49/49.

---

## Commit 81814ba — Section 72: tomography of 9 bodies -- AND THE VERDICT ON THE TASK, which closes the whole session

72.1 CORRELATION OF SCORE WITH STRUCTURE (a fresh 9-seed sweep on the Section 62 base). Structural
number versus score, Pearson r / R^2: |err| hidden-to-motor (Section 68.2) -0.838 / 0.70; "strong"
inputs into port 0 -0.801 / 0.64; top-3 drive share +0.678 / 0.46; inputs into port 0 -0.519 / 0.27;
mean nodes +0.515 / 0.27; hidden-layer sign agreement -0.440 / 0.19; motor-layer cosine +0.353 /
0.12; live synapses -0.279 / 0.08; deaf somas -0.190 / 0.04.
THE THREE DOMINANT PREDICTORS DESCRIBE ONE PHENOTYPE: the score is higher where the hidden layer is
WEAKER (smaller |err| ratio), where there are FEWER strong inputs into port 0, and where the drive is
MORE CONCENTRATED (higher top-3). In other words A LEAN BODY WITH A NARROW, CONCENTRATED READOUT WINS.
And note the sign of the last rows: a MORE ACCURATE hidden credit means a WORSE score (r = -0.44) --
bodies where the hidden layer genuinely works (77-82% agreement) play worse than bodies where it
barely matters (56-60%).

72.2 WHY: THE VERDICT ON THE TASK (same run, Section 40.2 lag oracle): depth 1 (the current symbol)
+0.0% in-sample gain, test median 0.1667, does not beat the bar; depth 2 +0.2%; depth 4 +0.4%;
depth 6 +1.5%; depth 8 +1.9%. CONTEXT ADDS NOTHING. Any depth (1-8) gives at most a 1.9% gain over
the constant, and the memoryless model (depth 1) gives exactly 0.0%. So this task contains NO
EXTRACTABLE TEMPORAL STRUCTURE: predicting the next symbol does not depend (within error) on the
previous ones. THE HIDDEN LAYER HAS NOTHING TO LEARN.

72.3 THE COMPLETE CAUSAL PICTURE (and it finally closes): (1) the task is EMPTY -- the lag oracle
proves memory adds nothing (<=1.9% over depths 1-8) and a memoryless model gains 0.0% over the
median constant; (2) therefore the optimum is a constant plus a little peripheral information, and
the substrate FINDS it: the LEANEST bodies with a concentrated readout win (72.1); (3) the hidden
layer cannot help (nothing to learn) and only DILUTES the answer -- which is exactly why a "more
accurate hidden credit" correlates with a WORSE score; (4) the score is the best window out of about
2700 (Section 48), so its "+7.56%" or "-14%" is the SPREAD OF WINDOWS around the bar, not a trace of
learning. That is why the ensemble swings from -14% to +12% with a flawless direct layer.
SO "LEARNING DOES NOT APPEAR" IS NOT A DEFECT OF THE SUBSTRATE BUT AN HONEST RESULT: there is
nothing to learn in this task beyond the constant. Eight reverted core changes and 15+ refuted
hypotheses all ran into a task whose conditional structure (in the form the substrate sees it: one
scalar per symbol, a cyclic stream without correlations) equals the constant.

72.4 SECTION 73 -- CHANGE THE TASK (and check it with the oracle BEFORE touching the core). This can
be done without touching the core and verified in five minutes, because the oracle machinery already
exists: (1) find a stream with REAL temporal structure -- one where the lag oracle shows a LARGE
gain (say >=20% at depth 3-4) rather than 1.9%. Candidates: actual text (the bukvar is already
there -- we only need to check whether it reads as text or as a shuffled cycle), or a task with a
hidden state (predicting the next symbol of a PAIR where the current letter is ambiguous);
(2) the oracle FIRST: if the lag oracle shows a large gain, the task has something to learn;
(3) THEN the same substrate and the same instrument, watching the Section 54 curve. If the curve
falls (a [T1] appears) the substrate is sound and the wall was the EMPTY TASK; if it does not fall,
the wall really is in the substrate -- and only then, with a clear conscience, do we return to the
learning rules armed with an informative signal.

Instrument + log only. cargo test = 49/49.

---

## Commit 78e924e — Section 71: ONE CORRECT TRAVERSAL (Jacobi) -- TRIED AND REVERTED

71.1 THE HYPOTHESIS AND ITS IMPLEMENTATION. Section 70 showed that the sign agreement falls with
BODY SIZE (r = -0.35 / -0.48) and that the number of backward passes equals the number of nodes.
So instead of the N-fold echo, the SOURCE IS READ FROM A FROZEN COPY (the state at the start of the
sweep) while the result is written into node_errors: every connection adds its credit EXACTLY ONCE
per sweep, and each sweep deepens the credit by ONE HOP. That is a sum OVER PATHS (Jacobi) rather
than an N-fold echo, and it converges by its own criterion rather than by the node count.

71.2 THE THREE PRE-REGISTERED CHECKS -- ALL THREE FAILED (9 seeds).
(1) Hidden-layer sign agreement: target >=75%; measured 72.4% (was 64.1%) -- an improvement, but the
target was not reached.
(2) Size-to-agreement correlation: target |r| < 0.2; measured -0.328 (was -0.35/-0.48) -- BARELY
MOVED.
(3) Score median: target >= +0.00%; measured -1.68% (was +0.00%), mean -4.09%.
At the same time seed 9 set a NEW SESSION RECORD of +11.76% (was +7.56%), seed 4 almost healed
(-13.98% -> -1.68%), but seed 5 (was +4.50%) fell to -11.40%.

71.3 THE LESSON -- and it closes a whole line of investigation: THE ECHO DID HURT THE DIRECTION
(72.4% versus 64.1%), BUT THE SIZE-DEPENDENCE IS NOT FROM THE ECHO. The correlation r did not
vanish (-0.33) even though the traversal's semantics were fixed. So the size-dependence comes from
THE APPROXIMATION ITSELF: a bigger body has more PATHS, and every path is estimated with a STATIC
factor that ignores the TIME AXIS (recurrence, delay_ticks, InputPrev). That is a structural error
of the approximation, not an artefact of the loop.
AND THE MAIN POINT, which closes the "credit" line: three consecutive attempts each improved their
OWN metric and NONE improved the score -- Section 64a (slope convention in the chain): hidden
gradient share 0.8 -> 15%, score -10.50%; Section 69 (rate matching): RMS|dw| 0.1 -> 3.7, score
-13.20%; Section 71 (one correct traversal): sign 64.1 -> 72.4%, score -1.68%. So THE HIDDEN CREDIT
IS NOT THE SCORE'S BOTTLENECK. The direct layer is nearly flawless (88-89% sign, cosine +0.60) and
it is what delivers +7.6...+11.8% on individual seeds.

71.4 SECTION 72 -- the question changes class: WHERE DOES THE VARIANCE COME FROM? The ensemble's
score swings from -14% to +12% with a nearly flawless direct layer. That is no longer a question
about the learning rule but about the BODY: which morphology a given seed got (how many inputs port
0 has, how the drive is distributed, what share of the gradient the hidden layer carries) and how
ontogenesis picks it. Section 72 (measurement, 0 core lines): correlate a seed's score with the
body's structural numbers (inputs into port 0, top-3 drive share, hidden gradient share, node and
connection counts). If the score is predictable from structure, the wall is in MORPHOGENESIS AND
SELECTION -- the last big line we have not yet measured.

This is the eighth core change tried and reverted this session. cargo test = 49/49.

---

## Commit 12d1800 — Section 70: accumulation CONFIRMED -- three proxies of "more paths, more confusion"

70.1 INSTRUMENT: three independent cuts of the sign agreement "rule vs truth" (Section 66.2), all
with 0 core lines: (1) body size (nodes.len()) against the hidden layer's agreement -- Pearson
correlation across probes; (2) the target's IN-DEGREE (1 / 2-3 / 4+ enabled inputs); (3) the
target's OUT-DEGREE (0 / 1 / 2+).

70.2 MEASURED. Seed 5 (mean 22.1 nodes, hidden agreement 77.1%): in-degree 1 -- motor 4 pairs 50.0%,
hidden 15 pairs 80.0%; in-degree 2-3 -- 15 pairs 93.3%, 58 pairs 69.0%; in-degree 4+ -- 50 pairs
92.0%, 1 pair 100%. Out-degree 0 -- motor 69 pairs 89.9%; out-degree 1 -- hidden 39 pairs 79.5%;
out-degree 2+ -- hidden 35 pairs 62.9%. r = -0.350 (18 probes) => [M1]: the MORE NODES the body has,
the WORSE the sign agreement. Seed 9 (where the hidden layer barely matters): r = -0.477 => [M1]
again, even stronger.

70.3 VERDICT: three proxies say ONE AND THE SAME THING: more nodes means worse agreement
(r = -0.35 ... -0.48 on two seeds); more incoming paths means worse agreement (80.0% -> 69.0%); more
outgoing paths means worse agreement (79.5% -> 62.9%). This is exactly what the code does: the
number of backward passes equals the NUMBER OF NODES in the body, and every pass ADDS the credit
again. A body with 27 nodes accumulates the same credit three times more often than a body with 9
nodes -- and this explains the "sometimes 91%, sometimes 32%" depending on the body (Sections 67,
69).

70.4 SECTION 71 -- the fix the measurement has already justified: ONE CORRECT BACKWARD TRAVERSAL
instead of N-fold accumulation: every connection passes its credit ONCE, in order from the motors
inwards (or normalised by the number of contributions). PRE-REGISTERED CHECKS: (1) the hidden
layer's sign agreement >= 75% on both reference seeds (was 71.6% / 56.5%); (2) the correlation r
between size and agreement must DISAPPEAR (|r| < 0.2) -- the estimate must stop depending on body
size; (3) the 9-seed score median must be no worse than +0.00% (Section 62). Only AFTER that does
Section 69 (rate matching) make sense to retry -- now on a STABLE direction rather than a lottery.

Instrument + log only. cargo test = 49/49.

---

## Commit 7df6697 — Section 69: matching layer speeds (step normalisation) -- TRIED AND REVERTED

69.1 THE HYPOTHESIS AND ITS IMPLEMENTATION. The motive was direct: Section 68.2 showed that the
hidden layer's error estimate is 11-31x SMALLER, so with a shared eta the direct layer learns in
tens of ticks while the hidden one needs thousands (i.e. it never gets there). The classical remedy
is RATE MATCHING (LARS/AdaGrad): the step must be proportional to the LAYER'S OWN error scale
rather than to the depth at which the signal faded. Implemented SELF-CALIBRATINGLY, with no magic
numbers: each soma keeps its own `error_scale` (an EWMA of |node_errors|), and its synapses' step is
normalised as `saturate(err) * scale / own`, where `scale` is the organism's own (`ctx.error_scale`).
For a soma whose own scale equals the organism's (i.e. a motor) the step is UNCHANGED; for a hidden
soma with `own ~ scale/30` it gives x30.

69.2 THE THREE PRE-REGISTERED CHECKS -- ALL THREE FAILED.
(1) RMS|dw| hidden/motor: target ~1.0; measured 3.659 (was ~0.1) -- OVERSHOT.
(2) Hidden-layer sign agreement: target >=65%; measured 65.0% -- right on the boundary, with a
per-seed spread of 32.3-90.9%.
(3) Score median: target >= +0.00%; measured -13.20% (was +0.00%), mean -16.37%.
And one more figure that explains everything: the hidden layer's cosine fell to -0.282 (was about
-0.05) -- the hidden layer now moves 3.7 times MORE, and in the WRONG direction.
BUT THE OTHER SIDE OF IT: two seeds set SESSION RECORDS (seed 5: +9.54%, seed 2: +8.28%) and seed 3
reached [T1] for the first time. So the normalisation DOES work -- but only where the hidden
credit's direction happened to be stable, and it DESTROYS where that direction is noise (seed 1:
-53.0%, seed 8: -46.3%).

69.3 THE LESSON (and it is the third one on the same theme): THE ORDER MUST BE DIFFERENT.
Normalisation AMPLIFIES motion. If the direction is unreliable (Section 67: 48-83% depending on the
body), then it is the LOTTERY that gets amplified. This is the same lesson Section 64a taught from
the other side: there I amplified the unnormalised credit -- worse; here I normalised it -- the
median is worse too. So: FIRST the RELIABILITY of the direction, and only then the matching of
speeds.

REVERT: everything is removed (the error_scale field, the EWMA, the two constants; TRACE_ETA_RATIO
is gone too -- it was left dead after Section 62). The Section 62 baseline is reproduced
BYTE-FOR-BYTE: seed 9 = 0.1541 versus the bar 0.1667 = +7.56%. cargo test = 49/49.

69.4 SECTION 70 -- the next step is already determined: THE RELIABILITY OF THE HIDDEN CREDIT.
Three measurements point at one thing: the hidden credit is UNSTABLE FROM BODY TO BODY (Section 67:
82.6% versus 48.1%; Section 69: 32-91% across seeds). And there is a MECHANICAL suspect that has
never been tested: `for _pass in 0..max_passes { node_errors[conn.from] += credit * 0.5 }` where
max_passes equals the NUMBER OF NODES. A body with 9 nodes and a body with 27 nodes therefore
accumulate DIFFERENT AMOUNTS of the same credit -- which explains why the estimate is "sometimes
right, sometimes a coin" depending on the body.
Section 70 (measurement): stratify the sign agreement by the body's node count and by the target's
IN-DEGREE: if more nodes / more in-degree means worse agreement, then the accumulation really does
inflate the estimate, and the fix is obvious (normalise by the number of contributions, or do one
proper traversal).

This is the seventh core change tried and reverted this session. cargo test = 49/49.

---

## Commit 03bfd4e — Section 68: where the scale is lost -- source, slope, or credit?

68.1 THE "NUMERICAL DROWNING" HYPOTHESIS -- REFUTED, AND IN THE OPPOSITE DIRECTION. The motive was
strong: g = pre*sigma'*err is proportional to |pre|, so if a sensor shouts about 1.0 while a soma
whispers about 0.03, a soma->motor synapse loses by a factor of thirty at the same weight and no
credit rule can fix that. MEASURED (seed 5, 23 probes). Node classes, as n / mean |output| / max
|output| / mean |state| / mean |input|: SENSOR 4.9 / 0.7600 / 1.0290 / 0.7600 / 0.7600; HIDDEN SOMA
13.5 / 1.4249 / 5.1031 / 1.4094 / 2.1128; MOTOR 3.7 / 1.1519 / 1.2084 / 1.1519 / 2.3134. Synapse
sources, as n / mean |pre| / mean |w| / mean |pre*w|: SENSOR 19.0 / 0.7051 / 0.5441 / 0.3371;
HIDDEN SOMA 18.2 / 2.3075 / 0.4067 / 1.5403; MOTOR 0.6 / 0.2015 / 0.1053 / 0.0689.
RATIOS soma/sensor: output 1.87x, |pre| 3.27x, DRIVE 4.57x. The somas do not whisper -- they SHOUT
4.6 times louder than the sensors. So [H2]: the hidden layer's small |g| is NOT caused by the
source scale. The drowning hypothesis is discarded.

68.2 THE FACTORIZATION |g| = |pre|*|sigma'|*|err| -- THIS is where it falls. Added: the gradient
split into factors per layer (mean |pre|, mean |sigma'| from the Section 60.1h instrument, mean |g|,
and the IMPLIED |err| = |g|/(|pre|*sigma')). Seed 5: ->MOTOR 11.9 / 0.1106 / 0.0879 / 0.0162 /
0.0310; ->HIDDEN 20.3 / 0.1118 / 0.0593 / 0.0011 / 0.0017. Ratios hidden/motor: |pre| 1.72x,
|sigma'| 1.15x, |g| 0.111x, implied |err| 0.092x. On seed 9 the same, more sharply: the implied
|err| is 31x smaller. [K1]: THE CREDIT IS WHAT COLLAPSES. The hidden layer's source is not weaker
(it is louder), its slope is comparable, yet |g| is about 9x smaller and the implied |err| is
11-31x smaller. So the bottleneck is neither the source nor the slope but the ERROR ESTIMATE itself:
the backward chain, or its scale.

68.3 SYNTHESIS OF THE THREE MEASUREMENTS (and it is consistent): Section 66.2 [E2] -- the hidden
layer's estimate points the RIGHT WAY (71.6% on seed 5 versus 89.9% for the motor); Section 67 --
the hidden credit is UNSTABLE from body to body (82.6% versus 48.1% in the largest buckets) and its
|g| is 1-3 orders of magnitude below the motor's; Section 68.2 [K1] -- what collapses is |err|
(11-31x) at a LOUDER source and a comparable slope. TOGETHER: the hidden layer is not broken -- it
is one to two orders of magnitude SLOWER than the direct layer, because its credit is one to two
orders of magnitude smaller. With a SHARED eta this means the direct layer learns in tens of ticks
while the hidden one needs thousands -- i.e. it never gets there.

68.4 SECTION 69 -- the direct consequence (and it agrees with 64a!): NORMALIZE THE LEARNING STEP
BY THE CREDIT'S OWN SCALE (self-calibrating, no magic numbers -- in the spirit of the "organism's
own scales" the engine already has: error_scale, the mass factor that measurement found helpful).
Each soma must learn at a rate set by its OWN error scale rather than by the motor's. WHY THIS
AGREES WITH 64a: there I amplified the hidden credit by multiplying it with input_sensitivity --
and it got WORSE, because what was amplified was the UNNORMALISED signal (11-31x smaller and
unstable). Normalisation is the opposite move in spirit: not "turn up the volume" but "bring all
layers to a common scale".

PRE-REGISTERED CHECKS FOR 69: (1) the |dw| ratio hidden/motor must rise from about 0.1x to about
1x; (2) the hidden layer's sign agreement must NOT fall below 65%; (3) the score median must be no
worse than +0.00% (Section 62).

Instrument + log only. cargo test = 49/49.

---

## Commit 386117f — Section 67: buckets by |g| -- and the instrument flaw this measurement exposed

67.1 INSTRUMENT: self-calibrating buckets -- within each window the |g| values are sorted and the
cuts are the 33rd and 66th percentiles (no magic boundaries). Accumulated: sign agreement and the
median |g| per (layer x bucket).

67.2 MEASURED. Seed 5 / seed 9, as pairs / agreement / median |g|:
motor weak 7/100.0%/5.92e-2 versus 11/54.5%/5.80e-2; motor medium 23/95.7%/3.53e-1 versus
35/91.4%/5.15e-1; motor strong 35/88.6%/6.55e-1 versus 73/90.4%/2.29;
hidden weak 50/66.0%/6.52e-2 versus 87/58.6%/4.21e-2;
hidden medium 23/82.6%/1.61e-1 versus 54/48.1%/6.21e-2;
hidden strong 1/100.0%/3.86e-4 versus 6/100.0%/1.43e-1.

67.3 INSTRUMENT FLAW (found by this very measurement): the [F1]/[F2] verdicts in the report rest on
the "strong" bucket, and it has n = 1 (seed 5) and n = 6 (seed 9) -- statistically empty, so the
100.0% there means nothing. Worse, the buckets are COMMON to both layers while the layers' |g|
differ by ORDERS OF MAGNITUDE: the hidden "strong" bucket (3.86e-4) is 150 times weaker than the
motor "weak" one (5.92e-2). So the [F1] verdicts must be treated as INVALID, and I retract them.

67.4 WHAT SURVIVES CRITICISM: (1) the method and the direct layer are reliable -- on well-populated
buckets the motor layer gives 88-95% (medium and strong) on both seeds; (2) the best-populated
hidden buckets are INCONSISTENT across seeds: medium 82.6% (seed 5) versus 48.1% (seed 9), so the
hidden credit is neither "right" nor "wrong" -- it is UNSTABLE from body to body; (3) the key
quantitative fact: the hidden layer's |g| is 1-3 ORDERS OF MAGNITUDE smaller than the motor layer's
in the same nominal buckets (medium 0.16/0.06 versus 0.35/0.52; strong 3.9e-4/0.14 versus
0.66/2.29). So the hidden tissue barely affects the error -- and that is no longer about credit.

67.5 SECTION 68 -- the question changes class: why does the hidden tissue not weigh? Three concrete
mechanisms of "numerical drowning", each measurable right now with the same instrument:
(1) THE SCALE OF A SOMA'S OUTPUT: compare |output| for sensor sources versus soma sources -- if
somas emit about 0.05 while sensors emit about 1.0, then a soma->motor synapse loses to a
sensor->motor one by a factor of twenty at the same weight, and no credit rule can fix that;
(2) THE SHARE OF DRIVE INTO PORT 0 (Section 60.1d): port 0 has 1-11 synapses against 89-321 in the
body, and the top three hold 55-65% of the drive -- the readout is a narrow window that somas
hardly enter; (3) THE WEIGHT of soma->motor versus sensor->motor (the same hypothesis from the
other side). Each of the three yields numbers immediately.

Instrument + log only. cargo test = 49/49.

---

## Commit c3d4ac2 — Section 66.2: the sign of the backward chain against the truth (gold-standard method)

66.2.1 INSTRUMENT: a shadow chain + perturbations + a method control.
(1) Shadow reproduction of the engine's estimate: est[motor] = saturate(t_p - out_p), then up to N
passes of credit = est[to]*w*activation_slope(from); est[from] += credit*0.5. Every ingredient is
reachable from outside, so node_errors is reproduced WITHOUT any core edit.
(2) The truth from perturbations: g_i = dloss/dw_i by central differences. Descent requires
dw_i ~ -g_i while the rule gives dw_i ~ pre_i*est[to], so we compare the SIGNS
sign(pre_i*est[to]) against sign(-g_i).
(3) POSITIVE CONTROL OF THE METHOD: the same on the MOTOR layer, where truth and estimate are both
reliable. If the control is below 60% the method is wrong and no conclusion about the hidden layer
may be drawn.

66.2.2 MEASURED (250k, 23 probes). Seed 5 / seed 9: motor layer (method control) 89.9% (62/69) /
87.5% (112/128); HIDDEN layer 71.6% (53/74) / 56.5% (83/147); overall 80.4% (115/143) / 70.9%
(195/275); pairs discarded (|pre| too small) 726 / 764; verdict [E2] / [E3].

66.2.3 VERDICT AND ITS NUANCE: the method is valid -- the motor-layer control is 88-90% on both
seeds. The direct layer is nearly correct (88-90%), agreeing with Section 65.2 (+0.598): the direct
layer must NOT be touched. The hidden chain gives the correct sign 71.6% (seed 5) and 56.5%
(seed 9) -- it neither lies nor is reliable, and it depends on the body. THE NUANCE THAT CHANGES
THE READING: seed 9 is a body whose hidden layer carries 0.8% of the gradient, i.e. it barely
affects the error. For somas that weigh nothing, dloss/do_j is about zero -- and "sign agreement"
there measures NOISE AROUND ZERO, not an error of the chain. Seed 5 (6% of the gradient in the
hidden layer) gives 71.6%, markedly better. So two explanations remain undivided: (a) the chain
scrambles the sign where a soma MATTERS; (b) the chain is right but the hidden tissue does NOT
matter -- and then we are measuring noise.

66.2.4 SECTION 67 -- separating them in one step: stratify the sign agreement by the MAGNITUDE of
the true gradient |g_i| (tertiles). If the agreement on STRONG |g| is >= 80% while weak ones are
about 50%, then the chain is right WHERE IT MATTERS and the wall is not the credit but the
RELEVANCE of the hidden tissue (0.8-6% of the gradient while consuming 20-53% of the plastic
budget). If the agreement on STRONG |g| is also about 50%, then the chain really does scramble the
direction where it hurts, and only then must the FORMULA of the chain be changed.

Instrument + log only. cargo test = 49/49.

---

## Commit e7e0bcf — Section 65.2: error compression (saturate) -- REFUTED AS THE CAUSE, BUT A NEW LEAD FOUND

65.2.1 A PRE-REGISTERED CAVEAT (checking my own reasoning first): the form
pre*sigma'*saturate(e) for ONE SHARED SCALAR e is just the whole vector multiplied by a POSITIVE
number k = sat(e)/e > 0, and a cosine is scale-invariant. So prediction #1: the cosine of that form
MUST match the linear one exactly. Ratio distortion is possible only when the error DIFFERS per
target -- i.e. PER MOTOR PORT. The instrument therefore measures four forms: global error; the
error of the SPECIFIC motor port (t_p - out_p); that same error compressed by the engine's
saturate; and the hypothesis form with a global scalar (the control for prediction #1).

65.2.2 MEASURED (pooled per class, cosine with the target -g). Seed 5 motor/hidden:
pre*sigma'*e +0.035/-0.013; per-port +0.123/-0.013; per-port compressed +0.127/-0.013;
global compressed (the hypothesis) +0.020/-0.013. Seed 9 motor/hidden: -0.157/-0.002;
+0.426/-0.002; +0.598/-0.009; -0.131/-0.009. scale = 0.1667 and max |e|/scale = 4.24 (seed 5) and
3.85 (seed 9), i.e. sat(e)/e = 0.236-0.260 -- the compression is ACTIVE (the step is about four
times smaller than the error), not a linear regime.

65.2.3 VERDICT ON THE HYPOTHESIS: prediction #1 confirmed -- in the hidden layer the hypothesis
and the linear global form give the SAME cosine (-0.013 / -0.009): for one shared error the
compression does not change the direction. That is geometry, and it gave the hypothesis no chance.
[S2]: compression is NOT the cause of the anti-gradient -- in the hidden layer the compressed form
is no worse than the linear one. (The opposite in the motor layer: compressed BEATS linear,
+0.598 versus +0.426.)

65.2.4 THE UNEXPECTED LEAD (worth more than the hypothesis): the error of the SPECIFIC TARGET is
far better than the "shared" one: in the motor layer the cosine jumps from -0.157 to +0.426, and
the compressed per-port form gives +0.598 -- the best figure of the session. So the engine ALREADY
has the right signal for the direct layer (node_errors[to] for a motor is exactly the per-port
compressed error), while the hidden layer stays about zero under ANY scalar multiplier -- hence no
scalar form can save it: what is wrong is the ESTIMATE of the hidden soma's error itself (the
backward chain), not the multiplier in front of it.

AN HONEST METHODOLOGICAL CAVEAT: "REALISED dw" in the Section 62/63 tables is compared against a
gradient from a DIFFERENT tick (dw is averaged between probes, the gradient is instantaneous), so
it is a WEAK estimator; the reliable part is the CANDIDATE table (same tick, same gradient). That
refines the reading of Section 63: "the hidden layer against the gradient" is about the REALISED
update, not about the form of the credit.

65.2.5 SECTION 66 -- what is left: the hidden soma's error estimate itself. The one link not yet
measured is the multi-pass accumulation of the backward chain
(for _pass in 0..max_passes { node_errors[conn.from] += credit * 0.5 }).
66.1 (accumulation): correlate the hidden-layer angle with the target's IN-DEGREE -- more incoming
paths means more additions and a more distorted estimate.
66.2 (against the truth): for each hidden soma estimate the TRUE dloss/do_j numerically (by
perturbing its outgoing weights) and compare the SIGN with what the chain gives -- does the
estimate match the truth anywhere at all?

Instrument + log only. cargo test = 49/49.

---

## Commit 238c01f — Section 65.3: the phase shift (delay_ticks / InputPrev) -- REFUTED

65.3.1 THE HYPOTHESIS: the single-tick backward pass sees neither delay_ticks nor genes that read
InputPrev. In the CYCLIC Bukvar stream a shift of half a period physically INVERTS the sign
(cos(wt+pi) = -cos wt), so temporal connections should give an anti-gradient -- which would have
explained the mysterious -0.03 angle.

65.3.2 INSTRUMENT: a cross-tab of target x temporality. The layer code now has six classes:
->motor / ->hidden / ->other x static / TEMPORAL, where TEMPORAL = delay_ticks > 0 OR the source
gene mentions ScalarOp::InputPrev (both mechanisms of the hypothesis, tested together). The pooled
cosine is computed per class, together with the class's share of the gradient norm.

65.3.3 MEASURED -- THE HYPOTHESIS DID NOT HOLD. Seed 5: ->motor static 9.6/window, +0.046, 93.9%
of the gradient; ->motor temporal 0.0, NaN, 0.0%; ->hidden static 13.2, -0.049, 6.0%; ->hidden
temporal 2.0, -0.036, 0.0%. Seed 9: ->motor static 10.2, -0.027, 99.2%; ->motor temporal 0.0, NaN,
0.0%; ->hidden static 12.1, +0.007, 0.8%; ->hidden temporal 1.4, +0.024, 0.0%.
[G6] on BOTH seeds: (a) TEMPORAL MOTOR CONNECTIONS DO NOT EXIST AT ALL, so an "output phase shift"
is physically impossible in this morphology; (b) temporal hidden connections are NOT WORSE than
static ones (seed 5: -0.036 versus -0.049; seed 9: +0.024 versus +0.007), so delays and InputPrev
are NOT the source of the anti-gradient; (c) where the anti-gradient does exist (seed 5) it lives
in the STATIC hidden synapses (-0.049), which carry 6% of the gradient, while the temporal hidden
ones carry 0.0%.

65.3.4 WHERE THIS MOVES THE SUSPICION: saturate plus multi-pass accumulation. The code hints at
what should have been seen earlier: `scale = ctx.error_scale...; saturate = |e| scale*(e/scale).tanh()`
then `node_errors[m_id] = saturate(err)` on entering the backward chain, and then
`for _pass in 0..max_passes { node_errors[conn.from] += credit*0.5 }` -- the node's error is (a)
ACCUMULATED up to N times by the same traversal and (b) COMPRESSED BY tanh on every read, which
preserves the sign but DESTROYS THE RATIOS -- and in a multidimensional space it is precisely the
ratios between different nodes' errors that set the direction. Two measurable hypotheses remain:
65.1 (unnormalised accumulation): correlate the hidden-layer cosine with the target's IN-DEGREE.
65.2 (ratio compression): add the candidate pre*sigma'*saturate(e) (with the same scale) to the
table and compare its cosine with pre*sigma'*e.

Instrument + log only this round. cargo test = 49/49.

---

## Commit 32ad365 — Section 64a: one slope convention in the backward pass -- TRIED AND REVERTED

64a.1 THE HYPOTHESIS (mathematically tempting): the chain rule for propagating the error from the
target j to the source i requires the factor do_i/dnet_i -- the SOURCE's full input sensitivity --
while the backward pass multiplied by activation_slope() (only do/dstate, a different derivative).
So bring the convention in line with the one that already delivered the breakthrough in the
forward step (Section 62, input_sensitivity()).

64a.2 THE PRE-REGISTERED CHECKS -- ALL THREE FAILED.
(1) Hidden-layer cosine: expected to move from -0.037 into positive territory; measured -0.027 --
still negative.
(2) Section 54: expected some seeds to reach [T1]; measured [T1] 0 of 9, [T2] 1, [T3] 8 (Section
62 had 6x[T2]).
(3) Hidden-layer gradient share: expected 1-6% -> 15-30%; measured 15.3% -- it DID rise, but with
no benefit.
Score over 9 seeds: median -10.50% (was +0.00%), mean -8.15%, maximum -0.06%. WORSE ON ALL NINE
SEEDS. Seed 1 is telling: direct cosine +0.181 (the best of the session), hidden gradient share
70.3% -- and the score is -12.06%.

64a.3 THE LESSON (worth more than the attempt): the hidden layer CAN be made weightier
(0.8% -> 15%, up to 70%), and doing so makes everything WORSE. So its credit is not merely
mis-scaled: it carries a WRONG DIRECTION, and amplifying it only multiplies the damage. That
directly refutes the ordering in Section 64b: "make the hidden layer relevant" is PREMATURE --
relevance without direction is harmful.

REVERT: the convention is restored to activation_slope(), and the Section 62 baseline is
reproduced BYTE-FOR-BYTE (seed 9 = 0.1541 versus the bar 0.1667 = +7.56%; the Section 63 numbers
are identical: 99.2%/0.8%).

64a.4 SECTION 65 -- the remainder is narrowed to three concrete mechanisms, all measurable with
the instrument that already exists:
(1) MULTI-PASS ACCUMULATION (for _pass in 0..max_passes { node_errors[from] += credit*0.5 }):
the same credit is added up to N times on the same traversal, so a node's error is inflated in
proportion to the NETWORK SIZE -- that is not a gradient but an unnormalised path sum. Check: the
distribution of node_errors by depth against the motor error (depth must not multiply credit).
(2) saturate() ON THE ERROR (both in the forward step eff_err = saturate(post_err) and in the
backward pass): compressing large errors preserves the sign but DESTROYS THE RATIOS, and the
ratios are what determine the direction in a multidimensional space. Check: add the candidate
pre*sigma'*saturate(e) to the table and compare cosines.
(3) THE TIME AXIS: the forward pass has delay_ticks and genes reading InputPrev/state, while the
backward pass is SINGLE-TICK: for recurrent and delayed paths the credit is not "off by a scale"
but simply WRONG IN SIGN. Check: the hidden-layer cosine split by delay_ticks = 0 versus > 0.

This is the sixth core change tried and reverted this session. cargo test = 49/49.

---

## Commit 480acc9 — Section 63: the layer split -- where exactly does the remaining divergence live

63.1 INSTRUMENT: a pooled cosine per layer plus norms. Section 62.0 gained a LAYER tag per
connection (0 = straight into a motor, 1 = into a hidden soma) and a POOL instead of an average of
cosines: for every (layer, candidate) pair it accumulates sum(p*(-g)), sum|p|^2, sum|g|^2, so the
cosine is computed on a shared pool and the norms give the DISTRIBUTION of the gradient and of the
motion across layers -- which an average of cosines could not show.

63.2 MEASURED (250k, 15 windows). Seed 5 versus seed 9 (the +7.56% record):
gradient share direct/hidden 93.9%/6.1% versus 99.2%/0.8%; motion dw share 79.7%/20.3% versus
47.1%/52.9%; cosine of REALISED dw direct +0.015 versus -0.027; hidden -0.037 versus +0.008;
pre*e/(1+2*mass) +0.100 (best) versus +0.020.

63.3 TWO DIFFERENT REMAINDERS (and they are not the same thing):
(1) CREDIT DIVERGENCE (seed 5, [G3]): the direct layer follows the gradient (+0.015) while the
hidden layer goes AGAINST it (-0.037). That matches theory -- the Section 62 delta rule is exact
only for the last layer, and the hidden credit travels through the backward pass where the slope
convention differs (credit = err_to * weight * slope of the SOURCE, brain.rs:856-866, whereas the
forward step now multiplies by the MEASURED sensitivity of the TARGET).
(2) RELEVANCE OF THE HIDDEN LAYER (both seeds): the direct layer carries 94-99% of the gradient
while the hidden layer carries 0.8-6%. So the hidden tissue barely affects the error yet consumes
20-53% of the plastic budget. The seed 9 record (+7.56%) is a win of the DIRECT READOUT (the very
"linear alphabet" Sections 40/41 measured as weak but real), not of composition learning.
(3) THE MASS FACTOR STAYS: measurement says it IMPROVES alignment (pre*e/(1+2*mass) = +0.100
versus +0.019 without it) -- mature tissue takes a smaller step, and that favours the direction.

63.4 SECTION 64 -- two hypotheses already separated by numbers:
64a (if the wall is credit): bring the hidden credit convention in line with the direct one --
multiply the backward-pass credit by the SOURCE's measured input_sensitivity instead of
activation_slope. Prediction: the hidden layer's cosine must move from -0.037 into positive
territory and the Section 54 curve from [T2] to [T1].
64b (if the wall is relevance): if after 64a the hidden layer's gradient share stays around 1-6%,
then the hidden tissue is STRUCTURALLY unused (its output does not reach the motor in a way that
weighs), and the fix is not in the rule but in the MORPHOLOGY of the readout.

Core unchanged this round (instrument + log only). cargo test = 49/49.

---

## Commit 2fff34c — Section 62: the form of the credit -- MEASUREMENT PICKS THE RULE (and it works)

62.0 INSTRUMENT: THE ANGLE BETWEEN THE RULE AND THE TRUE GRADIENT. Before touching the core the
question was put to measurement: which way does the update point relative to the true gradient?
For every synapse the Section 60.1 instrument computes the SIGNED g_i = dLOSS/dw_i by central
differences on the same clones, and main keeps a weight snapshot from the previous probe, so the
REALISED dw is known. Then the cosine with the target -g (descent must go AGAINST the gradient)
for seven candidate forms. One measurement, seven hypotheses, zero core edits.

62.1 MEASURED (seed 5, 23 windows): pre*e (raw error, classical LMS) +0.091;
pre*sigma'*e (delta rule with the local slope) +0.108 -- BEST; pre*de (error REDUCTION, the
hypothesis from the previous step) +0.014 -- WORST; elig*e (eligibility trace) -0.045;
elig*sigma'*e -0.054; pre*e/(1+2*mass) (the current eta factor) +0.059; REALISED dw (what the
code actually did) -0.056.

Three conclusions, all from numbers: (1) the REDUCTION hypothesis is refuted (+0.014, worst) --
which is logical, because in Bukvar the target is available in the SAME tick as the input, so the
instantaneous error already IS the gradient and a temporal credit window adds nothing; (2) the
best form is the delta rule with the target's local slope sigma' = do_j/dnet_j (+0.108);
(3) the body's realised update has a NEGATIVE angle (-0.056) -- the weight motion is not merely
weak, it is OPPOSITE to the descent direction, and it matches the trace version (-0.045) almost
exactly.

62.2 THE CAUSE IS IN THE CODE (found by reading, confirmed by measurement). The weight update in
adapt_synapses_ctx has TWO parts: part 2, the classical LMS `delta_w = individual_eta * pre *
eff_err` -- WITHOUT the target's local slope; and part 2b, the "Directed Eligibility Trace"
update `delta_nm = individual_eta_nm * directed_err * conn.eligibility_trace`. And the trace
itself is defined as the HEBBIAN PRODUCT: `hebb = pre_out * post_out; trace = trace*0.92 +
hebb*0.08` (brain.rs:743). So part 2b effectively yields `dw ~ err * pre * post` -- the error
weighted by the soma's OUTPUT, whereas the delta rule requires do_j/dnet_j (the SLOPE). That is
the classical substitution of activation for sensitivity, and it is what flipped the direction:
part 2b dominated part 2, so the realised dw inherited the trace's sign.

62.3 SECTION 62 IN THE CORE: (1) NodeGene::input_sensitivity() -- a MEASURED do/dinput at the
operating point (numerically on its own genes, the same device as activation_slope, since no
analytic formula exists for an arbitrary gene); (2) part 2 became the delta rule
`dw = eta * pre * sigma'(target) * eff_err`; (3) part 2b is REMOVED -- the `post` factor takes the
update away from the gradient (measured: -0.045 versus +0.091). The trace remains as a measured
quantity (the calcium cascade, the Section 57 instrument) but no longer moves weights.

62.4 RESULT -- THE BEST CONFIGURATION OF THE WHOLE SESSION. Median over 9 seeds: -0.90% (Section
53 base) / -0.78% (Section 61) -> +0.00% (at the bar). Mean: -1.94% / -3.65% -> -0.19%. Maximum:
+2.82% / +2.34% -> +7.56%. Seeds at or above the bar: 1 / 3 -> 5 (seeds 1, 2, 3, 5, 9). Section 54
verdicts: 6x[T3] / mostly [T3] -> 6x[T2], 3x[T3]. Seed 5 (the old champion, which collapsed to
-18.78% under Section 61) is now +4.50%; seed 2 went -6.24% -> +4.44%; seed 9 is +7.56%, a
session record. The Section 54 curves move downward en masse for the first time, and several
seeds drop BELOW the old floor: 0.3412 -> 0.3243, 0.3346 -> 0.3222, 0.3536 -> 0.3339.

HONEST CAVEATS: [T1] (monotone decline) is 0 of 9; [T2] means "a trend exists but is
non-monotone", so learning is NOT yet reliable. The median is exactly 0.00% -- two seeds sit ON
the bar, not above it. The spread is wide (-13.98% to +7.56%), so n=9 is a minimum, not proof.
But for the first time this session a core change is better than the base on the median, the
mean AND the maximum -- and it came not from taste but from measuring the angle between the rule
and the gradient.

Core: src/genome/node.rs (input_sensitivity), src/genome/brain.rs (delta rule + removal of the
trace-based update). cargo test = 49/49.

---

## Commit 2d84681 — Section 61: constant generators -- a cell that does not read its input

THE CYST HYPOTHESIS WAS REFUTED BY MEASUREMENT (so Section 61 was rewritten before any core
edit). Instead of writing a "new synapse must have a path to a motor" rule into the core, the
Section 60.1 instrument got a CONNECTOME COMPOSITION cross-tab -- and it showed the hypothesis
does not hold: sensor->PORT-0 5 synapses 0% ghosts; sensor->other motor 30, 100% ghosts;
sensor->hidden 63, 95.2%; hidden->PORT-0 20, 10%; hidden->other motor 141, 100%; hidden->hidden
59, 94.9%. ALL 171 ghosts DO end on a motor, so a path-to-motor rule would change nothing (it
would cure a disease that does not exist). The instrument also re-measured sensitivity against
the engine's OWN loss over all ports (step.targets) instead of last_pred (port 0 only): the very
same figure, 91.0%, so this is not a port-reading artifact.

THE REAL MECHANISM: half the somas are constant generators. Section 60.1g/60.1h measure, per
target, how many inputs are live, and per soma, the INPUT gain h(x) along the real path
(update(x) then compute_output()) over the physical range. Result: 18 hidden somas with inputs,
17 of them DEAF TO ALL THEIR INPUTS (41 synapses, none live); 9 of 18 (50.0%) do not read their
input ANYWHERE; median input gain 2.26e-3, min 0, max 17.2; motors 3, none deaf. So nine of
eighteen hidden somas emit a constant: their weights can be turned for 500,000 ticks (Section 59:
weight directedness 39.7%) with no effect on the error -- the signal has nowhere to flow.

WHY THE ENGINE DID NOT CATCH IT (measured, not guessed). The engine already has a measured
criterion is_viable_feature() (response to its own knobs plus nonlinearity over the input) and a
bounded search GENE_RESPONSE_ATTEMPTS = 24. But: (1) recycle_dormant_genes installed a new program
WITHOUT any check, and closed a loop -- a constant generator has zero flux, so it counts as
dormant and receives another UNCHECKED program; (2) mutate_computation rewrote a gene WITHOUT a
check, so an ordinary mutation could delete the opcode that reads the input; (3) crucially, 24
random attempts do not guarantee even the weakest condition -- the new test showed 37 of 41 somas
still blind after the search. Example of a blind cell: integration=[Sensitivity]
emission=[Div, Decay, Bell, Mul, Sensitivity, Max], span 0.00e0 -- the integration gene emits its
own knob and never reads the input. (4) RULER DEFECT found by that test: a panic v[v.len()/2] on
an EMPTY vector in the Section 60.1 instrument killed the run -- 3 of 9 Section 61 seeds produced
no verdict. Fixed: an empty vector is a legitimate state, not an instrument error.

SECTION 61 IN THE CORE: A GUARANTEE, NOT A LOTTERY. Added NodeGene::reads_input_over_range()
(the weak necessary condition, measured on the real path) and NodeGene::repair_if_input_blind():
if the program is blind it is repaired to the simplest physical form that ALREADY carries the
input -- integration [Input] (state := input), and if that is not enough, emission [State]
(output := state). Such a cell is a transparent wire from which mutation and plasticity can shape
a feature; a blind generator is a dead end with nothing to shape. The guarantee is applied
everywhere a soma's program is born or rewritten: newborn_node, recycle_dormant_genes,
mutate_computation, NodeGene::random_hidden. Test test_section61_rewritten_programs_must_read_input
(40 growth cycles plus re-differentiation): 0 blind (was 37 of 41).

RESULT: THE MECHANISM IS FIXED, LEARNING IS NOT THERE. Live synapses 9.0% -> 25.2%; deaf hidden
somas 50.0% -> 13.5%; median input gain 2.3e-3 -> 1.000; the instrument's verdict flips from [C1]
"ghosts" to [C4] "the weights ARE in the circuit". Score median over 9 seeds: -0.90% -> -0.78%
(within noise); mean -1.94% -> -3.65%. Five seeds improved (1: -4.38 -> -0.78; 3: -0.72 -> +1.50;
9: -10.32 -> +2.34; 4: -0.90 -> -0.48; 6: -1.74 -> -0.42) and four got worse (2, 5, 7, 8),
including the reproducible champion seed 5 (+2.82% -> -18.78%). The Section 54 curves stayed at
the same level (~0.33) with tiny shifts (seed 8 -0.0135, seed 7 -0.0048, seed 1 -0.0030): there is
still no learning curve.

CONCLUSION: the wiring/gene line is CLOSED. The mechanism named by measurement was real and has
been fixed, yet no learning appeared. So the wall is NOT the wiring, NOT blind somas, NOT eta
(Section 56), NOT time (Section 59). One hypothesis remains -- the FORM OF THE CREDIT
(Section 60.3) -- and it is now the only one, because every other has been refuted by measurement.

Core changed for the first time since Section 53: src/genome/node.rs, src/genome/mutator.rs.
cargo test = 49/49 (48 + the new invariant test).

---

## Commit 215b69f — Section 60.1: output sensitivity -- 91% of synapses are GHOSTS

Instrument (examples only, 0 core lines), using the engine itself rather than
reimplementing its forward pass:
* cradle.current_multimodal_step(&self, ...) is a PURE read of this tick's input -- no cursor
  advance, no RefCell -- so the input can be taken WITHOUT changing the simulation state;
* BrainGenome is Clone, so from ONE snapshot two brain copies are made, one synapse's weight is
  shifted by +-1%, and the engine's own step_multi_io is run on both;
* Sensitivity = |pred(w+eps) - pred(w-eps)| / 2eps, on two scales: 1 tick and 8 ticks (does the
  effect arrive through delay_ticks and intermediate somas);
* UNION over 25 probes (every 10k ticks, different stream phases), keyed by the (from,to) pair:
  a synapse counts as ALIVE if it moved the output even once -- this kills the "the source was
  merely silent at that instant" artefact;
* plus the analytic drive decomposition (the signal into a target is bus.send(to,
  source_output * w), so each synapse's contribution to the motor is visible WITHOUT any
  perturbation) and the motor's local gain slope = measured_sensitivity / |source_output|.

Determinism preserved: seed 5 with the probe still yields the very same 0.1620 as the Section 53
baseline, so the probe is pure telemetry -- a built-in test that the ruler does not move the
engine.

MEASURED (seed 5, 250k): live share per probe 28.4% -> 13.3% -> 8.5% -> 20.0% -> 11.2%, median
|dpred/dw| EXACTLY 0.00e0 at every probe. UNION over 25 probes: 321 unique connections, alive at
least once 29 (9.0%), alive on the 1-tick scale 31 (9.7%), alive in EVERY probe 13 (4.0%).
=> [C1-UNION] 91.0% of connections are PERMANENT GHOSTS: across 25 different stream phases, each
followed for 8 ticks, a +-1% weight change NEVER once moved the prediction.

CONFOUND FOUND AND REMOVED: the first version counted "46 synapses into the motor", but last_pred
is only the FIRST port. Counting the drive for port 0 alone: 11 inputs, max |contribution| 0.5350,
3 of them strong (>=10% of max) and ALL THREE ARE LIVE (0 deaf). So the median slope of 0.000e0
over 28 measurements is about the rest (other ports plus weak inputs), not about a deaf port.

MECHANISM: neither saturation nor a hard gate -- TOPOLOGY. Port 0 is not deaf (3 of 3 strong
inputs respond), the drive is not cancelled (|sum| 2.294 out of sum|.| 3.107, so 74% arrives), and
therefore the 91% are dead not because the motor is deaf but because their signal has NO ROUTE to
the port that defines the prediction. The body grows tissue into a soundproof room: 78 of 89
synapses live outside the path weight -> pred.

CONSEQUENCE THAT CLOSES THE CREDIT QUESTION: for 91% of the weights the derivative dpred/dw is
EXACTLY zero, so ANY learning rule (centred, reduction-based, advantage) is multiplied by zero:
dw = eta*pre*eff_err*0 = 0. The form of the credit cannot help here BY CONSTRUCTION -- this is not
a weak signal, it is a missing path. So Section 60.3 (centering) is postponed: first the circuit
must be switched on.

CROSS-VALIDATION (seed 7, the most directed run of Section 59): 49 unique connections, alive at
least once 4 (8.2%), alive in every probe 4 (8.2%), ghosts 91.8%, and PORT 0 HAS EXACTLY ONE
SYNAPTIC INPUT (it is live). Motor output -0.7126, identical at 1 and 8 ticks. Two bodies of very
different size (321 and 49 connections) give the same 91% ghosts; and in seed 7 -- the best run by
weight dynamics -- the prediction is a function of a SINGLE synapse. That is not a brain with a
readout, that is one wire.

SECTION 61 (next core change, the first this session with a named mechanism): ontogenesis grows
synapses with no path to the actuator. So a new synapse must have a route to at least one motor
port, and resorption must not stop at proven tissue whose proof is activity rather than influence.
Success measure (in the instrument, not the core): the live share must rise above 9% and TOP-3 must
fall from 55% (i.e. the drive must spread rather than rest on three). Falsifiable prediction: if
after Section 61 the live share rises while the Section 54 error does NOT move, then wiring is not
the issue and only then do we take up the form of the credit (Section 60.3).

Core untouched (examples/diagnose.rs + this log), cargo test = 48/48.

---

## Commit 753e7bc — Section 59: time does not save the day -- sqrt(t) refuted, but only by the third argument

9 seeds x 500,000 ticks, 0 core lines. Logs: target/O59_s1..9.txt.

FIRST OF ALL: the logs were unreadable. PowerShell's Out-File -Encoding utf8 after a
native command double-transcodes: the engine writes UTF-8, PowerShell decodes it as
CP850 (the OEM codepage) and only then encodes to UTF-8. Every Cyrillic field becomes
mojibake, so any parser looking for a verdict returns empty -- which is why several
extractions showed N/A -- while ASCII markers like [T1] and 0.1667 SURVIVE and silently
yield WRONG results. Recovery for existing logs: text.encode('cp850').decode('utf-8')
(all 256 CP850 code points are defined, so the recovery is lossless). For future runs:
cmd /c redirect of the exe output, or [Console]::OutputEncoding set to UTF8 before
launching. After recovery all 9 seeds were re-read (target/o59_table.py) and the
Section 54 numbers and the scores matched what had parsed before (seed 5:
0.3258 to 0.3397, delta +0.0139; score +2.82%).

THE SCORE COULD NOT MOVE -- that is a property of the metric, not evidence.
sim.best_window_error is a RUNNING MINIMUM over windows, and the best window falls in
the first half of the run for all 7 seeds measured in both runs; hence the 500k score is
bit-identical to the 250k one (-4.38, -1.02, -0.72, -0.54, -0.66, +2.82, -10.32). The
five extra windows (epochs 6-10) never once beat the first half. So "the median did not
budge" is NOT evidence about sqrt(t) -- it is a tautology of a best-window metric. The
sets also differed: Section 53 had 7 seeds (4 and 6 CRASHED from the ruler defect fixed
in Section 58) giving median -0.72%, while Section 59 has 9 giving -0.90%.

THE VALID EVIDENCE IS THE SECTION 54 CURVES, and they are categorical: 6 of 9 seeds are
[T3] no trend, 3 are [T2] weak and non-monotone, and 0 of 9 are [T1] monotone decline.
The bar is 0.1667 while the epochs live at 0.33 (same measure, doubled scale): the body
sits at the dumb-constant level for all 10 epochs. Stronger still, the error is not
merely failing to fall -- it is PINNED to a constant (seed 1: 0.3415 for nine epochs
straight; seed 7: 0.3346 for five; seed 9: 0.3360 for eight). That is the same
fixed-point signature Section 56 saw as delta 0.0000.

THE ENGINE ITSELF CONCEDED STAGNATION: two of the nine runs did not exhaust the budget
but stopped on their own convergence criterion -- PLATEAU (record stopped improving) at
366,275 ticks (seed 4) and at 349,804 (seed 7), i.e. 130-150k ticks short of the cap.
The remaining seven hit the safety cap without ever taking the tolerance.

THE SIGN WHISPER DOES NOT GROW WITH TIME: 8 of 9 seeds sit above 50% on the mid scale
(5,000 ticks), mean 53.9%, errors +-2.4..8.5. If SNR were proportional to sqrt(t) this
number had to rise when time doubled -- it did not (seed 5 went 48.6% to 53.4% while
seed 2 went to 46.7%, i.e. within the same noise). The whisper is stationary.

THE HEADLINE FINDING: weight motion is DECOUPLED from the error. Seed 7 is the most
directed run of the nine -- weight directedness 0.397 (39.7%), fast sign agreement
66.1%, mean |w| growing 2.58 to 5.57 (x2.2) -- and its error does not move at all
(pinned at 0.3346) with a score dead on the bar. Seed 2 has the worst sign (46.7%, a
mean-reverting coin) and the SAME error level. So sustained synaptic drift worth 40% of
directedness buys nothing: weight movement is not performance. That changes the suspect
-- the question is no longer how strong the credit is, but whether the prediction
depends on the plastic weights at all.

SECTION 60, reordered. Centering the error has two traps visible in our own code:
(1) the rule is already supervised LMS, dw = eta*pre*(target - pred), and its DC
component is exactly what produces a constant predictor -- the body already sits
precisely at the constant level, so subtracting the mean error removes the one thing
demonstrably working; (2) if the prediction sits near the median then the mean error is
ALREADY zero and centering is a no-op. So:
  60.1 OUTPUT SENSITIVITY first (decisive, cheap, 0 core lines): snapshot the body at
     several epochs, and for each enabled synapse (or a random sample), on a CLONE,
     shift w by +-eps and recompute the target soma's output from the stored states,
     recording the absolute dpred per eps. If it is about zero for most synapses the
     weights are OUTSIDE the causal path to the output and ANY credit rule is equally
     futile -- the PATH must be fixed, not the credit.
  60.2 DC OF THE CREDIT: the mean of (target - pred) per epoch; if about zero the
     centering hypothesis dies cheaply, before any core edit.
  60.3 only if 60.1 shows real sensitivity: then contrastive credit as REDUCTION
     (pre times the change of error, plasticity driven by the error DECREASING rather
     than by its level), always keeping the bias term or the constant the body already
     knows disappears.

SIDE BUT IMPORTANT: growth hurts. The first epoch is best for 6 of 9 seeds and the error
rises as hidden somas grow (seed 5: 0.3258 to 0.3397 as 11.6 to 13.6; seed 4: 0.3316 to
0.3401 as 8.6 to 12.2).

Core untouched (only this log changed this round), cargo test = 48/48.

---

## Commit ae67995 — Section 58: the sign of the credit -- NOT a coin, but WEAK, and the weakness tracks the score. Instrument (examples only, 0 core lines): a new SignDynamics struct measures the PERSISTENCE OF sign(dw) on three scales at once -- fast (delta between adjacent 100-tick censuses), mid (net shift over 5,000 ticks vs the previous 5,000; the decisive sample, where an epoch-scale trend lives but the body has not yet replaced its connections), and slow (per epoch). Plus the half-period asymmetry and the efficiency |sum dw|/sum|dw|. Connections are keyed by the (from,to) pair, so a live connection does not depend on prune/birth ordering.  RULER DEFECT THE INSTRUMENT FOUND (and which had been silently killing seed 4): seed 4 used to CRASH. It was not Section 58 -- the committed version without it crashes at the very same place (diag_head.rs:811, same message). The cause is the Section 41 oracle: let dim_all = oracle.xs_all.first().map(|r| r.len()). The third pool's row is sensors ++ somas, and the sensor block is POSITIONAL: channels appear and DIE (prune), so the row length floats. The length of the FIRST row is no measure for the rest: fit_readout walked up to dim_all = 52, met a row of length 51, and the run died for good (seed 4, 2362 output lines instead of the full 2574). Fix: fit the COMMON PREFIX of all rows and print the diagnosis explicitly -- the instrument now warns by itself: rows 51..52, fitting common prefix 51. Seed 4 now COMPLETES (final verdict present).  MEASURED: the sign is not a coin -- it is weak, and the weakness correlates with the score. Seed 5 (frozen, median -0.9%): fast 56.8%+-0.39 (62,538 pairs), mid 48.6%+-3.12 (984 pairs), efficiency 2.9e-2. Seed 4 (winner, +3.5%): fast 52.7%+-0.33 (87,447), mid 58.0%+-2.74 (1,248), efficiency 5.2e-3. The 9.4-point gap at errors of +-3 is statistically significant. In the FROZEN seed the mid-scale sign is 48.6%, i.e. not even a coin but mildly ANTI-persistent (shifts revert -- the signature of homeostasis/bounds, not learning). In the WINNING seed it is 58.0% (95% CI [55.3, 60.7]), i.e. a direction genuinely exists -- but its MAGNITUDE is tiny: a path of 2.3e2 yields a shift of 1.2, so 0.5% of the distance travelled survives as drift (2.9% in seed 5).  REFINED VERDICT (replacing the absoluteness of Section 57): Section 57 said the signal carries no sign. Section 58 refines it -- the sign IS there, but the SNR is ~10-20%, and it is that magnitude, not the presence, that separates the winner from stagnation. So the thing to amplify is NOT eta: Section 56 already proved that x100 on eta means x100 on the noise and wrecks the body in the first epoch. Drift and noise grow differently -- noise as sqrt(t), drift as t -- hence SNR is proportional to sqrt(t), and the right lever is the INTEGRATION TIME, not the step amplitude.  Section 59 (falsifiable prediction): if SNR is proportional to sqrt(t), doubling the run (250k -> 500k) must raise the MEDIAN across seeds without changing eta, the architecture, or a single core line. If the median does not move (and only the best window improves -- the cherry-pick effect of Section 48), then sqrt(t) is not our lever and the next suspect is the FORM of the credit itself (eff_err as saturate(node_errors) -- whether it loses its sign on saturation). Control: the Section 58 measure in the same run must stay above 53% on the mid scale; if at 500k it falls to a coin, the drift is exhausting itself and the sqrt(t) hypothesis is refuted by a second, independent route.  Core untouched: only examples/diagnose.rs and this log changed, so core outputs stay byte-identical to the Section 53 baseline. cargo test = 48/48, cargo check --all-targets clean.


---

## Commit ada37b0 — Section 57: the update factors -- all three are ALIVE, yet there is no direction. Instrument (examples only, 0 core lines): WeightDynamics now also reports the ABSOLUTE |dw| per census (not the relative measure of Section 55) and two factors readable from outside -- pre (the mean output of each enabled connection's SOURCE node) and eligibility_trace. eff_err is internal to the core and cannot be read from the example, so its contribution is derived by elimination.  Measured (seed 5, the very run whose weights looked 'frozen'): |dw| = 5.44e-2, 1.04e-1, 6.34e-2, 4.51e-2, 1.57e-2 per census -- NOT zero, the weights do move; pre = 0.62, 1.41, 2.99, 1.23, 1.20 -- the sources fire; eligibility = 0.27, 1.62, 1.80, 0.59, 0.57 -- the traces are alive. So [S4]: all three factors are non-zero.  Verdict: the churn of 0.0052 with a live |dw| only means the motion is small RELATIVE to the weight scale -- it exists. What matters is that DIRECTEDNESS is 0.021, i.e. the net shift over an epoch is ~zero while the path travelled is not: a SYMMETRIC random walk, as much plus as minus. So the freeze is NOT a switched-off rule. The rule runs, the sources speak, the trace remembers -- but the SIGNAL HAS NO DIRECTION: update signs alternate and cancel each other. This agrees exactly with Section 56, where raising the volume a hundredfold produced louder noise rather than direction -- because there is no direction to amplify. Both measurements point at the same place: the credit carries no sign.  Section 58: measure the SIGN rather than the amplitude. Section 29 already measured that the winner's detector-to-motor direction is right on 93.3% of ticks while the stuck runs sit at 50.3-59.2%, i.e. the sign DOES separate success from stagnation. If eff_err signs are random, any step yields a symmetric walk -- precisely what we observe. Measure: for each connection, the share of epochs whose net shift sign agrees with the previous epoch's. Near 50% means random signs; above 70% means a direction exists but is wrong. That is the single remaining measurement standing between us and learning.


---

## Commit 9967689 — Section 56: decoupling myelin from the learning step was tried and REVERTED -- eta was NOT the noose. The hypothesis from Section 55 was elegant and code-grounded: consolidation grows structural_mass to the cap of 100 on every success while the learning step is divided by 1 + mass*2 = 201, so eta_eff falls to 0.0006. So the divisor was changed to 1 + mass/100 (at most 2x) in BOTH the main weight update and the neuromodulated (eligibility) update.  Measured: churn did rise (0.005 -> 0.009...0.164), but the SCORE FELL on all four seeds (-4.4 -> -44.5, -1.1 -> -12.6, -0.9 -> -11.9, +2.8 -> -11.5), because the first epoch is now damaged (0.36-0.42 against 0.32-0.34: too large a step wrecks the body at the start), and -- the decisive part -- THE FREEZE PERSISTED: the error again settles to an exact constant with delta = 0.0000 (seed 1: 0.4052 four times in a row; seed 2: 0.3384 three times).  Verdict: eta was NOT the noose. The hypothesis predicted that raising the step a hundredfold would revive learning; the weights did move more, yet learning did not revive. So the freeze lives BEYOND eta, in the chain delta_w = eta * pre * eff_err: the culprit is the SIGNAL, not the step multiplier -- either pre (source output) is ~0, or eff_err (the error reaching the weight) is ~0, or the product cancels exactly. The fact that the error becomes an EXACT constant is the signature of a stationary point where all updates cancel, not of a too-small step.  Reverted the divisor to 1 + mass*2 with a documented 'tried and reverted' note; seed 5 again gives +2.8% (0.1620), so the Section 53 baseline is reproduced for the FOURTH time. 48/48 tests.  Section 57: measure the SIGNAL rather than the step -- decompose delta_w into its factors (pre, eff_err = saturate(node_errors[to]), eligibility_trace) and measure |delta_w| in ABSOLUTE units rather than relative to sum|w| as in Section 55. If eff_err is ~0 the credit never reaches the weights, which is not 'weak learning' but learning that is DISCONNECTED, and the measure will name the broken link directly.


---

## Commit a0e1a04 — Section 55: THE WEIGHTS ARE FROZEN -- and the cause is in the code: myelin divides the learning step. Instrument (examples only, 0 core lines): WeightDynamics samples all enabled connections every 100 ticks and reports three classical quantities per epoch -- CHURN (sum |dw| / sum |w|, the path travelled relative to the weight scale), DIRECTEDNESS (|sum dw| / sum |dw|, the mean shift against the mean magnitude; near zero = pure Brownian noise), and myelin with its per-census change.  Measured (seed 5): churn 0.0050, 0.0063, 0.0101, 0.0035, 0.0012 across the five epochs -- i.e. the total weight path over 50000 ticks is 0.5% of the weights' own scale -- and directedness averages 0.021, i.e. that little motion is directionless. So [W1] THE WEIGHTS ARE FROZEN, while myelin GROWS (+4.1 per census): the body accumulates credit without moving its functional weights.  The cause is visible in the code and it is paradoxical: on every success (err <= tolerance, quality > 0) the consolidation step does structural_mass = (structural_mass + flux*quality*0.25).min(100.0), and the learning step is individual_eta = eta * boost / (1.0 + structural_mass * 2.0). DEFAULT_PLASTICITY_ETA is 0.12 -- not small -- but at mass 100 (the cap) the effective step is 0.12/201 = 0.0006, i.e. 200x smaller. So every success grows myelin, and myelin DIVIDES the learning rate: a self-locking consolidation where success freezes the system. Right after the body achieves anything (error drops under tolerance) the myelin starts growing and thereby switches off its own learning; the measured 'path 0.5%' is the leftover of that plasticity. This is consistent with everything: Section 54 has no learning curve because after the first successes eta_eff goes to zero; Section 55 shows frozen weights with growing myelin; and 'use it or lose it' erases nothing because the weights never moved.  Section 56: DECOUPLE credit from the learning step. The biological role of myelin is structural protection (proven tissue is not pruned); the role of a learning brake is not biological -- it is a side effect of the form 1/(1+2*mass) with mass up to 100. So make individual_eta independent of the mass, or divide by a NORMALISED mass (mass / mean body mass) so that maturation slows learning by a finite factor rather than 200x. Prediction: churn must rise from 0.005 to >=0.05 (weights move again), directedness must exceed 0.1, and the Section 54 learning curve must finally bend downward (last epoch better than the first), validated on n>=9 against the Section 53 base.


---

## Commit d205a56 — Section 54: time epochs -- there is NO learning curve, and the 'advantage' is a cherry, not accumulation. Instrument (examples only, 0 core lines): Epochs slices the run into 5 epochs of 50000 ticks and takes, per epoch, the engine's mean L1 on SCORED ticks (learning curve), the mean hidden somas and closed slots (discovery curve), and the ticks/second plus wall seconds (compute price) with ticks-per-soma. This is only a legitimate measurement now, because before Section 53 the brake reset the body twice per run (Section 52: 96% of losses), so any 'accumulation' was fiction -- we were measuring a lucky snapshot of a random configuration.  Measured. Seed 5 (the +2.8% run): epoch mean L1 = 0.3258, 0.3284, 0.3411, 0.3334, 0.3345 => [T3] NO TREND (slightly worse end to end); slots = 2,1,2,0,2 of 13 (flat). Seed 2: 0.3414, 0.3328, 0.3352, 0.3349, 0.3347 (delta -0.0067) => [T2] weak and non-monotone; slots 5,6,4,3,2 (falling).  Verdict: the error over 250000 ticks does NOT fall -- it wanders around 0.334, i.e. around the constant's level (0.3315). There is no learning curve on either seed, and the slot-discovery curve is flat or falling. Hence the 'advantage' is a cherry: seed 5's best window (0.1621) is selected from ~2700 windows while the GENERAL error sits at the constant's level in every epoch. This finally confirms the protocol defect measured in Section 40 and removes the session's last illusion.  Compute price scales with body size: ticks/s 33327 -> 25494 (-24%) at 11.6 -> 14.6 somas (seed 5), and 25748 -> 48735 (+89%) at 12.6 -> 9.8 somas (seed 2); ticks per soma stays ~330-480k in both, so the overhead is constant and throughput is set purely by cell count.  Section 55: measure WHY nothing accumulates -- weight dynamics. Candidates already known to be real: (a) unused_mass_decay plus 'use it or lose it' erase weights faster than experience accumulates; (b) intrinsic plasticity drifts the tuning genes into noise; (c) the readout lacks degrees of freedom to fix what it has learned. The measure: mean |dw| per epoch against mean |w| -- if plasticity is comparable to the weights themselves, knowledge boils rather than sets.


---

## Commit 2cd2550 — Section 53: pay the energy crisis in TISSUE instead of amnesia -- the biggest shift of the session, and the brake is dead. The crisis handler in loop_runner now, on an energy crisis, runs the Section 38 resorption pass repeatedly (resorption REFUNDS biomass into the vault) until energy climbs above crisis_energy, and only falls back to the champion snapshot when there is genuinely nothing left to resorb. A fatal error (window_err >= fatal_error) keeps its own path, because there the body is not starving but producing catastrophic predictions, which resorption cannot cure. 48/48 tests.  Result on n=9 against the Section 48 base: mean -10.6% -> -2.6% (+8.0 points) and median -10.3% -> -0.9% (+9.4 points) -- the largest shift of the whole session. Five runs now sit within -1.1% (seeds 2, 3, 4, 7, 8), i.e. the typical trajectory now stands right AT the bar, and seed 5 = +2.8% (0.1621) is above it. REVIVALS = 0 on all nine seeds: the emergency brake never fires any more.  The pre-registered mechanism predictions failed again (max alive 8-31, not >40; coverage 2-7, not >=10) while the score moved enormously -- and the reason is now clear: the body stopped being ERASED. Twice per run the amnesia used to destroy 96% of everything grown (Section 52), taking the weights, the myelin and everything the readout had learned with it; now structure and learning survive to the end of the run. So the wall was neither cell count nor slot coverage but the CONTINUITY of the body's existence. This also explains why Section 48's +3.5% appeared on only one seed: there the coin happened to fall so that energy never dipped, while every other trajectory was killed by the brake.  Honest cost: seed 4 lost its +3.5% and gives -0.9% -- on that single trajectory amnesia helped (it restored a champion that happened to score well on the control window), so removing the catastrophe also removed that lucky rescue. And the honest advantage over the constant (mean L1) stayed at about -1.8%, so the gain is in the TAIL of the distribution and in typical runs rather than in the depth of the prediction itself.  Section 54: now that the body no longer gets erased, it finally makes sense to measure ACCUMULATION over time -- does slot coverage grow during a run, does the population grow, and does the error improve from start to end, i.e. is there finally real learning rather than a lucky window.


---

## Commit ecd47c9 — Section 52: THE EMERGENCY BRAKE IS THE MAIN EXECUTIONER -- two firings destroyed 96% of all node losses. Instrument (examples only, 0 core lines): the energy revival has a signature that needs no core changes -- energy JUMPS from below crisis_energy (20E) to revive_energy (60E) -- so the report counts jumps, ticks below crisis, the minimum energy, and the nodes that vanish on each jump.  Measured on seed 4 (the seed that reproduces +3.5%): 2 revivals per run, 48 somas lost on them out of 50 total node losses = 96% of ALL losses; energy was below the crisis threshold for 0.06% of ticks, minimum 14.5E against thresholds 20E (crisis) and 15E (grace).  Verdict [E1]: the brake is the main executioner. The friction measured in Section 47 ('losses = 77% of births') is therefore NOT gradual tissue turnover -- it is TWO CATASTROPHIC AMNESIAS, each replacing the whole body with the champion snapshot and erasing 96% of everything grown.  This explains the whole session's picture: max alive 20-36 but 15-16 at the end (the body grows and is then reset); coverage never accumulating (the table is erased with the body); the huge seed-to-seed variance (whether energy dips below 20E is a matter of luck); and why Section 51's patience change could not reduce friction (it is not the regression trigger).  The fix now follows by itself: the brake responds to an ENERGY shortage with an action that has nothing to do with energy -- amnesia -- even though the engine already owns the right instrument, since resorption REFUNDS biomass (metabolism.reward(form_connection_cost) in the Section 38 soft pass). So Section 53 will make the energy crisis pay in TISSUE: run the soft resorption pass until energy rises above the crisis threshold (bounded by removable links), and fall back to the champion only when there is genuinely nothing to resorb. Prediction: kept nodes stop vanishing, max simultaneously alive somas exceed 40, coverage reaches >=10 slots, and the median moves from -10.3% into positive, validated on n>=9 against the Section 48 base.


---

## Commit 8120421 — Section 51: regression patience tried and REVERTED -- the friction is not there. The change raised EvolutionConfig::regression_windows from 3 to 6 (halving how often the scalpel is drawn), justified by Section 46 having cut the cooldown 20 -> 4 and thus tripled the growth rate. The mechanism did NOT move: node deltas stayed in the same ratio (+82/-65, +94/-87, +108/-92 vs +97/-86, +94/-70, +101/-92 before) and the max simultaneously alive somas did not grow (18-32 vs 20-36). Score on n=7 (seeds 5 and 8 were killed twice by the environment timeout): mean -13.7%, median -17.1% against the Section 48 base mean -10.6%, median -10.3% -- a 6.8 point median drop, and the winner seed 4 was lost again.  Reverted to 3, with a documented 'tried and reverted' note; after the revert seed 4 gives +3.5% (0.1609) for the THIRD time, so the Section 48 baseline is fully restored and its positive is thrice reproducible.  Where is the friction then? Section 37 showed mortality has TWO political paths: (a) empty resorption -> brain = best_brain, which Section 38 cured with the soft pass; and (b) ENERGY REVIVAL (energy < crisis_energy or window_err >= fatal_error -> brain = best_brain + energy := revive_energy, loop_runner.rs:1181-1190), which has never been re-measured since Section 38. Section 52 will measure that second path: how many times per run it fires, how many somas vanish with it, and whether losses correlate with energy dropping below crisis_energy = 20E.


---

## Commit be4d8db — Section 50: the birth-sharpness floor was tried and REVERTED -- wide cells are the coarse basis. The change raised the newborn sharpness floor to the measured one-hot threshold (ONE_HOT_SHARPNESS = 5.0, so s in [5, 30]) in both detector_genes and inherit_from_node's fresh working point. The Section 43 instrument immediately falsified my own prediction of >=80% one-hot newborns: [0.5,5.0] gave 6.2%, [0.5,30] gave 44.8%, and [5,30] gives only 52.1% with the mean symbols per bell collapsing to 0.65 -- i.e. most bells now hit NOTHING, because with high sharpness a random centre in [-1.2, 1.2] often lands outside the symbol span [-1, 1]. So the ceiling is the CENTRE range, not the sharpness.  Score on n=9: mean -10.6% -> -13.2%, median -10.3% -> -17.6%; 6 of 8 seeds worse, including the winner seed 4 (+3.5% -> -18.9%), while coverage gained only one slot (7 -> 8) and max alive somas fell (8-29 vs 20-36). Why: removing the wide cells left the readout with only RARE features (each firing about 1 tick in 13) -- the same lesson as Section 35 (a sparse AND gate adds nothing) and Section 43 (canalising the birth form sets the ceiling, here downwards). Diversity of features is not noise, it is necessary support.  Reverted: the range is again the continuous [0.5, SENSITIVITY_MAX], with the constant kept in the core purely as an instrument reference and a documented 'tried and reverted' note. Reproduction check: after the revert seed 4 again gives EXACTLY +3.5% (0.1609), so the Section 48 win is reproducible rather than an RNG coincidence. 48/48 tests.  Remaining axis: (a) more SIMULTANEOUSLY living somas -- 13 slots need ~41 one-hot at once while the body holds 20-36 somas of which 25-50% are sharp (5-18, a 2-8x deficit), and friction is measured at 77% losses per birth. Section 51: reduce the friction (make the soft pass rarer and/or make birth cheaper), predicting max alive 36 -> 50+, coverage 8 -> >=10 and the honest mean moving from -1.6% into positive, against the Section 48 base.


---

## Commit c1c2c8d — Section 49: the 'weeds choke orchids' hypothesis is REFUTED -- sharp cells are not displaced at all. Instrument: CellLife now records width_birth/sens_birth (state at the first census, i.e. essentially at birth), and the report buckets every life by its birth width with death counts, mean age of the dead, population share, plus the composition of the LIVING set at the end.  Measured (seed 4 = +3.5% and seed 5 = honest +1.7%): sharp cells had ZERO deaths in both runs while wide cells had 2 each (mean ages 65250 and 15950); among the living, sharp cells are 27% (seed 4) and 50% (seed 5); and seed 5's 45.5% sharp share among the tracked lives matches the Section 43 birth model (44.8% one-hot) almost exactly. So there is no attrition of sharp cells -- the fourth of my own hypotheses refuted in a row -- and it agrees with Section 37: there is no per-cell selection at all; deaths are policy (rollback), not competition.  Consequence: exactly ONE axis remains and it is arithmetic. Covering 13 slots needs ~41 one-hot somas simultaneously; the body has 5-18 (20-36 alive x 25-50% sharp), a 2-8x deficit. Two direct levers: (a) more living cells -- losses are 77% of births, and the Section 38 soft pass removes one link per regression event; (b) a higher sharp share at birth (currently 44.8%), which is set by the interaction of the centre range [-1.2, 1.2] (some centres land outside the symbol span [-1,1] and hit nothing) with the sharpness range [0.5, 30]. Section 50 does (b) first -- cheaper, does not touch mortality, and is verifiable by the Section 43 instrument in the same run.  Trajectory of the median: -22.2% -> -17.6% -> (Section 43 dip) -> -13.9% (Section 46) -> -10.3% (Section 48), with seed 4 above the bar and seed 5 honestly positive.


---

## Commit be8626d — Section 48: channel isolation + imprinting -- the FIRST POSITIVES of the session. (48.1) Changes: the micro-column now hears ONE random afferent sensor instead of the sum of all sensors at weights 0.1-0.5; that connection's weight is 1.0 (unity transmission) so the cell's input lives on the SAME SCALE as the signal -- which matters because the archetype samples bell centres in [-1.2, 1.2], i.e. on the data's own scale, while the old 0.1-0.5 weights made the input 2-10x smaller, so a random centre could not coincide with the input even in principle (another scale defect found and fixed); the Section 47 imprint now lands on a CLEAN channel; and three diagnose probes now inject gain*v (the soma's real input scale) instead of the raw symbol. 48/48 tests. (48.2) Result on n=8 (seed 8 was killed twice by the environment timeout -- recorded honestly as an incomplete set): bar advantage mean -14.5% -> -10.6%, median -14.2% -> -10.3%. TWO FIRSTS: seed 4 = +3.5% is the first run ABOVE the bar (0.1725 vs 0.1667) on the Section 43+ substrate, and seed 5 shows the first POSITIVE honest advantage over the constant (+1.7%). (48.3) The pre-registered predictions are only partly met: coverage did NOT reach 10-13 slots (still max 7), the median L1 did not go to 0.0000 and the honest mean did not move to 0.2766 -- but the score distribution shifted up by 3.9 p.p. on both mean and median. (48.4) Verdict: isolation + imprinting is the first change to yield positives, but the coverage ceiling did not fall -- the benefit came from correctly TUNED (though still wide) cells rather than from a complete table, since only 44.8% of newborns are one-hot. The population deficit remains, and a new axis is visible: the SHARE of sharp cells among the living. (48.5) Section 49: measure the distribution of sensitivity among LIVING vs BORN cells (do wide cells crowd out sharp ones in the readout, since a wide cell drives more flux and earns more myelin?), then decide between that and further population growth.


---

## Commit 316154a — Section 47: newborn imprinting implemented -- and it measured EXACTLY ZERO, with the cause measured too. (47.1) Net growth rate extracted from the 9 Section 46 runs: +577 nodes added vs -445 lost => losses are 77% of births; the body is a leaking bucket (mean 64 births per run, three quarters lost immediately). Notably seed 5, with the least friction (+33/-4), also had the best coverage (7 slots), while seed 6 with the most churn (+101/-92) netted only 9 nodes. (47.2) GenomeMutator::imprint_newborn() sets a newborn's threshold (bell center) to the input it actually receives at that instant (sum of weight x source output), applied in BOTH growth paths (node insertion into a connection, and micro-column birth). Justified in code as biological imprinting / activity-dependent differentiation; no data knowledge -- the engine never sees symbols. 48/48 tests. (47.3) Result on n=9 against the Section 46 base: paired difference EXACTLY 0.0 p.p. (4 better, 4 worse, 1 tie), mean -14.5% -> -14.5%, median -13.9% -> -14.2%, coverage max still 7 slots, honest advantage still -1.76%. (47.4) WHY ZERO, and it is a measured cause rather than a failure: the micro-column (mutator.rs:290-296) connects the new cell to ALL sensors with weights 0.1-0.5, so the cell's live input is a SUM (0.3*x_t + 0.3*x_{t-1} + ...), not the symbol value. The imprint dutifully placed the bell center on that MIXTURE, which lands off the 13-slot grid -- in the isolated probe the bell then responds to no symbol at all. This is the same fact measured in 32.5 (a soma's input is already a sum, channels are inseparable) and 42 (even with perfect isolation no soma is a table row), now caught on the imprinting itself: for imprinting to produce a table row the cell must hear ONE sensor, not the sum of all. (47.5) Section 48: imprinting PLUS channel isolation, meaningful only together -- make the micro-column's sensory node hear a single afferent sensor (biologically normal: a cortical column receives one afferent fibre, not 'the sum of the world') and keep the imprint. Prediction: the bell center then maps linearly onto the 13-slot grid, each of ~13 births closes a NEW slot instead of needing ~41 blind draws; coverage must pass 10-13 slots, the median must move toward 0.0000 and the honest mean toward 0.2766, validated on n>=9.


---

## Commit 6b0ccfb — Section 46: the demographic brake removed -- the population grew, coverage only by one slot. (46.1) Measured BEFORE the change: the growth chain at loop_runner.rs:1016-1030 increments stagnation_counter ONLY while ontogenesis_cooldown == 0, so the interval between growth attempts is cooldown + stagnation threshold; with cooldown = 20 windows the body spent 57.7% of the run BARRED from growing (80 cooldown armings, mean interval 3125 ticks). [B1]. (46.2) Change: EvolutionConfig::ontogenesis_cooldown 20 -> 4, justified in code -- the incubation period used to protect against the +1/-1 synapse cycle, but that role now belongs to the soft resorption pass (Section 38) and the juvenile synapse guard, so the pause should apply to the newborn tissue rather than to a stalled organism. 48/48 tests. (46.3) The MECHANISM responded as predicted: max simultaneously alive somas went 22 -> up to 36 (typically 23-28), max grid coverage 6 -> up to 7 slots, the barred fraction fell from 57.7% to 6.6-36%, cooldown armings 80 -> 25-449. (46.4) Score on n=9 against the Section 43 base: mean -18.2% -> -14.5%, median -22.2% -> -13.9% (paired +3.7 p.p., 6 better / 2 worse / 1 tie, sd ~7 -- not significant, but the median shift of 8.3 points is the largest of the session). The honest mean advantage over the constant stayed at -1.8% (was -1.6%), so the predicted move toward 0.2766 did NOT happen. (46.5) Verdict: the valve was real, but it is only half the job -- coverage reached 7 of 13 rather than >=10, and the coupon model agrees: even 36 alive somas (~16 one-hot) predict only ~9 slots, so ~90 hidden somas would be needed for all 13. The cost is measured too: seeds 1 and 7 instead SHRANK (5 and 7 alive) at 449 and 249 armings, i.e. a short pause revives the +1/-1 cycle where tissue has nothing to hold on to. Section 47: measure the NET growth rate (added minus lost nodes) as a function of body size -- if it is positive the body is merely slow, if zero the fix is to make birth cheaper or loss slower.


---

## Commit e38148e — Section 45: population economics -- too few draws, no cloning, and the collector counts the LIVING. Instrument: total_births and max_alive in Demography (new-life detection by fingerprint staleness), cumulative +added/-lost nodes from the morphogenesis branch, and slot_occupancy() giving empty/x1/x2/x3+ slots.  Measured (seeds 3 and 8): cumulatively born hidden somas 33 and 55 (cross-check via morphogenesis: +39/-21 and +61/-43 nodes), max simultaneously alive 22, alive at the end 15 and 16, one-hot at the end 6 and 4, slots empty/x1/x2/x3+ = 8/4/1/0 and 9/4/0/0.  Two verdicts: [E2] the ceiling is in BIRTH, not retention (33-55 born vs 22 alive -- only 1.5-2.5x, not hundreds vs twenty); and [K3] CLONING DOES NOT DOMINATE, refuting my own Section 44.5 hypothesis #3 -- zero slots hold 3+ somas and 8-9 are empty, so the centers are spread uniformly and the local search does not cluster them.  The coupon-collector model fits precisely again, but on the LIVING set: 6 and 4 one-hot somas alive predict 13*(1-(12/13)^k) = 4.9 and 3.5 slots; measured 5 and 4. So coverage is determined not by cumulative draws but by how many one-hot somas the body holds AT ONCE.  Quantitative verdict: closing 13 slots needs ~41 one-hot somas alive simultaneously; the body holds 15-22 somas total with 4-6 one-hot -- a 7-10x deficit in count and ~4x in body size. And this is NOT metabolism: energy sits pinned at the 999.9-1000E ceiling, so the body is not starving, it simply does not accumulate size -- morphogenesis adds +39..+61 nodes while losses remove -21..-43, i.e. births ~= losses and the population oscillates at 15-22 instead of growing.  Section 46 will ask what holds that equilibrium (soft resorption per Section 38, morphogenesis cooldown, or maintenance tax), with a pre-registered prediction: push capacity so the body accumulates >=40 hidden somas simultaneously and coverage must exceed 10 of 13 slots, the honest mean must move toward 0.2766 and the median toward 0.0000, validated on n>=9.


---

## Commit 2d91f11 — Section 44: grid coverage measured -- and it refutes my own Section 43.4 hypothesis. Instrument (examples only): symbol_coverage() counts how many of the 13 symbol values have a soma responding to EXACTLY that value; traced every 100 ticks together with the hidden count, plus max-ever, sharp-drop count and time-share above thresholds.  Result (seeds 3 and 8): the MAX EVER coverage over a whole 250k run is 6 of 13 (at ticks 186200 / 164500) with up to 22 hidden somas; time spent with >=10 slots is 0.0% on both; sharp coverage drops (>=3 slots in 100 ticks) are 1 and 0. So the rollback is NOT eating the table -- the table was never built (contradicting my Section 43.4 claim), and 22 somas yielded only 6 slots.  The real name of the problem is the COUPON COLLECTOR: covering all 13 slots by uniform random draws needs 13*H13 ~= 41 draws; the body has <=22 hidden somas of which 44.8% are one-hot => ~10 effective draws, whose expected coverage is 13*(1-(12/13)^10) ~= 7.2 -- measured 6. So the body behaves as an honest coupon collector and is short by roughly a factor of four.  Verdict: the table is unfinished not because of shape, policy or sharpness, but because of the NUMBER OF DRAWS. Section 45: measure cumulatively-born hidden somas vs simultaneously-alive (is the ceiling birth or retention?), and whether the local search (donor.threshold x U(0.8,1.25) for half the newborns) clusters the centers instead of spreading them.


---

## Commit 2635f1f — Section 43 executed: widen the birth sharpness range past the one-hot threshold (SENSITIVITY_MAX = 30.0, applied to birth, fresh working point, inheritance ceiling, micro-mutation, intrinsic plasticity, and both diagnose probes). Prediction 1 CONFIRMED spectacularly: one-hot newborns went from 6.2% to 44.8% (mean symbols per bell 4.14 -> 1.17), and real table rows now appear in the champions (1-4 of 3-21, i.e. 10-33%; seed 3 was 0 of 7 before). Predictions 2 and 3 FALSIFIED on n=9: the median L1 does not fall to 0.0000 and the mean does not move to 0.2766 -- instead the distribution worsened (mean -13.5% -> -18.2%, median -17.6% -> -22.2%) and the honest advantage over the constant is now negative in ALL nine seeds (-0.4% to -5.4%, where the base held +0.1%). Paired mean -4.6 p.p. with sd ~11 => not statistically significant, but the sign is the same in 7 of 9.  Verdict: a table row is NECESSARY BUT NOT SUFFICIENT. The table needs 13 rows and the champion has 1-4, so it is 8-30% filled: any input without a row falls back to the constant and eats the whole gain. Coverage, not shape, is the next axis. Section 44: measure grid coverage (how many of the 13 symbol values have a detector responding to exactly one), as a function of body size and of one-hot detector lifetime -- i.e. is the ceiling the growth rate or the Section 37/38 rollback policy?


---

## Commit 44cfa11 — Section 42: THE ROOT CAUSE, MEASURED AND REDUCED TO ONE NUMBER. (42.1) Live line added to 30.6: buckets of top-input-share vs symbols-per-bell vs collisions -- seed 3 has ZERO detectors with a dominant (>=85%) source, so isolation cannot even be tested live. (42.2) Counterfactual probe: each hidden soma swept over the REAL 13 symbol values with a perfectly isolated channel => of the champion's 7 somas, 0 respond to exactly one symbol (2 dead, 1 at three, 4 at five+). So the villain is NOT the summation -- it is the response form. (42.3) THE NUMBER: the 13 symbol values are evenly spaced at 0.1667 (min = median). A bell exp(-((x-c)s)^2) against that grid responds to 12.08 symbols at s=0.5, 7.46 at s=1, 4.54 at s=2, 2.85 at s=3, and exactly 1.00 at s>=5.0. detector_genes samples sensitivity in [0.5, 5.0] -- the upper bound of the birth range IS the one-hot threshold, with zero margin. A newborn detector is a table row only if its sharpness lands exactly on the boundary; at s=4.9 the bell already catches TWO symbols, at s=2 five.  This single number explains everything measured: 25's 26% 'narrow' newborns are narrower than 0.25 but WIDER than the 0.167 grid step (2-3 symbols); 31's median of 5 symbols per bell matches the table's 4.54 at s=2 exactly; 31.5's 7-of-13 unambiguous successors cannot reach detectors that do not separate symbols; 33/35's 'the receptive-field form is not the wall' is right -- the wall is the FORM'S PARAMETER; 41's table needs rows and none are ever born.  Section 43 (falsified prediction): widen the birth sharpness range past the threshold with margin (e.g. [0.5, 12.0]; the mutation clamp is already 10.0). Then (1) the one-hot share among newborns must become > 0, (2) the engine's median L1 must fall from 0.2120 toward the table ceiling 0.0000, (3) the mean must move from 0.3313 toward the measured table ceiling 0.2766, (4) validated on n>=9 seeds per Section 36.


---

## Commit a2ad2e8 — Section 41: the honest deep-lag grid kills the lag hypothesis, and the conditional-median/table probe finds the real vein. (41.1) My own FOURTH ruler defect: Section 40 measured the lag gain against the MEAN-constant (0.3707), but the best constant under MAE is the MEDIAN-constant (0.3315) -- the mean-constant is 12% worse, so any model 'gained' 10% merely by learning to output something closer to the median. With the honest baseline the deep grid (depths 1..16) shows in-sample gains from -10.8% to +0.1% and test mean L1 from 0.3673 to 0.3313 -- the lag models are WORSE than the best constant at every depth and merely converge to it at 16. The 'monotone positive law' of Section 40 was a baseline artifact; I retract it. (41.2) Conditional-median probe (the rigorous closure): 13 input classes, 6 unique conditional medians, spread 1.8333 => [M2] the L1-optimal predictor DOES move with the input, so L1 gain exists in principle -- it is simply not reachable by LINEAR functions of a SCALAR input. (41.3) TABLE predictor (train 60% / test 40%, 16001 held-out ticks, 100% coverage): median L1 = 0.0000 and mean L1 = 0.2766 vs the constant's 0.1667 / 0.3315 => gain +100% by median and +16.6% by mean. So the bar is not merely beatable -- it collapses to ZERO: knowing only the current symbol, the next one is named EXACTLY for more than half the ticks. (41.4) Verdict: the wall is FEATURE FORM (linear vs table), not context depth. This fits Section 31/33/37 exactly: a narrow Bell IS a table/one-hot detector, 26% of newborns are narrow, but they collide because the soma's input is a SUM (Section 32.5) -- summation destroys the table. Section 42: measure whether a soma can get input dominated by ONE sensor, and whether dominance kills the collision rate.


---

## Commit 62f62e1 — Section 40: the honest criterion and the lagged oracle -- and the two metrics give opposite verdicts, which explains the whole session. (40.1) Like-for-like comparison on the FULL run (250000 scored ticks, same statistic, same cycle): engine 0.3313/0.2120 vs constant-median 0.3315/0.1667 => advantage +0.1% by MEAN and -27.2% by MEDIAN. The protocol defect is now a number: 'window error 0.1759' is the MINIMUM over ~2700 windows while the bar 0.1667 is a whole-cycle MEDIAN -- incomparable. Honestly compared, the engine merely TIES the constant by mean (i.e. adds no skill) and loses 27% by median.  (40.2) Lagged oracle, same grid, two fits (LAD for L1, OLS for the mean), lag sources separately sensor (s_t, s_{t-1}...) and target (y_{t-1}...). Under the BAR'S METRIC (L1/median): in-sample gain +0.0% / +0.2% / +0.2% / +0.4% for depths 1..4 -- the conditional MEDIAN does not move at ANY depth, so the bar 0.1667 is unbeatable constructively by any predictor, linear or not, with or without memory. Under the ENGINE'S METRIC (mean L1): gain +0.9% / +2.1% / +3.1% / +4.6% -- MONOTONE INCREASING with context depth, test mean 0.3673 -> 0.3537. This is the first structured positive dependence in the entire session: history depth buys predictability. Even so, 4 lags do not reach the honest mean floor 0.3315, and the engine already ties that floor (0.3313) without capturing the 4.6%.  Conclusion: the criterion must change to mean L1 over the full cycle against the floor 0.3315, and the axis of work is CONTEXT DEPTH, not feature shape -- Section 34 already showed State is a single register, so history must be distributed along a chain (the 'k >= 2 needed' measurement from Section 31). Section 41: extend the lag grid to 6/8/12/16 to find the depth where the mean-metric gain crosses the floor.


---

## Commit 6cd81bc — Section 39: the linear readout oracle (DeepMind-style feature probe) finally localizes the wall -- and it is the METRIC, not the body. Instrument (examples only, 0 core lines): collect hidden-soma outputs H and targets Y exactly on the ticks the engine itself scores (detected via window_ticks increments, which only happen in the action phase), then replace the motor readout with the exact solution. Three feature pools, two oracles (somas only; sensors+somas).  Two ruler defects were found and fixed on myself before trusting the verdict: (1) the first oracle was OLS, minimizing L2 while the engine is judged by L1, where the optimal constant is the MEDIAN -- that alone made 'representation failure' look true; a LAD/IRLS oracle was added. (2) The first LAD version pinned the intercept to the MEAN, which produced a mathematically impossible negative in-sample gain (-1.8%) -- that is what exposed it. Now the intercept is free and the fit keeps the best-so-far solution, so the oracle is provably never worse than the best constant in-sample (gain >= 0, as it must be).  Result (seed 3, 40001 scored ticks, 23 somas): IN-SAMPLE information gain over the best constant is +0.0% for the somas and +1.1% for the ENTIRE periphery (sensor value + every soma). Out-of-sample the soma oracle lands exactly on the constant (0.3323/0.1750 vs 0.3315/0.1667). So the omniscient weights converge to a constant: not one feature adds predictive information.  Verdict [T]: the bar 0.1667 is not a dumb heuristic but the L1 optimum for a mode-dominated target -- any predictor that deviates from the median on those ticks makes the median worse, so the bar is unbeatable by construction under this statistic. And the comparison protocol itself is invalid: the engine's general (non-cherry-picked) error is 0.3301/0.2015, i.e. 0.4% better than the constant -- the engine does exactly nothing -- while 'best window error' 0.1759 is the MINIMUM over ~2700 windows (a cherry) compared against a whole-cycle MEDIAN.  This explains why 18 of 18 configurations landed between -1.9% and -22.5%: they were all competing with a mathematical optimum, not with a dumb baseline. Section 40 must first redefine the success criterion as a like-for-like comparison (same statistic, same cycle) and only then test whether context memory adds anything (lagged oracle over [x_t, x_{t-1}, x_{t-2}]).


---

## Commit bf554bb — Section 38 (option b): soft resorption pass replaces the nuclear rollback. When the strict pass finds no idler AND everything is proven -- the exact condition that triggered 'brain = best_brain' and erased the whole body -- one weakest link is now shed instead (min structural mass, tie-break min flux), with the same physical guards: juvenile protection and the motor cannot be left without input. At most ONE link per event (margin pruning, not amputation), so the rollback survives as a genuine last resort. Policy (loop_runner) untouched. 48/48 tests (2 new: soft pass sheds the weakest and report is not empty; soft pass respects juvenile protection). Telemetry: ResorptionReport::soft_dissolved -> last_resorption_soft -> diagnose line.  Anatomy changed radically: seed 1 went from 3 morphogeneses to 126, seed 6 shows 45 soft passes and 74 morphogeneses -- the 9->6 limit cycle is broken and the body is no longer reverted to the champion snapshot.  Score did NOT move beyond noise: mean -16.4% -> -13.5% (paired +2.9 p.p., sd of differences ~10, 3 improved / 2 worse / 4 identical); 0 of 9 seeds above the bar exactly as before. Best runs now sit right at the edge (-1.9%, -4.4%) but none crosses.  Verdict: this is the fourth genuine defect peeled off (receptive-field form -> dimensionality -> tissue selection -> policy) and none of them explains the failure to learn. The question is now correctly reformulated: the body DOES grow many organs (74-126 morphogeneses) and still loses to the median, so the wall is in how existing features are turned into a prediction -- readout / learning / metric.


---

## Commit d149c85 — Section 37: cell demography refutes the 'silence tax executes snipers' hypothesis AND locates the real ceiling. Instrument (examples only): census every 100 ticks keyed by quantized-coordinate fingerprint (verified: pos_x/pos_y are set at birth and never mutated, so identity is stable under autophagy reindexing), recording age/response_width/recent_activity/degree. Deaths are classified as mass (>= half the body) or rollback-consistent (body composition becomes a subset of best_brain).  First pass looked like support: seed 3, narrow cells died 77.8% vs 62.1% for wide. But the same probe by loudness kills it -- the quietest cells (<1e-4 of body peak) died LESS (60% vs 66%), and dying cells were louder and less connected.  Correct attribution: 0 of 52 deaths on seed 3 are tissue selection; 100% are champion rollbacks. Independently confirmed by the engine's own counters: EVERY node-loss event logs 'resorbed 0s/0somas' -- tissue resorption removed zero somas in the whole run, autophagy never fired. seed 3 shows 15 consecutive 9->6 cycles: grow 3 organs, regress, resorption report empty, whole body replaced by the champion snapshot.  The paradox: the rollback fires MORE certainly the better the tissue has proven itself (an empty report means nothing was unproven), so success guarantees erasure -- the body can never accumulate organs beyond the champion snapshot. This independently confirms Section 26 and corrects Section 35.4's mechanism (quietness was measured right, the executioner was wrong). Section 38 is pre-warned: removing the rollback is already falsified (Section 21: deadlock, -17.1%), so the fix must change WHAT it rolls back, and must be validated on n>=9 seeds.


---

## Commit 450a737 — Section 34.5 executed: add the InputPrev physical delay channel (engine already held it in NodeGene::last_input; the opcode just stops hiding it). The conjunction bell(x_t)*bell(x_{t-1}) is now expressible and was born for the first time in the engine's history (0% -> 6.9% of random programs effectively 2-D; 6 of 16 somas in seed 2's body). Falsified anyway: the forced 2-D birth archetype is harmful (seed 2 -22.5% vs -10.1% with 1-D archetype) because an AND-gate is 7-222x quieter than its neighbours and the silence tax/autophagy kills it -- the substrate is anti-conjunctive. Kept the channel, reverted the archetype. The headline is Section 36: a 9-seed x 2-config sweep shows the baseline +4.4% was a +2-sigma tail of a distribution whose mean is -12.4% (sd 8.2), and one extra opcode in the mutation space alone moved seed 1 by 8.8 points. The engine loses to the trivial median in 17 of 18 trajectories.


---

## Commit a369833 — Champion 2-D audit (scenario A): ZERO effectively 2-D cells in every champion and every body (0 of 60 across 3 seeds) while 14/16, 3/3, 17/20 structurally read State -- so the +4.4% win is a lucky 1-D projection, consistent with the collision rates (33% vs 82%). And node.rs:170-174 shows State IS the program's output register, so a soma can hold history OR compute with it, never both: the 2-lag conjunction is not expressible at all, which CORRECTS Section 32.5's hypothetical 2-D numbers (real ceiling is zero). Minimal fix would be an engine-maintained InputPrev channel.


---

## Commit c0a9c94 — 2-D receptive field capacity in the instruction space (0 core changes): 7.9% of random programs effectively depend on BOTH Input and State, 0.3% (~1 in 297) form a narrow 2-D blob -- so the instruction space CAN express a conjunction. But the canalized birth archetype [Input, Bell] has ZERO State dependence (control), i.e. every newborn detector is strictly 1-D. Ceiling is the birth form, not the opcode set.


---

## Commit 34f5e4f — Out-of-sample data ceiling (no core change): honest k=1 floor is 0.3333 (worse than the dumb bar 0.1667), so the proposed 'read one sensor' fix is REFUTED; k=2 gives 0.0000, so the minimum context is two lags. Per-symbol: only 7 of 13 symbols have an unambiguous successor and they cover just 14% of the text. The real substrate defect is that the bell is a function of ONE scalar (a weighted sum), i.e. a 1-D slice of a 2-D context, which mathematically must collide.


---

## Commit 7956b03 — ROOT CAUSE FOUND: feature polysemy. Detector bells fire on a median of 5 different symbols (max 13); in the stuck seeds the symbols on one bell demand OPPOSITE motor directions with comparable mass, so the feature's sign ceiling collapses to a coin flip (50.7% / 58.3% vs 95.3% in the winner). Learning is NOT at fault: it captures 98-102% of that ceiling in all seeds. Phase shift refuted (0-tick path). Two ruler caveats documented (live-output confound; in-sample physical_floor).


---

## Commit eee4f7d — Credit-assignment probe (detector->motor synapse): the SIGN of the detector voice separates the winning seed (93.3% correct direction) from the stuck ones (50.3%, 59.2% = chance), while SNR does not (the winner has the LOWEST SNR). Ruler defect #7 fixed (two-hop chain through the column's intermediate soma).


---

## Commit 476ac55 — Median-vs-mean instrument: the 'median is blind to local improvements' hypothesis tested and inverted (median moves 1.5-2.8x more, per-organ 8 vs 1; under the mean metric the brain would score -34%). Ruler cross-check: 0 mismatches in 2747 windows.


---

## Commit 7f3df46 — Telemetry: incubation, organ anatomy, feature profile and detector fate. Detector death traced to two wholesale-rollback paths (empty resorption / energy revival); the rollback fix was implemented, measured negative (+4.4% -> -2.8%, seeds 2-3 byte-identical) and reverted. Core untouched.


---

## Commit 1ba1523 — Growth-lifecycle telemetry; selection anchor vs honest record; four Class B hypotheses falsified

Engine (src/): separate the HONEST record (best_window_error) from the elastic SELECTION ANCHOR (anchor_error) with annealing (§22); node viability (tunable gene + nonlinearity) and an activity trace so newborn cells can become real features instead of grey duplicates; juvenile protection for newborn synapses; local resorption instead of whole-brain rollback; dormant-node autophagy; ambiguity detector driving directed growth.

Observer (examples/diagnose.rs, zero lines in the engine): IncubationProfiler measures the body level before growth, in the first full window with the new organ, its minimum over grace and at grace expiry, with thresholds taken from the engine itself (§23). Organ anatomy: form, efferent output weight, motor/sensor reachability, survival, output flux and its drift, with node identity by quantised coordinate fingerprint because autophagy reindexes ids (§24). Feature profile: response shape measured by an independent 33-probe ruler over an extended range, centre lever and drift, and coverage of the real input field (§25).

Measured (bukvar, 250k ticks, seeds 1-3; runs must reproduce byte-for-byte): §22 anchor relaxation lifts the best case from +0.7% to +4.4% (0.1593). §23 falsifies 'grace is too short': the birth shock (+0.0125...+0.0288) is fully recovered within the 20 grace windows, net approx 0, so 'not enough time' is the smallest class in every run. §24 falsifies topology and gradient: 0 of 115 organs land in a blind alley, and a strictly zero output weight does not block learning because the weight update is not multiplied by the weight (brain.rs). §25 falsifies feature shape: 25-35% of new somas are narrow, correctly aimed detectors, yet the champion retains 0 of 3 (seeds 2,3) or 4 of 16 (seed 1) - the loss happens between birth and survival.

Docs: HARDCODE_AUDIT_NETWORK_CONSTRUCTION.md §23-§25 plus S-block updates (proved / disproved / next step). Verification: cargo test 44/44, cargo check --all-targets clean with no warnings; diagnose output byte-identical (0.1593 / 0.1959 / 0.1982).

---

## Commit c4a3391 — Refactor engine: honest metrics, directed growth, ambiguity detection and structural cleanup


---

## Commit de101fe — feat: universal dataset adaptability and model checkpoint save/load


---

## Commit d08c76d — feat: zero-hardcode living connectome engine with verified 26/26 crystallization


---
