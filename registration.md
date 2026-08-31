# Silicon Sample Benchmark — method registration form (completed, D_T1_secondary-3)

## 0 · Approach identity and output
- **0.1 Team ★** — team_17 — Steve Rathje (Carnegie Mellon University, srathje@andrew.cmu.edu, ORCID 0000-0001-6727-571X) and Dan-Mircea Mirea (Princeton University, dmirea@princeton.edu, alternate danmirea@sas.upenn.edu, ORCID 0000-0002-4349-7059). This entry's pipeline was built and run by D.-M. Mirea via an automated Codex CLI agent; the team's other entries (secondary-1/-2 and the primary) use an independent Claude-based pipeline by S. Rathje.
- **0.2 Plain-language summary ★** — We simulate 9,000 synthetic U.S. respondents with an OpenAI model: quota-matched demographic-only profiles complete the full post-message survey in batched sessions, one condition each. A pre-registered, condition-blind block-mixture replaces half of each intervention's respondents with their own matched-control responses, implementing an externally validated 0.5 effect calibration at the individual level.
- **0.3 Submission tier & approach family ★** — Tier 1; per-respondent simulation; single model (gpt-5.6-sol); matched-control block-mixture calibration; literature-informed, zero-shot.
- **0.4 Pipeline diagram** — official materials + evidence library -> locked prompts (method/PROTOCOLS_AND_PROMPTS.md) -> isolated batched respondent sessions -> schema validation -> deterministic post-processing (block-mixture calibration, composite computation) -> prediction file(s).
- **0.5 Coverage ★** — 9,000 respondents: 500 × 16 interventions + 1,000 control; one condition each. Full coverage confirmed: all 16 interventions and all 13 outcomes (+ control where required).

## A · Scope of LLM use
- **A.1 Purpose** — LLM generates individual survey responses; all cleaning/aggregation/calibration deterministic code; Codex agent orchestrated the build.
- **A.2 Degree of automation ★** — Fully automated at prediction time; no human selection, editing, or review of predicted values. Human input limited to pre-registered design/budget approvals.

## B · Model / system details
- **B.2 Access & context mode** — Codex CLI (`codex exec`, codex-cli 0.151.0-alpha.7.1), ChatGPT-login allowance (no API key), ephemeral read-only sessions, strict JSON-Schema output, 600s timeout; calls 2026-08-30/31 (timestamps in raw logs).
- **B.3 Configuration** — low reasoning effort; temperature/top-p/provider seed not exposed (unset; disclosed). Deterministic construction seed 20260830; SHA-256 tie-breaking.
- **B.4 Customization** — None: no fine-tuning, retrieval, web, or tools at prediction time (zero tool-use events recorded).
- **B.5 Persistent memory** — None; ephemeral isolated sessions; base methods cannot read each other's outputs.
- **B.6 Inference stack** — N/A (hosted).
- **B.1 Model name(s)** — gpt-5.6-sol (OpenAI hosted, 2026). Orchestration: Codex desktop/CLI agent.
- **B.7 Ensembles** — None (one generation per respondent).

## C · Prompts
- **C.1 Exact prompts** — Verbatim locked templates in method/PROTOCOLS_AND_PROMPTS.md; per-call rendered prompts in method/raw_model_logs.tar.gz. Pre-specified and hashed before target prediction; not refined against target outputs.
- **C.2 System-wide instructions** — The SYSTEM CONTRACT blocks inside the locked templates; no other system prompt.
- **C.3 Prompt-design rationale** — Ordinary-respondent/calibrated-forecaster contracts; independence and variance-preservation instructions; evidence-conditioning per method/EVIDENCE_MEMO.md.

## D · Persona / profile construction (Tiers 1–2)
- **D.1 Profile source** — Fully synthetic: 500 quota-matched core profiles via iterative proportional fitting to the official gender×age and gender×race cross-quotas; education/income/party margins from method/MODERATOR_WEIGHTS.csv; state included. Plus 500 independently drawn control profiles. No microdata; no latent attitudes (validation did not justify them).
- **D.2 Profile verbalization** — Compact canonical JSON, demographics only (profile_id, state, gender, age, race, education, income, party); no biographies or condition-dependent traits.
- **D.3 Assignment & weighting** — Matched design: the same 500 core profiles reused in every intervention; control = 500 core + 500 fresh (1,000 total; fillers 334/333/333 by seeded shuffle). Each respondent exactly one condition.

## E · Stimulus and survey administration
- **E.1 Stimulus presentation** — Verbatim official texts; one condition/text per call; Extreme-weather profiles grouped by exact rendered state branch (each record sees only its own branch); control batches contain only the assigned filler.
- **E.2 Survey walk-through** — One persistent post-message session per batch (2×250 per intervention; one call per control filler; 35 calls): message -> 12 trust items -> remaining scored blocks in official order -> verbatim newsletter offer page -> signup. Unscored content omitted.
- **E.3 Response elicitation** — Strict JSON Schema: compact fixed-order integer arrays (one per profile); code expands, checks row width and ID uniqueness; composites computed in code, never by the model.

## F · Stochasticity and aggregation
- **F.1 Runs & seeds** — One generation per respondent (9,000); construction seed 20260830; provider sampling not seedable (B.3).
- **F.2 Aggregation rule** — None at respondent level; composites computed deterministically from items after calibration (G.3).

## G · Validation & post-processing
- **G.1 Human validation** — None of outputs; numeric gates only.
- **G.2 Post-processing** — Schema/row-width/ID checks with the frozen retry rule; integer formats enforced; composites recomputed after calibration; effective N=500/intervention, 1,000 control (per-call accounting in raw logs).
- **G.3 Calibration corrections** — Locked 0.5 effect calibration implemented as a condition-blind matched-control block mixture: per intervention, the 250 core IDs ranked by SHA256(condition|profile_id|calibration) have their complete response block replaced by their independently generated matched-control block (all-or-none, preserving coherence/integers/newsletter dependency); expected effect halved; controls untouched. Selected on held-out external validation (replayed final-method diagnostics: pooled r 0.218, RMSE 1.671; method/EXTERNAL_VALIDATION_RESULTS.md).

## H · Learning and conditioning components
- **H.1 Fine-tuning data** — N/A.
- **H.2 Context & retrieval corpora** — Frozen in-prompt evidence/baseline tables only (method/EVIDENCE_LIBRARY.csv, MODERATOR_WEIGHTS.csv); no retrieval index.

## I · Data inputs, blinding, and competing interests
- **I.1 Competing interests ★** — No project funding; model access via the member's ChatGPT/Codex plan allowance (OpenAI). No other LLM-interested relationships.
- **I.2 External human data †** — Published sources only, itemized in method/EVIDENCE_LIBRARY.csv (30 records: Vlasceanu 2024; Ashokkumar/Hewitt 2026 Nature; Rode 2021; Goldwert 2026; Geiger 2024; CCAM 2025; Voelkel 2026 materials; etc.); one held-out published study used for validation (method/EXTERNAL_VALIDATION_SPEC.md).
- **I.3 Blinding attestation ★** — We attest that no team member accessed, solicited, or was shown any human outcome data from this study, including pilots and the preliminary-results talks, before the prediction lock. — team_17, 2026-08-31
- **I.4 Contamination note †** — gpt-5.6-sol (OpenAI, 2026; training cutoff not published) postdates the public benchmark materials; sealed human outcomes cannot be in training data. The validation study is published and may be in training data — disclosed in method/EXTERNAL_VALIDATION_SPEC.md as a leakage limitation.

## J · Internal selection procedure
- **J.1 Design-space search †** — One locked configuration per tier, chosen via a pre-registered external validation on one held-out U.S. climate-messaging study (16 ATEs, 23 calls, $1.72 API-equivalent): pooled/within-outcome Pearson, Spearman, direction, RMSE, calibration computed per method (method/EXTERNAL_VALIDATION_RESULTS.md). Frozen decisions: 0.5 multiplier on treatment-minus-control effects (all tiers; block-mixture at Tier 1), k=3 medians (Tiers 2-3), no rank pass, no cross-method ensemble; Tier-2 chosen as this pipeline's best method by the locked RMSE tie-break (the team's overall primary is a different pipeline's Tier-3 entry). Full lock + hashes: method/DESIGN_LOCK.md, method/SHA256_MANIFEST.csv.

## K · Reproducibility & frozen artifacts
- **K.1 Code & materials** — Full pipeline code (method/scripts/), locked protocols, evidence files, and design lock in this deposit; no secrets; construction seeded (20260830); model sampling not seedable (B.3).
- **K.2 Raw output logs †** — Complete raw JSONL event streams, stderr, parsed outputs, prompts: method/raw_model_logs.tar.gz (public, in-deposit). Frozen retry rule: at most 2 retries on transport/timeout/rate-limit/schema/duplicate-ID/missing-field/nonfinite/arithmetic failures, identical prompt + validation error attached; valid responses never rejected for surprising values; every prompt/stream/parse/retry/refusal/clip/exclusion retained (method/raw_model_logs.tar.gz).
- **K.3 Computational resources** — 35 target calls (gpt-5.6-sol; 2×250/intervention + 3 control batches), ~0.8-1.2M input + 0.65-1.05M output tokens; cash cost $0 (ChatGPT/Codex plan allowance; never purchased credits), API-equivalent planning value $16-26; plus 23 validation calls ($1.72 API-equivalent). Runtime ~12-30 min at 4 concurrent.

## L · Disclosure class
Class **A · Open** — all items public; nothing escrowed or withheld.
