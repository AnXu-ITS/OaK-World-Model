<div align="center">

# Toward Continually Growing World Models: An OaK-Inspired Architecture for Learned Abstraction and Planning

[![Paper](https://img.shields.io/badge/Paper-PDF-red.svg)](paper/Toward_Continually_Growing_World_Models_Preprint.pdf)
[![Venue](https://img.shields.io/badge/NeurIPS%202026-Workshop%20on%20Continual%20World%20Models-darkgreen.svg)](#)
[![Preprint](https://img.shields.io/badge/arXiv-Preprint-b31b1b.svg)](paper/Toward_Continually_Growing_World_Models_Preprint.pdf)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Author](https://img.shields.io/badge/Author-An%20Xu%20(TUM)-blue.svg)](mailto:AnX.RTL2324@tum-asia.edu.sg)

</div>

---

## 📌 Abstract

Learned world models increasingly empower autonomous agents to predict transitions, imagine rollouts, and plan behaviors in complex environments. However, conventional continual world modeling paradigms predominantly formulate lifelong adaptation along a single parametric axis: updating neural network weights ($\theta_t \to \theta_{t+1}$) over a static, fixed-dimensional latent substrate and a fixed single-step temporal granularity. When deployed in non-stationary or open-ended environments, this parameter-only adaptation is vulnerable to loss of plasticity, capacity saturation, and compounding rollout errors over extended horizons.

In this position paper, we argue that genuine continual world modeling requires introducing a second, **structural adaptation axis** ($\mathcal{K}_t \to \mathcal{K}_{t+1}$), wherein the model's predictive state abstractions, temporal abstractions, and option models continually grow, evaluate, and reorganize over time. Grounding this dual-axis formulation in Richard Sutton's **OaK architecture** and its **FC-STOMP** lifecycle (**F**eature Construction $\to$ **S**ub**T**ask $\to$ **O**ption $\to$ **M**odel $\to$ **P**lanning), we formulate **Structural Continual World Modeling (SCWM)**. Under SCWM, discovered state features dynamically expand the world model's predictive state representation, inducing reward-respecting subtasks, closed-loop options, and predictive option models, with retention and pruning governed by downstream planning utility.

---

## 🚀 Key Contributions

1. **Dual-Axis Framework:** We formalize *Structural Continual World Modeling (SCWM)*, augmenting parameter-level adaptation ($\theta_t \to \theta_{t+1}$) with an explicit structural adaptation axis $(\theta_t, \mathcal{K}_t) \to (\theta_{t+1}, \mathcal{K}_{t+1})$.
2. **Constructive Dynamics:** Discovered features $\mathcal{F}_t$ directly expand the world model's predictive state representation ($\tilde{z}_t = \Phi_t(z_t)$), inducing reward-respecting subtasks with terminal stopping bonuses, predictive option models, and multi-scale planning.
3. **Actionable Agenda:** We formulate five falsifiable hypotheses (**H1–H5**) and experimental protocols evaluating planning depth, plasticity retention, and Pareto-optimal utility pruning.

---

## 🏛️ Architecture Overview

The SCWM architecture integrates a **Fast Online Interaction Loop** (handling 1-step primitive dynamics over the extended state) with a **Slow Structural Continual Loop** (executing the OaK FC-STOMP cycle).

```
+---------------------------------------------------------------------------------------------------------+
|                               Fast Online Interaction Loop (1-Step Latent Dynamics)                     |
|                                                                                                         |
|   +---------------+      o_t      +---------------+     \tilde{z}_t    +---------------+     1-step     |
|   |  Environment  | ------------> |    Encoder    | -----------------> | Extended Dyn. | ------------+  |
|   | \mathcal{E}   | <------------ |   E_\phi      |                    |   M_\theta    |             |  |
|   +---------------+  execute a_t  +---------------+                    +---------------+             |  |
|          ^                                                                     |                     v  |
|          |                                            Surprise Trigger         |             +----------+
|          +---------------------------------------------------------------------+-------------| Planner  |
|                                                                                |  \tau-jumps |    P     |
|   +----------------------------------------------------------------------------+             +----------+
|   |                              Slow Structural Continual Loop (OaK FC-STOMP)               ^       |  |
|   v                                                                                          |       |  |
| +-----------------+         +-----------------+         +-----------------+     +----------+ |       |  |
| |  (F) Features   | ------> |  (S) SubTasks   | ------> |   (O) Options   | --> | (M) Opt  |-+       |  |
| | Surprise -> f_i |         | g_i=(R_t, z_i)  |         |   \pi_i, \beta_i|     |    Model |         |  |
| +-----------------+         +-----------------+         +-----------------+     +----------+         |  |
|          ^                                                                                   |       |  |
|          |           +-------------------------------------------------------------------+   |       |  |
|          +---------- |   Utility Evaluation & Structural Lifecycle Management (GrowPrune)| <-+-------+  |
|       Retain / Prune |   U(k) = \lambda_1 U_pred + \lambda_2 U_plan + \lambda_3 U_reuse - \lambda_4 C_comp      |
|                      +-------------------------------------------------------------------+              |
+---------------------------------------------------------------------------------------------------------+
```

---

## 📊 Mapping OaK FC-STOMP to Structural World Models

| OaK Component | World-Model Interpretation | Formal Representation / Mechanism |
| :--- | :--- | :--- |
| **Base Substrate** | Parametric latent dynamics baseline | $z_t = E_\phi(o_{\le t}), \;\; p_\theta(\tilde{z}_{t+1}, r_{t+1} \mid \tilde{z}_t, a_t)$ |
| **Feature Construction ($F$)** | Predictive state representation expansion | $\mathcal{F}_{t+1} = \mathcal{F}_t \cup \{f_{\text{new}}\}, \;\; \tilde{z}_t = [z_t^\top, f_1(z_t), \dots]^\top$ |
| **SubTask ($S$)** | Reward-respecting local control objective | $\mathcal{G}_{t+1} = \mathcal{G}_t \cup \{g_i\}, \;\; g_i = (C_t = R_t, z_i)$ |
| **Option ($O$)** | Closed-loop temporally extended behavior | $\mathcal{O}_{t+1} = \mathcal{O}_t \cup \{o_i\}, \;\; o_i = (\mathcal{I}_i, \pi_i, \beta_i)$ |
| **Option Model ($M$)** | Temporal jump-ahead predictive dynamics | $\mathcal{M}_{o_i}: (\tilde{z}_t, o_i) \mapsto (\hat{\tilde{z}}_{t+\tau}, \hat{R}_{t:t+\tau}, \hat{\tau}) \sim p_{\psi_i}$ |
| **Planning ($P$)** | Multi-scale forward tree/trajectory search | $\mathcal{A}^+_t = \mathcal{A} \cup \mathcal{O}_t, \;\; \text{Search over hybrid rollouts}$ |
| **Utility Feedback** | Knowledge retention, revision, and pruning | $U(k) = \lambda_1 U_{\text{pred}} + \lambda_2 U_{\text{plan}} + \lambda_3 U_{\text{reuse}} - \lambda_4 C_{\text{comp}}$ |

---

## 🔬 Research Hypotheses & Experimental Agenda

* **H1 (Planning Horizon):** Planning over hybrid $\mathcal{A}^+_t = \mathcal{A} \cup \mathcal{O}_t$ achieves deeper horizons and higher returns than 1-step planning ($p_\theta$) under fixed compute budget $B_{\text{plan}}$.
* **H2 (Adaptation Speed):** Constructing option models accelerates policy adaptation under non-stationary task shifts $\mathcal{D}_1 \to \dots \to \mathcal{D}_N$ relative to fixed-architecture tuning.
* **H3 (Growth Frontier):** Multi-objective utility pruning yields a superior Pareto frontier over unconstrained growth ($|\mathcal{K}_t| \to \infty$) or prediction-only pruning.
* **H4 (Plasticity Preservation):** Structurally allocating feature-option units better preserves representation plasticity (effective rank, dormant units) relative to fixed-capacity continual baselines under sustained non-stationarity.
* **H5 (Planning Utility):** Abstractions retained via planning utility ($U_{\text{plan}}$) outperform abstractions selected purely via reconstruction ($U_{\text{pred}}$).

### Planned Evaluation Protocols
* **Exp A (Horizon vs. Compute):** Benchmark 1-step MPC ($p_\theta$) vs. Option-MPC ($\mathcal{M}_\theta \cup \mathcal{M}_{\mathcal{O}}$) under strict simulation budgets in Crafter, Meta-World, and Continual DMC.
* **Exp B (Sequential Shift):** Evaluate forward and backward transfer across sequential task sequences against *Continual-Dreamer* (Kessler et al., 2023), *Online-WM* (Liu et al., 2025), and *DRAGO* (Fu et al., 2025).
* **Exp C (Utility Ablation):** Systematic comparison among unconstrained growth, random pruning, prediction-only pruning ($U = U_{\text{pred}}$), and multi-objective pruning ($U = U_{\text{pred}} + U_{\text{plan}} - C_{\text{comp}}$).

---

## 📂 Repository Structure

```
.
├── arxiv/                          # arXiv submission source & build artifacts
│   ├── main.tex                    # Preprint LaTeX source
│   ├── main.bbl                    # Pre-compiled bibliography
│   ├── neurips_2026.sty            # Style package
│   └── references.bib              # Complete BibTeX database (32 entries)
├── paper/                          # Compiled paper PDF & distribution archives
│   ├── Toward_Continually_Growing_World_Models_Preprint.pdf
│   └── arxiv_source.tar.gz
├── CITATION.cff                    # Citation metadata
├── LICENSE                         # Open-source MIT License
└── README.md                       # Project landing page
```

---

## 📖 Citation

If you find this position paper or conceptual architecture relevant to your research, please cite:

```bibtex
@article{xu2026continually,
  title={Toward Continually Growing World Models: An OaK-Inspired Architecture for Learned Abstraction and Planning},
  author={Xu, An},
  journal={arXiv preprint},
  year={2026},
  note={Workshop on Continual World Models, NeurIPS 2026}
}
```

---

## 📬 Contact & Inquiries

* **Author**: An Xu (Technical University of Munich)
* **Email**: `AnX.RTL2324@tum-asia.edu.sg`
* **Affiliation**: Technical University of Munich, Harbin, China