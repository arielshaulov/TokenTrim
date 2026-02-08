# TokenTrim: Inference-Time Token Pruning for Autoregressive Long Video Generation

<p align="center">
  <a href="https://arielshaulov.github.io/TokenTrim/"><img src="https://img.shields.io/badge/Project-Page-blue" alt="Project Page"></a>
  <a href="https://arxiv.org/abs/2602.00268"><img src="https://img.shields.io/badge/arXiv-2602.00268-b31b1b" alt="arXiv"></a>
</p>

**TokenTrim** is an inference-time method for stabilizing long-horizon autoregressive text-to-video generation. It mitigates temporal drift by identifying unstable latent tokens and **hard-pruning** them from the temporal KV cache before reuse, preventing error propagation across rollout steps.

---

## Qualitative Results

**Prompt:** *A humanoid appears out of smoke, then disperses and reforms.*

<table>
  <tr>
    <th>Rolling Forcing</th>
    <th>+ TokenTrim</th>
  </tr>
  <tr>
    <td><img src="assets/gifs/smoke_rolling.gif" width="320" alt="Rolling Forcing - smoke"></td>
    <td><img src="assets/gifs/smoke_ours.gif" width="320" alt="TokenTrim - smoke"></td>
  </tr>
</table>

**Prompt:** *A young girl skips down a quiet suburban street lined with trees.*

<table>
  <tr>
    <th>Self Forcing</th>
    <th>+ TokenTrim</th>
  </tr>
  <tr>
    <td><img src="assets/gifs/girl_walking_self.gif" width="320" alt="Self Forcing - girl walking"></td>
    <td><img src="assets/gifs/girl_walking_ours.gif" width="320" alt="TokenTrim - girl walking"></td>
  </tr>
</table>

**Prompt:** *Antares in full dragon form towering over a burning battlefield.*

<table>
  <tr>
    <th>+ FlowMo</th>
    <th>+ TokenTrim</th>
  </tr>
  <tr>
    <td><img src="assets/gifs/dragon1_flowmo.gif" width="320" alt="FlowMo - dragon"></td>
    <td><img src="assets/gifs/dragon1_ours.gif" width="320" alt="TokenTrim - dragon"></td>
  </tr>
</table>

---

## Highlights

- **Inference-time only** -- no retraining, no fine-tuning, no model edits.
- **Latent-space token stability** -- detects drift via latent summaries and prunes the most unstable tokens.
- **Compatible with autoregressive long-video pipelines** -- Rolling Forcing and Self Forcing.
- **Minimal overhead** -- pruning is lightweight and applied only when drift is detected.

---

## Method Overview

TokenTrim identifies unstable latent tokens during autoregressive video generation and removes them from the temporal KV cache to prevent error propagation.

<p align="center">
  <img src="assets/fig_smoke_dif.jpg" width="800" alt="TokenTrim method overview">
</p>

**Figure:** TokenTrim overview at autoregressive step *t*.
**(a)** Latent summaries are computed for the previous batch and the candidate batch, and per-token drift is estimated by their difference. Tokens with the largest drift values form the unstable set.
**(b)** If the drift severity exceeds an adaptive threshold, unstable tokens are masked in the temporal KV cache and the batch is regenerated using the pruned cache; otherwise, the batch is accepted and the cache is updated normally.

---

## Release Checklist

- [ ] Release code for **Rolling Forcing** + **TokenTrim** integration
- [ ] Release code for **Self Forcing** + **TokenTrim** integration
- [ ] Release code for **FlowMo comparison** (scripts + configs + prompts)

---

## Supported Models / Pipelines

TokenTrim is integrated into:
- **Wan2.1-1.3B** text-to-video backbone
- Autoregressive long-video inference pipelines:
  - [Rolling Forcing](https://github.com/TencentARC/RollingForcing)
  - [Self Forcing](https://github.com/guandeh17/Self-Forcing)

---

## Acknowledgments

TokenTrim builds on autoregressive long-video generation frameworks, including:
- [Rolling Forcing](https://github.com/TencentARC/RollingForcing)
- [Self Forcing](https://github.com/guandeh17/Self-Forcing)

For more details, examples, and videos, visit the [project page](https://arielshaulov.github.io/TokenTrim/).

---

## Citation

If you find TokenTrim useful in your research, please cite our paper:

```bibtex
@article{shaulov2026tokentrim,
  title={TokenTrim: Inference-Time Token Pruning for Autoregressive Long Video Generation},
  author={Shaulov, Ariel and Shaar, Eitan and Edenzon, Amit and Wolf, Lior},
  journal={arXiv preprint arXiv:2602.00268},
  year={2026}
}
```
