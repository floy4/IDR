# IDR: Infer-Diagnose-Refine Framework for Test-time Adaptation in Vision-Language-Action Models

## Overview

Vision-language-action (VLA) models predict sequential actions to execute tasks specified by language instructions, conditioned on visual observations and proprioceptive states. However, existing models typically perform static multi-modal fusion across all inputs for action generation, overlooking the varying importance of visual observations during the dynamic execution process.

In this paper, we propose an **Infer-Diagnose-Refine (IDR)** framework, a model-agnostic paradigm that can be seamlessly integrated with any VLA model at test time. IDR first infers actions under factual and counterfactual scenarios of visual observations, then diagnoses the causal effects of visual observations as the estimated dynamic importance, which is finally used to refine the initial action predictions in a training-free manner.

## Method Overview

IDR treats visual importance as a dynamic factor during execution, diagnoses how visual observations causally affect the current prediction, and refines the action accordingly. The framework consists of three stages:

### Key Components

- **Infer**: IDR constructs counterfactual inputs through zero-padding interventions, enabling the frozen VLA model to infer factual and counterfactual outputs under different modality conditions.

- **Diagnose**: The changes of outputs are quantified with a norm-based effect measurement to diagnose the dynamic importance of visual observations.

- **Refine**: The diagnosed effects are integrated with the initial action prediction through gated residual fusion, producing a refined action while preserving low-level control stability.

## Key Observations

Through test-time causal inference across models and environments, we derive three key observations:

### Observation 1: Deployment environments shift inherent causal effect patterns

The same VLA model (X-VLA) shifts its visual effect pattern substantially across different benchmarks or embodiments. X-VLA is proprioception-primary in LIBERO but becomes vision-dominant in SIMPLER. This environment-dependent shift suggests that modality balance is associated with deployment environments.

### Observation 2: Model architectures shape specific causal effect patterns

Within the same benchmark, different VLA models exhibit significantly different causal effect patterns. This variation reflects architectural designs where models relying heavily on a dominant vision-language space maintain a higher visual effect ratio, while architectures that inject proprioceptive states and action tokens earlier show stronger proprioceptive effects.

### Observation 3: Manipulation phases dynamically alter causal effect patterns

Within a single task execution, modality effects change with manipulation phases. Visual effect tends to increase during localization and placement, where external scene information is critical, while proprioceptive effect becomes more influential during motion-transition phases.

## Results

### Simulation Benchmarks

IDR consistently improves all four VLA backbones (π<sub>0.5</sub>, X-VLA, OpenVLA-OFT, VLA-Adapter) across LIBERO, CALVIN, and SIMPLER benchmarks.

### Real-World Experiments

We evaluate IDR on real-world manipulation tasks using a dual-arm ARX5 platform. IDR improves manipulation performance across various real-world tasks, particularly on challenging tasks requiring fine-grained visual guidance and contact-rich manipulation.

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
