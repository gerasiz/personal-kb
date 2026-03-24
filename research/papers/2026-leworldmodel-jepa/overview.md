---
title: "LeWorldModel: Stable End-to-End Joint-Embedding Predictive Architecture from Pixels"
authors: [Lucas Maes, Quentin Le Lidec, Damien Scieur, Yann LeCun, Randall Balestriero]
year: 2026
venue: "arXiv preprint (arXiv:2603.19312)"
url: "https://arxiv.org/abs/2603.19312"
date_analyzed: 2026-03-24
tags: [papers, world-models, jepa, representation-learning, model-predictive-control, self-supervised-learning]
---

# LeWorldModel: Stable End-to-End Joint-Embedding Predictive Architecture from Pixels

## Summary

LeWorldModel (LeWM) is a Joint-Embedding Predictive Architecture (JEPA) that learns latent world models end-to-end from raw pixels using only two loss terms — a next-embedding prediction loss and a SIGReg regularizer that enforces Gaussian-distributed latent embeddings. This dramatically simplifies training compared to prior JEPA world models (e.g. PLDM uses seven loss terms) while providing provable anti-collapse guarantees. With ~15M parameters trainable on a single GPU in hours, LeWM plans up to 48x faster than foundation-model-based approaches and remains competitive on diverse 2D and 3D control tasks.

## Key Ideas

- **Two-term training objective.** The entire loss is L_pred + λ·SIGReg(Z). The prediction loss is a simple MSE between predicted and actual next-step latent embeddings (teacher-forcing). SIGReg prevents representation collapse by encouraging embeddings to match an isotropic Gaussian distribution. No stop-gradients, no EMA target encoder, no multi-term balancing act.
- **SIGReg (Sketched-Isotropic-Gaussian Regularizer).** Projects high-dimensional embeddings onto M random unit-norm directions and applies the Epps–Pulley univariate normality test to each projection. By the Cramér–Wold theorem, matching all 1D marginals is equivalent to matching the full joint Gaussian. This gives a principled, scalable anti-collapse mechanism with a single effective hyperparameter (λ).
- **Radical simplicity over prior JEPAs.** Eliminates exponential moving average target encoders, stop-gradient operations, pre-trained frozen encoders, pixel-space reconstruction, reward signals, and proprioceptive inputs. Reduces tunable loss hyperparameters from six (PLDM) to one (λ), enabling logarithmic-time bisection search instead of polynomial grid search.
- **Compact latent planning.** At inference, uses Model Predictive Control (MPC) with the Cross-Entropy Method (CEM) to optimize action sequences in latent space toward a goal embedding. The compact 192-dim latent space with ~200x fewer tokens than DINO-WM enables 48x faster planning.
- **Emergent physical understanding.** The latent space encodes meaningful physical structure — linear and MLP probes can recover agent location, block location, and block angle. Latent trajectories exhibit temporal path straightening (an emergent property, not explicitly trained). A violation-of-expectation framework shows LeWM reliably flags physically implausible events (teleportation) via prediction surprise.

## Method / Approach

**Architecture:** An encoder (ViT, ~5M params, patch size 14, 12 layers, 192-dim) maps pixel observations o_t to latent embeddings z_t via the [CLS] token + a projection MLP with BatchNorm. A predictor (transformer, ~10M params, 6 layers, 16 heads, 10% dropout) takes z_t and action a_t and autoregressively predicts z_{t+1}. Actions are injected via Adaptive Layer Normalization (AdaLN), initialized to zero for stable training.

**Training:** Fully offline, reward-free. Trained on unannotated trajectories of (observation, action) pairs from arbitrary behavior policies. The two-term loss is optimized jointly end-to-end — gradients flow through everything. No architectural tricks needed.

**Planning:** Given start observation o_1 and goal observation o_g, encode both, then use CEM to find action sequence a_{1:H} minimizing ||ẑ_H - z_g||² in latent space. MPC executes only the first K actions, then replans.

## Evidence / Results (high level)

LeWM outperforms PLDM (the closest end-to-end JEPA baseline) on harder tasks like PushT (+18% success rate) and Reacher, while remaining competitive with DINO-WM (which uses a frozen DINOv2 encoder pre-trained on 124M images). On PushT, LeWM even surpasses DINO-WM using pixels-only, despite DINO-WM having access to proprioceptive information. Planning completes in under one second vs ~47s for DINO-WM. Training is smooth and monotonically convergent, unlike PLDM's noisy multi-term objective. The approach is robust to encoder architecture choice (ViT and ResNet-18 both work) and insensitive to most SIGReg parameters.

One weakness: LeWM underperforms on very simple environments (Two-Room), where the isotropic Gaussian prior may be a poor match for low-intrinsic-dimensionality data.

## Insights & Takeaways

- **The core insight is that collapse prevention in JEPAs doesn't need heuristics.** By replacing EMA + stop-gradient (which lack a well-defined optimization objective) with a statistically principled regularizer (SIGReg), you get provable guarantees and dramatically simpler training. This is a clean conceptual advance.
- **Practical accessibility matters.** Single-GPU training in hours with one hyperparameter to tune makes this reproducible by anyone. This is a direct response to the growing "compute moat" concern in world model research.
- **The Cramér–Wold approach to high-dimensional distribution matching is elegant.** Reducing a high-dimensional Gaussianity check to many univariate normality tests via random projections is a technique worth remembering beyond this specific application.
- **Limitation to note:** Planning horizons remain short due to autoregressive error accumulation. The authors flag hierarchical world modeling as future work. Also, the Gaussian prior assumption may not suit all environments (as shown by the Two-Room result).
- **Connects to LeCun's broader JEPA vision** for self-supervised world models that learn by prediction in latent space rather than pixel-space generation. LeWM is arguably the cleanest realization of this idea for control to date.

## Citation

> Maes, L., Le Lidec, Q., Scieur, D., LeCun, Y., & Balestriero, R. "LeWorldModel: Stable End-to-End Joint-Embedding Predictive Architecture from Pixels." *arXiv preprint arXiv:2603.19312*, 2026. [link](https://arxiv.org/abs/2603.19312)
