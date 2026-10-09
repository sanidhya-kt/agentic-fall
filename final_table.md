# Paper Tables Master Pack

**Purpose:** Copy-paste source for IEEE JBHI / Q1 submission tables.  
**Locked sources:** `Q1_SUBMISSION_PACK.md`, `RESULT_EXPORTS_INDEX.md`, `CRITIC_MULTIROUND_COMPARISON.md`, `results/vision/`, Overleaf `RCDP_Overleaf_2026_09_24/tables/`.  
**Cost:** \(\mathcal{R}=10\times\mathrm{FN}+1\times\mathrm{FP}\) unless β-sweep says otherwise.  
**Bhatti baseline:** frozen `tier1_only` = same `cnn_lstm_attn` checkpoint (not published 36-class ≈98% F1).

### Claim rules

| Do claim | Do NOT claim |
|----------|--------------|
| Ambiguous + SisFall recall↑ / cost↓ under selective escalation | Detector SOTA / beat Bhatti published multiclass |
| KFall as external **stability** (ΔF1 +0.003) | KFall as headline F1 breakthrough |
| Contrastive critic improves **grounding** (appendix) | Contrastive / multi-round as main detection win |
| Label-flip AC is a **negative** ablation | Forward label-flip AC |
| VLM pilot **DISCARD** | Image VLM as a paper contribution |
| Synthetic | **Anything** — no synthetic results exist |

---

## T1 — Bhatti vs ours: SisFall main (n=1000 / fold)

**Overleaf:** `tab_perfold.tex`, `tab_meanstd.tex`

| Fold | n | Tier1 F1 | Tier1 Rec | Tier1 Cost/1k | Stack F1 | Stack Rec | Stack Cost/1k | ΔF1 | Cost ↓ |
|-----:|--:|---------:|----------:|--------------:|---------:|----------:|--------------:|----:|-------:|
| 0 | 1000 | 0.774 | 0.781 | 1042 | **0.892** | **0.986** | **156** | +0.119 | 85% |
| 1 | 1000 | 0.737 | 0.748 | 1223 | **0.851** | **0.911** | **490** | +0.114 | 60% |
| 2 | 1000 | 0.755 | 0.783 | 1066 | **0.874** | **0.998** | **134** | +0.119 | 87% |
| 3 | 1000 | 0.766 | 0.732 | 1248 | **0.930** | **0.995** | **83** | +0.164 | 93% |
| 4 | 1000 | 0.760 | 0.749 | 1175 | **0.887** | **0.993** | **136** | +0.126 | 88% |

### T1b — 5-fold mean ± std (Q1 lock)

| Mode | F1 | Recall | Cost / 1000 |
|------|---:|-------:|------------:|
| `tier1_only` (Bhatti-style) | 0.759 ± 0.014 | 0.759 ± 0.022 | 1151 ± 93 |
| **`gate_knn_llm` (ours)** | **0.887 ± 0.029** | **0.977 ± 0.037** | **200 ± 164** |

- Mean ΔF1 ≈ **+0.128**; mean ΔRecall ≈ **+0.218**; mean cost ↓ ≈ **83%**

---

## T2 — Bhatti vs ours: KFall binary (n=1000 / fold)

**Overleaf:** `tab_kfall_mean.tex`, `tab_kfall_perfold.tex`  
**Frame:** cross-domain stability, not headline F1 gain.

| Fold | Tier1 F1 | Tier1 Rec | Tier1 Cost/1k | Stack F1 | Stack Rec | Stack Cost/1k |
|-----:|---------:|----------:|--------------:|---------:|----------:|--------------:|
| 0 | 0.992 | 0.998 | 17 | **0.993** | **1.000** | **7** |
| 1 | 0.987 | 0.996 | 31 | 0.986 | 0.996 | 32 |
| 2 | 0.987 | 0.994 | 40 | **0.989** | **0.996** | **29** |
| 3 | 0.992 | 0.990 | 53 | **0.995** | **0.996** | **23** |
| 4 | 0.980 | 0.979 | 109 | **0.989** | **0.994** | **38** |

### T2b — 5-fold mean ± std

| Mode | F1 | Recall | Cost / 1000 |
|------|---:|-------:|------------:|
| `tier1_only` | 0.987 ± 0.005 | 0.991 ± 0.007 | 50 ± 35 |
| **`gate_knn_llm`** | **0.990 ± 0.004** | **0.996 ± 0.002** | **26 ± 12** |

| Metric | Mean Δ | Percentile 95% CI | Sign |
|--------|-------:|-------------------|------|
| F1 | +0.003 | [0.000, 0.006] | 4/5 |
| Recall | +0.005 | [0.001, 0.010] | 4/5 |
| Cost/1000 | −24.2 | [−48.4, −5.8] | 4/5 |

**Honesty:** KFall 36-class fold0 (our reimplementation) macro-F1 **0.541**, weighted-F1 **0.583** — not comparable to Bhatti published ≈98% multiclass (`tab_kfall_multiclass.tex`).

---

## T3 — Ambiguous cases: Bhatti vs ours (SisFall, n=300 / fold)

**Lead stress bench.** 100 D18/D19 + 100 fall + 100 ADL.  
**Overleaf:** `tab_ambiguous_sisfall.tex`

| Fold | Tier1 F1 | Stack F1 | ΔF1 | Cost/win (T1→Stack) | Esc. |
|-----:|---------:|---------:|----:|---------------------|-----:|
| 0 | 0.696 | **0.868** | +0.172 | 0.86 → 0.13 | 0.59 |
| 1 | 0.738 | **0.894** | +0.156 | 0.82 → 0.28 | 0.52 |
| 2 | 0.682 | **0.892** | +0.210 | 0.98 → 0.11 | 0.63 |
| 3 | 0.714 | **0.902** | +0.189 | 0.92 → 0.16 | 0.61 |
| 4 | 0.735 | **0.922** | +0.186 | 0.93 → 0.06 | 0.62 |
| **Mean** | **0.713** | **0.896** | **+0.183** | **0.90 → 0.15 (−84%)** | **0.59** |

### T3b — Ambiguous means (extra metrics)

| Mode | F1 | Recall | Acc | Esc | Cost/win |
|------|---:|-------:|----:|----:|---------:|
| `tier1_only` | 0.713 ± 0.024 | 0.768 ± 0.020 | 0.793 ± 0.024 | 0.000 | 0.903 ± 0.065 |
| **`gate_knn_llm`** | **0.896 ± 0.019** | **0.976 ± 0.028** | **0.924 ± 0.016** | **0.594 ± 0.042** | **0.148 ± 0.085** |

---

## T4 — Ambiguous cases: Bhatti vs ours (KFall, n=300 / fold)

| Fold | Tier1 F1 | Tier1 Rec | Tier1 Cost | Stack F1 | Stack Rec | Stack Cost | AmbAcc (stack) |
|-----:|---------:|----------:|-----------:|---------:|----------:|-----------:|---------------:|
| 0 | 0.966 | 0.990 | 16 | **0.990** | **1.000** | **2** | 1.000 |
| 1 | 0.985 | 1.000 | 3 | **1.000** | **1.000** | **0** | 1.000 |
| 2 | 0.947 | 0.990 | 20 | **0.976** | **1.000** | **5** | 0.960 |
| 3 | 0.985 | 1.000 | 3 | 0.966 | 1.000 | 7 | 0.940 |
| 4 | 0.957 | 0.990 | 18 | **0.985** | **0.990** | **12** | 0.980 |
| **Mean** | **0.968** | **0.994** | **12** | **0.983** | **0.998** | **5.2** | **0.976** |

---

## T5 — Cost ladder (all modes, SisFall)

**Overleaf:** `tab_ablation_mean.tex` + critic multiround  
Clinical cost/1k unless noted. Graded cost / grounding from critic protocol (same folds, n=1000).

| Mode | Critic? | F1 | Recall | Cost/1k | Graded cost | Grounding | Esc |
|------|---------|---:|-------:|--------:|------------:|----------:|----:|
| `tier1_only` | No | 0.759 ± 0.014 | 0.759 ± 0.022 | **1151 ± 93** | — | — | 0.000 |
| `gate_only` | No | 0.759 ± 0.014 | 0.759 ± 0.022 | 1151 ± 93 | — | — | ~0.47 |
| `gate_knn` | No | 0.753 ± 0.012 | 0.723 ± 0.032 | 1286 ± 133 | — | — | ~0.47 |
| `gate_llm` | No | 0.702 ± 0.021 | 0.895 ± 0.027 | 740 ± 105 | — | — | ~0.47 |
| **`gate_knn_llm`** | **No** | **0.887 ± 0.029** | **0.977 ± 0.037** | **200 ± 164** | 588 ± 178† | 0.436 ± 0.058† | 0.448† |
| `+ action_critique` | Yes (freeze) | 0.891 ± 0.024 | 0.971 ± 0.034 | 215 ± 155 | 586 ± 176 | 0.448 ± 0.058 | 0.448 |
| `+ contrastive_critique` | Yes (freeze) | 0.878 ± 0.021 | 0.971 ± 0.034 | 228 ± 152 | **324 ± 156** | **0.843 ± 0.053** | 0.448 |
| `+ multi_round_critique` | Yes | 0.899 ± 0.020 | 0.973 ± 0.036 | **906 ± 703** | 1281 ± 704 | 0.854 ± 0.054 | 0.429 |
| `+ actor_critic` (label flip) ❌ | Yes (harmful) | 0.761 | 0.732 | ~1105 | — | — | ~0.48 |

† Graded/grounding for no-critic from critic-protocol paired runs (F1 0.890 in that export; Q1 lock for main detection remains 0.887).

**Readout:** Cost collapses only when **kNN + LLM** combine. Label-flip AC destroys the gain. Multi-round explodes clinical cost.

---

## T6 — Module ablation ladder (why each piece matters)

**Overleaf:** `tab_ablation_mean.tex`  
SisFall 5-fold mean (core five modes).

| Mode | F1 | Recall | Cost/1k | Role |
|------|---:|-------:|--------:|------|
| `tier1_only` | 0.759 | 0.759 | 1151 | Bhatti-style detector alone |
| `gate_only` | 0.759 | 0.759 | 1151 | Routing only — no gain |
| `gate_knn` | 0.753 | 0.723 | 1286 | Memory without LLM — can hurt |
| `gate_llm` | 0.702 | 0.895 | 740 | LLM without memory — FAR risk |
| **`gate_knn_llm`** | **0.887** | **0.977** | **200** | **Full primary stack** |

Fold-0 archive n=4000 (same story, sharper): stack F1 **0.907**, Rec **0.993**, Cost **516**, escalated F1 **0.952**.

---

## T7 — Action critique vs no-critic (SisFall)

**Overleaf:** `tab_grounding.tex` (partial)

| Mode | F1 | Recall | Cost | Graded cost | Grounding |
|------|---:|-------:|-----:|------------:|----------:|
| `gate_knn_llm` (no critic) | 0.890 ± 0.024 | 0.971 ± 0.034 | 215 ± 156 | 588 ± 178 | 0.436 ± 0.058 |
| **`action_critique`** | **0.891 ± 0.024** | 0.971 ± 0.034 | 215 ± 155 | 586 ± 176 | 0.448 ± 0.058 |

**Readout:** Detection essentially unchanged; slight grounding bump. Safe Q1-style critic (labels frozen).

---

## T8 — Contrastive action critique (SisFall)

| Mode | F1 | Recall | Cost | Graded cost | Grounding |
|------|---:|-------:|-----:|------------:|----------:|
| `gate_knn_llm` | 0.890 ± 0.024 | 0.971 ± 0.034 | 215 ± 156 | 588 ± 178 | 0.436 ± 0.058 |
| **`contrastive_action_critique`** | 0.878 ± 0.021 | 0.971 ± 0.034 | 228 ± 152 | **324 ± 156** | **0.843 ± 0.053** |

**Readout:** Best **explanation / graded action** trade-off. Use in **appendix**, not as primary detection claim (F1 slightly lower).

---

## T9 — Multi-round critique (SisFall)

| Mode | F1 | Recall | Cost | Graded cost | Grounding |
|------|---:|-------:|-----:|------------:|----------:|
| `gate_knn_llm` | 0.890 ± 0.024 | 0.971 ± 0.034 | 215 ± 156 | 588 ± 178 | 0.436 ± 0.058 |
| `contrastive_action_critique` | 0.878 ± 0.021 | 0.971 ± 0.034 | 228 ± 152 | 324 ± 156 | 0.843 ± 0.053 |
| **`multi_round_critique`** | **0.899 ± 0.020** | 0.973 ± 0.036 | **906 ± 703** | **1281 ± 704** | **0.854 ± 0.054** |

**Recommendation:** **Drop or appendix-only** — grounding ≈ contrastive but clinical cost blows up.

---

## T10 — With vs without AC (SisFall + KFall cross)

| Dataset | Mode | F1 | Recall | Cost | Grounding |
|---------|------|---:|-------:|-----:|----------:|
| SisFall | no critic | **0.890** | **0.971** | **215** | 0.436 |
| SisFall | action_critique | 0.891 | 0.971 | 215 | 0.448 |
| SisFall | contrastive | 0.878 | 0.971 | 228 | **0.843** |
| SisFall | multi_round | 0.899 | 0.973 | 906 | 0.854 |
| KFall | no critic | 0.990 | 0.996 | **26** | 0.011 |
| KFall | action_critique | 0.990 | 0.996 | 26 | 0.495 |
| KFall | contrastive | 0.990 | 0.996 | 26 | 0.495 |
| KFall | multi_round | 0.990 | 0.996 | 26 | 0.495 |

**KFall note:** Tier-1 near ceiling; critic mainly affects grounding, not F1/cost.

---

## T11 — VLM / URFall image pilot

**Verdict: DISCARD.** Model: Qwen2-VL-2B-Instruct. Test n=63 (train 147).  
Source: `results/vision/PILOT_VERDICT.md`

| Model | Acc | Prec | Recall | Spec | F1 | TP | TN | FP | FN | Cost/1k |
|-------|----:|-----:|-------:|-----:|---:|---:|---:|---:|---:|--------:|
| CNN ResNet18 | 1.000 | 1.000 | 1.000 | 1.000 | **1.000** | 27 | 36 | 0 | 0 | **0.0** |
| Zero-shot VLM | 0.429 | 0.429 | 1.000 | 0.000 | 0.600 | 27 | 0 | 36 | 0 | 571.4 |
| LoRA VLM | 0.603 | 0.520 | 0.963 | 0.333 | 0.675 | 26 | 12 | 24 | 1 | 539.7 |

| Criterion | Value | Pass? |
|-----------|------:|:-----:|
| beat_zeroshot (ΔF1 LoRA−ZS) | +0.075 | Yes |
| beat_cnn | False | No |
| ΔF1 LoRA−CNN | −0.325 | No |
| Pilot decision | **DISCARD** | — |

---

## T12 — LLM backends on ambiguous cases (SisFall)

**Overleaf:** `tab_llm.tex`  
Forced escalation, n=150/fold, 5-fold. Official aggregate.

| Backend | Amb. F1 | Recall | Spec | Cost/1k |
|---------|--------:|-------:|-----:|--------:|
| heuristic | 0.034 ± 0.044 | 0.018 ± 0.023 | 0.992 ± 0.010 | 6416 ± 285 |
| qwen2.5-coder:7b | 0.823 ± 0.029 | 1.000 ± 0.000 | 0.195 ± 0.045 | 280 ± 42 |
| **mistral:latest** | **0.972 ± 0.010** | **1.000 ± 0.000** | **0.891 ± 0.036** | **37 ± 13** |

### T12b — Per-fold ambiguous F1 (raw `llm_compare_fold*.json`)

| Fold | Heuristic | Mistral | Qwen2.5 |
|-----:|----------:|--------:|--------:|
| 0 | 0.057 | 0.963 | 0.841 |
| 1 | 0.114 | 0.980 | 0.822 |
| 2 | 0.061 | 0.954 | 0.814 |
| 3 | 0.097 | 0.975 | 0.807 |
| 4 | 0.082 | 0.975 | 0.879 |
| Mean | 0.082 | 0.969 | 0.832 |

---

## T13 — Latency (SisFall agentic timing)

**Overleaf:** `tab_latency.tex`  
GPU / Ollama timing — not wearable hardware.

| Fold | Tier-1 (ms) | Escalated (s) | Mean stack (s) |
|-----:|------------:|--------------:|---------------:|
| 0 | 8.96 | 12.81 | 5.68 |
| 1 | 8.09 | 10.66 | 3.73 |
| 2 | 8.52 | 22.85 | 10.52 |
| 3 | 8.36 | 11.00 | 5.99 |
| 4 | 8.89 | 10.86 | 5.93 |
| **Mean** | **8.56** | **13.64** | **6.37** |

Escalation rate (main stack): **0.468 ± 0.082**.

---

## T14 — Grounding + graded action cost (focus)

**Overleaf:** `tab_grounding.tex`

| Mode | Clinical cost | Graded action cost | Grounding \(G\) |
|------|--------------:|-------------------:|----------------:|
| no critic | 215 | 588 | 0.436 |
| action_critique | 215 | 586 | 0.448 |
| **contrastive_critique** | 228 | **324** | **0.843** |
| multi_round | 906 | 1281 | 0.854 |

---

## T15 — Backbone zoo (detector-only context)

**Overleaf:** `tab_backbone.tex` · E1 standard protocol Youden F1 means  
**Not** the agentic claim — shows primary backbone is competitive, not SOTA-chasing.

| Backbone | F1 mean | F1 std | Rec mean | Spec mean |
|----------|--------:|-------:|---------:|----------:|
| modern_tcn | 0.742 | 0.015 | 0.745 | 0.867 |
| **cnn_lstm_attn (primary)** | **0.718** | 0.022 | 0.756 | 0.820 |
| tcn | 0.702 | 0.029 | 0.733 | 0.820 |
| cnn1d | 0.701 | 0.031 | 0.751 | 0.799 |
| cnn_lstm | 0.696 | 0.010 | 0.741 | 0.803 |
| lstm | 0.693 | 0.016 | 0.720 | 0.818 |
| transformer | 0.690 | 0.012 | 0.742 | 0.792 |
| tsmixer | 0.671 | 0.022 | 0.671 | 0.834 |
| patchtst | 0.541 | 0.009 | 0.627 | 0.647 |

All fail G1 gate (need F1 ≥ 0.95). Agentic story wraps frozen `cnn_lstm_attn`.

---

## T16 — HaRMamba vs CNN–LSTM–Attn (same agentic stack)

Source: `results/harmamba/HARMAMBA_COMPLETE_COMPARISON.md`

| Mode | CNN-LSTM F1 | HaRMamba F1 | ΔF1 | CNN Rec | HM Rec | CNN Cost | HM Cost |
|------|------------:|------------:|----:|--------:|-------:|---------:|--------:|
| `tier1_only` | 0.759 | 0.738 | −0.021 | 0.759 | 0.759 | 1151 | 1175 |
| `gate_knn` | 0.753 | 0.741 | −0.011 | 0.723 | 0.729 | 1286 | 1277 |
| **`gate_knn_llm`** | **0.887** | **0.872** | **−0.015** | **0.977** | **0.977** | **200** | **213** |

Params: CNN-LSTM-Attn ≈ 565k · HaRMamba ≈ 325k (0.6×).  
**Readout:** Stack works on both; CNN primary remains slightly stronger on SisFall.

---

## T17 — Paired statistics (stack − Tier-1, SisFall main)

**Overleaf:** `tab_stats.tex` · BCa bootstrap

| Metric | Mean Δ | BCa 95% CI | Sign | Wilcoxon p |
|--------|-------:|------------|------|-----------:|
| F1 | +0.132 | [0.119, 0.144] | **5/5** | 0.0625 |
| Recall | +0.213 | [0.182, 0.238] | **5/5** | 0.0625 |
| Cost/1000 | −936 | [−1064, −829] | **5/5** | 0.0625 |

Wilcoxon bottoms at 0.0625 with n=5 when all signs agree; CIs exclude zero.

---

## T18 — Negative ablation: label-flip Actor–Critic

**Overleaf:** `tab_negative_ablation.tex`  
**Do not forward.**

| Fold | Stack F1 | Stack Rec | Stack FN | AC F1 | AC Rec | AC FN | ΔFN |
|-----:|---------:|----------:|---------:|------:|-------:|------:|----:|
| 0 | 0.893 | 0.986 | 6 | 0.768 | 0.755 | 105 | +99 |
| 1 | 0.851 | 0.911 | 39 | 0.744 | 0.714 | 125 | +86 |
| 2 | 0.896 | 0.986 | 6 | 0.757 | 0.725 | 119 | +113 |
| 3 | 0.917 | 0.993 | 3 | 0.766 | 0.721 | 122 | +119 |
| 4 | 0.898 | 0.981 | 8 | 0.769 | 0.744 | 110 | +102 |
| **Mean** | **0.891** | **0.971** | **12.4** | **0.761** | **0.732** | **116.2** | **+103.8** |

---

## T19 — β-cost sensitivity

**Overleaf:** `tab_beta_cost.tex`  
SisFall 5-fold mean cost/1000. Primary paper β=10.

| β | τ* | Mode | Cost/1000 | Δ vs Tier-1 |
|--:|---:|------|----------:|------------:|
| 5 | 0.167 | tier1_only | 628 | — |
| 5 | 0.167 | **gate_knn_llm** | **149** | −479 |
| **10** | **0.091** | tier1_only | 1151 | — |
| **10** | **0.091** | **gate_knn_llm** | **200** | **−951** |
| 20 | 0.048 | tier1_only | 2197 | — |
| 20 | 0.048 | **gate_knn_llm** | **302** | −1895 |

Advantage is monotone in β.

---

## T20 — Master cross-dataset summary

| Dataset | Protocol | Tier1 F1 | Stack / model F1 | Tier1 Cost | Stack Cost | ΔF1 | Note |
|---------|----------|---------:|-----------------:|-----------:|-----------:|----:|------|
| SisFall main | n=1000×5 | 0.759 | **0.887** | 1151 | **200** | +0.128 | Primary |
| SisFall ambiguous | n=300×5 | 0.713 | **0.896** | 0.90/win | **0.15/win** | +0.183 | Lead stress |
| KFall binary | n=1000×5 | 0.987 | **0.990** | 50 | **26** | +0.003 | Stability |
| KFall ambiguous | n=300×5 | 0.968 | **0.983** | 12 | **5.2** | +0.015 | External amb. |
| LLM amb. (Mistral) | n=150×5 | — | **0.972** | — | 37/1k | — | Reasoner |
| URFall LoRA VLM | n=63 | — | 0.675 | — | 540/1k | — | **DISCARD** |
| URFall CNN | n=63 | — | 1.000 | — | 0 | — | Beats VLM |

---

## Gaps (explicit — no invented numbers)

| Topic | Status | Action |
|-------|--------|--------|
| **Synthetic data of our architecture** | **MISSING** | Omit from paper, or run a new experiment later. `results/critique_bench/` empty. |
| **R1 near-fall F1** | **null** | Do not claim near-fall F1; FAR on D18/D19 is available instead. |
| **VLM 5-fold / IMU–vision fusion** | Not run | Pilot only; discarded. |
| Free-living duty cycle / wearable latency | Not established | State honestly in limitations. |

---

## Overleaf table map

| This doc | Overleaf file |
|----------|---------------|
| T1 / T1b | `tables/tab_perfold.tex`, `tab_meanstd.tex` |
| T2 / T2b | `tables/tab_kfall_mean.tex`, `tab_kfall_perfold.tex` |
| T3 | `tables/tab_ambiguous_sisfall.tex` |
| T4 | (insert or generate via `generate_kfall_paper_tables.py`) |
| T5–T6 | `tables/tab_ablation_mean.tex`, `tab_ablation.tex` |
| T7–T10, T14 | `tables/tab_grounding.tex` + critic multiround export |
| T11 | Vision pilot — appendix or omit |
| T12 | `tables/tab_llm.tex`, `tab_kfall_llm.tex` |
| T13 | `tables/tab_latency.tex` |
| T15 | `tables/tab_backbone.tex` |
| T16 | HaRMamba comparison markdown → new `tab_harmamba.tex` if needed |
| T17 | `tables/tab_stats.tex` |
| T18 | `tables/tab_negative_ablation.tex` |
| T19 | `tables/tab_beta_cost.tex` |
| Positioning | `tables/tab_positioning.tex`, `tab_bhatti_gaps.tex` |
| Multiclass honesty | `tables/tab_kfall_multiclass.tex` |

Optional appendix: `tab_conformal_gate.tex`, `tab_knn_k.tex`, `tab_seeds.tex`, `tab_pareto.tex`, `tab_robustness.tex`, `tab_adjudicator.tex`.

---

## Recommended paper placement

1. **Main:** T3 (ambiguous lead) → T1 (SisFall main) → T6 (ablation) → T17 (stats) → T2 (KFall stability)  
2. **Main or early appendix:** T12 (LLM backends), T19 (β-cost), T13 (latency)  
3. **Appendix:** T7–T10, T14 (critics), T18 (negative AC), T15–T16 (backbone / HaRMamba)  
4. **Omit or one-sentence limitations:** T11 VLM discard; synthetic gap  

**Primary system to claim:** `gate_knn_llm` (no critic).  
**Optional appendix critic:** `contrastive_action_critique`.  
**Drop:** multi-round (cost), label-flip AC, VLM track.
