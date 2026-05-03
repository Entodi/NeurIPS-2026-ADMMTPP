# AD-MMTPP — Publication Plan (OKR-style)

NeurIPS 2026. Target submission **2026-05-31**. Today: **2026-05-03** (~4 weeks).

## Top-level Objective

Ship AD-MMTPP as a defensible paper supporting the four contribution claims below — **and nothing else**. Every experiment, table, and appendix subsection must defend one of these four claims. Items that defend none go to `followup.md`.

## Stop rule

Before adding any new experiment or appendix subsection, name which claim it defends in one sentence. If you can't, do not run it for this submission. Append it to `followup.md` and move on.

---

## OKRs by Claim

### Claim 1 — Objective

> **Instantiate the LLM-coupled MTPP formulation for multimodal clinical event streams, with explicit treatment of the discretization the language-model tokenizer induces on the continuous mark space.**

| Key Result | Where | Status | Effort |
|---|---|---|---|
| §3.1 derives intensity-as-limit (Eq. 1), survival argument, joint NLL, tokenizer-cell discretization (Eq. 5) | `main.tex` §3.1 | **done** | — |
| Token-level CE established as discretized NLL surrogate, citing kong2025 + liu2025 | `main.tex` §3.1 | **done** | — |
| Aux-head formulations match the brainx implementation (Eqs. 6–13) | `main.tex` §A.1 | **done** | — |

No new experiments. The math defends the claim; code-to-math correspondence is in §A.1.

### Claim 2 — Objective

> **Demonstrate the first controlled tokenization-granularity ablation in the LLM-coupled TPP family — visit (V), modality (M), atomic-event (A) — at fixed backbone, adaptation, and training recipe.**

| Key Result | Where | Status | Effort |
|---|---|---|---|
| V/M/A BCA on TADPOLE D1 (5-fold mean ± std) + D2 rollover | §5.2 table | sweep done; aggregate pending | 1 day |
| V/M/A on EDUCATE binary + 5-way (leak-safe) | §5.2 table | sweep done; aggregate pending | 1 day |
| Two-visit example showing V/M/A side-by-side | `main.tex` Fig. 1 | **done** | — |

### Claim 3 — Objective

> **Show that survival (Cox partial likelihood, two pooling variants) and ordinal cumulative-link auxiliary heads — to our knowledge first in an LLM-coupled TPP — function as representation-shaping objectives without serving as the primary predictor.**

| Key Result | Where | Status | Effort |
|---|---|---|---|
| Aux-head ablation: pure / Hawkes / Cox / Cox-acc / Cox-attn / ordinal × {V, M, A} on TADPOLE | §5.2 table | 192-model sweep done; aggregate pending | 1 day |
| Same ablation on EDUCATE | §5.2 table | sweep done; aggregate pending | 1 day |
| ECE + reliability for best (encoding, head) pair vs pure-text baseline at one horizon | §A.8 | computable from existing predictions | 0.5 day |
| All head formulations specified in §A.1 and matched to `brainx` code | `main.tex` §A.1 | **done** | — |

### Claim 4 — Objective

> **Empirically validate the approach on two ADRD cohorts (TADPOLE/ADNI and the tertiary-care EHR dataset EDUCATE), beating at least one strong time-collapsing baseline and one closest-prior LLM-TPP comparator.**

| Key Result | Where | Status | Effort |
|---|---|---|---|
| Headline TADPOLE BCA + D2 rollover, best config | §5.2 table | aggregate from sweep | 1 day |
| Headline EDUCATE binary + 5-way under leak-safe preprocess | §5.2 table | aggregate from sweep | 1 day |
| Cerebra-style max-pool baseline (XGBoost/MLP on time-collapsed features) | §A.5 Tier 1 | runnable from preprocessed features | 1 day |
| TPP-LLM comparator on the same splits | §A.5 Tier 1 | public code, fine-tune on both cohorts | 2–4 days |
| Code-leakage gap on EDUCATE (leak-safe vs leak-permissive) | §A.7 | sweep already covers both; aggregate | 0.5 day |

---

## Sprint

| Week | Dates | Focus |
|---|---|---|
| 1 | May 3–10 | Aggregate existing sweep → the 4 headline tables. Delete all deferred placeholders from §A.6 / §A.7 / §A.8 (see Cut list). Lock §3.1 + §3.2 prose. |
| 2 | May 10–17 | Run Cerebra max-pool baseline + TPP-LLM comparator. Compute ECE + leak-gap tables. Draft Abstract + Conclusion. |
| 3 | May 17–24 | SHOULD list if budget permits (see below). Otherwise polish prose, figures, references. |
| 4 | May 24–31 | Internal review pass, figure polish, references audit, submission. |

---

## SHOULD include if cycles remain

| Item | Defends | Why optional |
|---|---|---|
| Cross-cohort transfer TADPOLE ↔ EDUCATE | strengthens Claim 4 | strongest single generalization signal but not load-bearing |
| DX-hidden ablation | Claim 4 (rigor) | aggregation only; defends "we don't just memorize the DX field" |
| From-scratch backbone vs pretrained | implicit framing of Claim 1 | one extra Stage-1 run per cohort |

## Cut / Defer to `followup.md`

These currently sit as `[Placeholder]` in `main.tex`. Defends no claim → delete from this submission.

| Cut item | Currently in | Defends any claim? |
|---|---|---|
| Trajectory generation eval (NLL + Wasserstein + clinician) | §A.8 | no — paper of its own |
| Multi-head combinations (Hawkes+ordinal, Cox+ordinal) | §A.6 | no — design-space exploration |
| γ ∈ {0.05, 0.1, 0.2} sweep | §A.6 | no |
| LoRA r ∈ {8, 16, 32} sweep | §A.6 | no |
| Time-token resolution {month / quarter / quantile / byte} | §A.6 | no |
| Vocabulary-extension ablation | §A.6 | no |
| Sub-sequence augmentation schedule | §A.6 | no |
| Static-context placement | §A.6 | no |
| Backbone scale {0.5, 0.8, 2}B | §A.6 | no |
| Stage-1-only vs Stage-1+2 | §A.6 | weak — keep only if free |
| Token-level leave-one-token-out attribution | §A.8 | no |
| Compute / FLOPs / latency table | §A.8 | no |
| Subgroup BCA (APOE-ε4, sex, education) | §A.8 | no — defer |
| Few-shot adaptation N ∈ {16, 64, 256} | §A.7 | no — defer |
| Effective context length curve | §A.7 | no |
| Inter-event time PIT | §A.8 | only if time prediction is reported (currently reserved for follow-up) |
| BEHRT / Med-BERT MLM baseline | §A.5 Tier 2 | no — pick zero or one Tier-2 baseline total |
| OpenFIM zero-shot | §A.5 Tier 2 | no |
| TimesFM forecasting baseline | §A.5 Tier 2 | no |
| IFTPP / AttNHP on diagnosis-only stream | §A.5 Tier 2 | no |
| MEME pseudo-notes + frozen MedBERT | §A.5 Tier 2 | no |

---

## File hygiene

- Delete the cut placeholders from `main.tex` so they stop generating to-do guilt every time you scroll the appendix.
- Move the cut list verbatim into `followup.md` so the work isn't lost.
- Keep this file updated as Key Results move from `pending` → `done`.

---

## Biomedical credibility gates (for the follow-up clinical paper)

This is **not** the NeurIPS 2026 plan — these are the gates a clinical reviewer (npj Digital Medicine, *Alzheimer's & Dementia*, *Lancet Digital Health*, JAMIA) will demand for the biomedical follow-up. Most are explicitly out of scope for the May 31 submission. Listed here so they are not lost and so any item that *also* strengthens NeurIPS is flagged.

The bar here is different: clinical reviewers don't credit method elegance. They credit (a) effect size relative to a clinically-deployed baseline, (b) calibration and net benefit at decision thresholds, (c) generalisation to an external cohort, (d) subgroup performance in heterogeneous ADRD populations, (e) interpretability of model outputs in clinical terms.

| # | Gate | Helps NeurIPS? | Status | What the experiment is |
|---|---|---|---|---|
| B1 | EDUCATE→ADNI (or ADNI→EDUCATE) transfer **effect size**, 5-fold CIs | yes — strengthens Claim 4 | covered as SHOULD ("Cross-cohort transfer") | Pretrain on cohort A, finetune+eval on cohort B. Δ vs cohort-B-only training, with bootstrap CIs. Need ≥3–5 absolute pts on BCA / mAUC, or a real % MAE drop, to register as clinical. |
| B2 | Beat XGBoost on tabular TADPOLE *and* a deployed-style baseline | yes — already in plan (Cerebra Tier-1) | runnable | Existing `train_xgboost_baseline.py` + Cerebra max-pool. Report on D1 + D2 + (ideally) D3 holdout. |
| B3 | Calibration: ECE + reliability diagrams | yes — already in plan (Claim 3 §A.8) | computable from existing preds | ECE per task per encoding for the headline (encoding, head) pair vs pure-text. Reliability plots in supplement. |
| B4 | Survival metrics: c-index + integrated Brier score (IBS) for Cox head | partial — strengthens Claim 3 | computable from existing preds | Currently Cox is framed as representation-shaping. For the clinical paper it has to also be reported as a survival predictor with c-index + IBS, time-dependent AUC. |
| B5 | **Decision-curve analysis / net benefit** at clinical thresholds | no — biomedical only | **not started** | Net-benefit curves over plausible action thresholds for "refer for biomarker testing" / "enrol in trial." Required by *Lancet Digital Health* and increasingly by *npj DM*. |
| B6 | **Subgroup analysis**: APOE-ε4 status, sex, age strata, baseline MMSE | no — explicitly in NeurIPS cut list | aggregation only, sweep already covers it | Per-subgroup BCA/mAUC with overlapping CIs. ADRD reviewers will probe this; foundation-model claims often fall apart here. |
| B7 | **External validation cohort**: ADNI → AIBL or NACC (or EDUCATE → a second EHR site) | no — biomedical only, blocking | **not started** — needs data access | Train on the existing cohorts, evaluate zero-shot (or few-shot) on a held-out cohort. This is the single most load-bearing item for a clinical-paper acceptance. Start the data-access paperwork now since it has multi-month lead time. |
| B8 | **Clinical interpretability of `(μ, α, β)`** | no — biomedical only | **not started** | Take trained intensity heads on held-out MCI patients; show post-event excitation `α − μ` differs by APOE-ε4 / sex / progressor-vs-stable in a clinically intelligible direction. One figure converts a methods paper into a clinical one. |
| B9 | Multi-horizon forecasting reported with horizon-stratified metrics (6 / 12 / 24 / 48 mo) | weak for NeurIPS | partly covered by D2 rollover | Single-model multi-horizon table; clinically what neurology trials care about (time-to-conversion windows). |
| B10 | Pre-registered analysis plan + reporting checklist (TRIPOD-AI / CONSORT-AI) | no | **not started** | Required by most clinical journals now. Cheap; do once at submission. |

### Routing

- **B1, B2, B3** — execute for NeurIPS as already planned. They double as biomedical-paper material.
- **B4, B6, B9** — aggregate from the existing 192-model sweep; defer the *write-up* to the biomedical paper but compute the numbers now while predictions are still on disk.
- **B5, B8, B10** — pure biomedical-paper work; do **not** allocate NeurIPS sprint time. Open as issues for the clinical follow-up.
- **B7** — start the external-cohort data-access process now. It is the long pole. Without an external cohort, the biomedical paper will land in a lower-tier venue regardless of effect size.

### Stop rule (biomedical edition)

Before claiming "the biomedical paper is ready," every cell in B1–B10 must be either **done** or **explicitly waived** with a one-sentence reason in this table. No exceptions — clinical reviewers actually check.
