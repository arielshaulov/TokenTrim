# TokenTrim: Inference-Time Token Pruning for Autoregressive Long Video Generation

**TokenTrim** is an inference-time method for stabilizing long-horizon autoregressive text-to-video generation.  
It mitigates temporal drift by identifying unstable latent tokens and **hard-pruning** them from the temporal KV cache before reuse, preventing error propagation across rollout steps.

Project page (code, examples, videos): https://amitedenzon.github.io/

---

## 🔥 Highlights

- **Inference-time only**: no retraining, no fine-tuning, no model edits.
- **Latent-space token stability**: detects drift via latent summaries and prunes the most unstable tokens.
- **Works with autoregressive long-video inference**:
  - Rolling Forcing
  - Self Forcing
- **Minimal overhead**: pruning is lightweight and applied only when drift is detected.

---

## 📌 Method Overview

Autoregressive long video generation commonly reuses previously generated content through a temporal KV cache.  
When corruption appears in earlier chunks, it can be **reused repeatedly**, amplifying errors and causing long-horizon drift.

TokenTrim introduces a simple control mechanism:

1. Compute latent summaries for consecutive generated batches.
2. Estimate per-token drift and define an unstable token set.
3. If drift is abnormal (relative to running statistics), **prune** unstable tokens from the KV cache.
4. Regenerate the current batch conditioned on the pruned cache (bounded to a small number of attempts).

For full details, see the paper and project page: https://amitedenzon.github.io/

---

## 🧩 Supported Models / Pipelines

This repository provides code for TokenTrim integrated into:
- **Wan2.1-1.3B** text-to-video backbone
- Autoregressive long-video inference pipelines:
  - **Rolling Forcing** — official repo: https://github.com/TencentARC/RollingForcing
  - **Self Forcing** — official repo: https://github.com/guandeh17/Self-Forcing

> Note: exact configs, prompts, and qualitative results are curated on the project page:  
> https://amitedenzon.github.io/
---


## 🙏 Acknowledgments

TokenTrim builds on autoregressive long-video generation frameworks, including:
- Rolling Forcing: https://github.com/TencentARC/RollingForcing  
- Self Forcing: https://github.com/guandeh17/Self-Forcing  

More details and examples: https://amitedenzon.github.io/