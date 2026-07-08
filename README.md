# IDR: Infer-Diagnose-Refine Framework for Test-time Adaptation in Vision-Language-Action Models

[![arXiv](https://img.shields.io/badge/arXiv-coming%20soon-orange)](https://arxiv.org)
[![GitHub Stars](https://img.shields.io/github/stars/floy4/IDR?style=social)](https://github.com/floy4/IDR)

## Overview

Vision-language-action (VLA) models predict sequential actions to execute tasks specified by language instructions, conditioned on visual observations and proprioceptive states. However, existing models typically perform static multi-modal fusion across all inputs for action generation, overlooking the varying importance of visual observations during the dynamic execution process.

In this paper, we propose an **Infer-Diagnose-Refine (IDR)** framework, a model-agnostic paradigm that can be seamlessly integrated with any VLA model at test time. IDR first infers actions under factual and counterfactual scenarios of visual observations, then diagnoses the causal effects of visual observations as the estimated dynamic importance, which is finally used to refine the initial action predictions in a training-free manner.

## Method Overview

IDR treats visual importance as a dynamic factor during execution, diagnoses how visual observations causally affect the current prediction, and refines the action accordingly.

![IDR Framework](https://raw.githubusercontent.com/floy4/IDR/master/images/method2.png)

*IDR Framework Architecture: Zero-padding interventions for counterfactual inference, norm-based effect quantification, and gated residual fusion.*

### Key Components

- **Infer**: IDR constructs counterfactual inputs through zero-padding interventions, enabling the frozen VLA model to infer factual and counterfactual outputs under different modality conditions.

- **Diagnose**: The changes of outputs are quantified with a norm-based effect measurement to diagnose the dynamic importance of visual observations.

- **Refine**: The diagnosed effects are integrated with the initial action prediction through gated residual fusion, producing a refined action while preserving low-level control stability.

## Causal Analysis

![Causal Graph](https://raw.githubusercontent.com/floy4/IDR/master/images/causal_graph.png)

*Causal graph illustrating the modality intervention framework.*

IDR treats visual importance as a dynamic factor during execution, diagnoses how visual observations causally affect the current prediction, and refines the action accordingly.

## Key Observations

Through test-time causal inference across models and environments, we derive three key observations.

### Observation 1: Deployment environments shift inherent causal effect patterns

The same VLA model (X-VLA) shifts its visual effect pattern substantially across different benchmarks or embodiments. X-VLA is proprioception-primary in LIBERO but becomes vision-dominant in SIMPLER. This environment-dependent shift suggests that modality balance is associated with deployment environments.

### Observation 2: Model architectures shape specific causal effect patterns

![Model Heatmap](https://raw.githubusercontent.com/floy4/IDR/master/images/fig_baseline_model_suite_heatmap.png)

*Visual effect ratio (R<sub>img</sub>) heatmap across models and LIBERO suites. Each model maintains a consistent visual effect pattern.*

Within the same benchmark, different VLA models exhibit significantly different causal effect patterns. This variation reflects architectural designs where models relying heavily on a dominant vision-language space maintain a higher visual effect ratio, while architectures that inject proprioceptive states and action tokens earlier show stronger proprioceptive effects.

### Observation 3: Manipulation phases dynamically alter causal effect patterns

![Phase Analysis](https://raw.githubusercontent.com/floy4/IDR/master/images/ablation_phase.png)

*Phase-aligned visual effect ratio. The plot reports the visual effect ratio R<sub>img</sub> over task progress for the baseline, Mode E, and Mode F. Dashed vertical lines indicate gripper close and open events.*

Within a single task execution, modality effects change with manipulation phases. Visual effect tends to increase during localization and placement, where external scene information is critical, while proprioceptive effect becomes more influential during motion-transition phases.

## Results

### Simulation Benchmarks

IDR consistently improves all four VLA backbones (π<sub>0.5</sub>, X-VLA, OpenVLA-OFT, VLA-Adapter) across LIBERO, CALVIN, and SIMPLER benchmarks.

### Real-World Experiments

![Real World Results](https://raw.githubusercontent.com/floy4/IDR/master/images/fig_realworld_panels.png)

*Real-world efficiency-success trade-off. Success rate and average completion time are reported for each task. Arrows (Baseline → IDR) pointing upward/leftward indicate simultaneous improvements in both.*

We evaluate IDR on real-world manipulation tasks using a dual-arm ARX5 platform. IDR improves manipulation performance across various real-world tasks, particularly on challenging tasks requiring fine-grained visual guidance and contact-rich manipulation.

### Ablation Studies

![Hyperparameter Ablation](https://raw.githubusercontent.com/floy4/IDR/master/images/ablation_on_hypers.png)

*Moderate correction scales and intervention thresholds yield the best performance, while excessive correction or uniform intervention degrades action generation.*

## BibTeX

```bibtex
@article{idr2026,
  title={Causality-driven Infer-Diagnose-Refine Framework for Test-time Adaptation in Vision-Language-Action Models},
  author={Anonymous Authors},
  journal={Manuscript},
  year={2026}
}
```

## Acknowledgements

This project builds upon several excellent prior works in vision-language-action models and causal inference. We thank the authors of related work for their contributions to the community.
