# AD-MMTPP — Paper-vs-brainx gap

For each paper artifact: what already exists in `brainx/`, and what's missing. No dates, no weeks. You decide the order.

## Method (§3)

| Paper artifact | In brainx | Gap |
|---|---|---|
| §3.1 problem formulation, Eqs. 1–5 | — | none (math is in `main.tex`) |
| §3.2 V/M/A example fig | — | none (Fig. 1 typeset) |
| §3.3 model + LoRA + 4-bit | `brainx/src/brainx/model/` configs | none |
| §3.4 two-stage training | `brainx/scripts/adni_20260502/job_pretrain.sh`, `job_finetune.sh` | none |
| §3.5 aux heads (Hawkes, Cox×2, ordinal) | `brainx/src/brainx/.../heads/` (per `model_cards/tpp_v10..v14`) | none |

## Results (§5.2)

| Headline / table | In brainx | Gap |
|---|---|---|
| TADPOLE BCA, 5-fold + D2 | 29 model dirs in `artifacts/neurips_adni_final/`; `scripts/adni_20260502/aggregate_results.py` emits CSV | aggregate → LaTeX table; D2 rollover eval not yet rolled into the CSV |
| EDUCATE binary + 5-way leak-safe | `artifacts/neurips_educate_final/educate_20260420/` (one sweep project) | aggregator script for EDUCATE (the existing one is ADNI-only); leak-safe vs leak-permissive both runs; LaTeX table |
| V/M/A granularity table | `evta_*`, `evtb_*`, `tpp_*` directories cover the 3 families × {base, strict, nodx, fim} × {aug, no-aug} | one row per family with mean ± std across folds; LaTeX |
| Aux-head × encoding ablation | sweep covers pure-text + Hawkes (`fim`) but Cox/ordinal not visible in `neurips_adni_final/` listing — verify what's actually there vs. promised | confirm Cox-acc, Cox-attn, ordinal runs exist; if not, run them; then table |
| DX-hidden ablation | `evta_nodx_*`, `evtb_nodx_*` directories exist | aggregate, contrast with `_base_*` |

## Baselines (§A.5)

| Baseline | In brainx | Gap |
|---|---|---|
| XGBoost (last visit / last-K-mean / slope) | `scripts/train_xgboost_baseline.py` (split + horizon parity with adni_20260409) | run on TADPOLE D1 + D2 + EDUCATE; report numbers |
| Cerebra-style max-pool over visits | — | implement (per-modality embed → max-pool → MLP head); 1 day |
| TPP-LLM on same splits | — | clone github.com/tppllm, fine-tune on TADPOLE + EDUCATE; 2–4 days |
| Language-TPP on same splits | — | clone github.com/qykong/Language-TPP, same; 2–4 days, *probably skip — pick one of the two* |
| From-scratch decoder-only LM | sweep recipe runs without pretrained init via config flag | one Stage-1 + Stage-2 per cohort |

## Generalization / robustness (§A.7)

| Item | In brainx | Gap |
|---|---|---|
| Code-leakage gap on EDUCATE | leak-safe and leak-permissive variants both in dataset configs | aggregate the gap across the two preprocess variants |
| Cross-cohort transfer TADPOLE↔EDUCATE | both cohorts trainable from the same recipe, but no transfer eval script | write `evaluate_cross_cohort.py`; the actual model is the existing checkpoint |
| D2 rollover | TADPOLE has D2 split in `preprocess_tadpole.py` | confirm D2 eval is being run by `evaluate_tadpole.py`; aggregate |

## Diagnostics (§A.8)

| Item | In brainx | Gap |
|---|---|---|
| ECE + reliability | logits saved per eval (verify) | small `compute_ece.py`; reliability plot |
| Anything else (subgroup, attribution, compute, PIT) | — | **cut** (see `followup.md`) |

## Writing

| Section | Status |
|---|---|
| §1 Intro | drafted |
| §2 Related Work | drafted; **add Record2Vec + L2C-TabPFN citations** before submission |
| §3 Method | drafted (math just rewritten) |
| §5 Experiments | placeholders pending tables above |
| §6 Conclusion | empty |
| Abstract | empty |

## Immediate-next decisions you (not me) should make

1. Pick **one** LLM-TPP comparator: TPP-LLM **or** Language-TPP. Not both.
2. Pick **zero or one** Tier-2 baseline. Default: zero.
3. Decide whether the Cox-acc, Cox-attn, and ordinal sweeps actually finished (the `artifacts/neurips_adni_final/` listing only shows `base`/`strict`/`nodx`/`fim` variants — the aux-head sweep may live elsewhere, or may not be done).
4. Decide whether D2 rollover numbers are real — `evaluate_tadpole.py` exists but I have not verified it's being run.

Items 3 and 4 are the only blockers I can see. Everything else is a clear write/run/aggregate task.

---

This file is yours now. Edit it, reorder it, delete what's wrong. I won't rewrite it without you asking.
