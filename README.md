# Diffusion Models for SU(2) Lattice Gauge Theory in Two Dimensions

Code repository for the paper:

> **Diffusion Models for SU(2) Lattice Gauge Theory in Two Dimensions**
>
> Herzallah Alharazin, Julia Yu. Panteleeva, Bao-Dong Sun
>
> [https://arxiv.org/abs/2602.09045]

## Overview

This repository provides the code used to train and sample from a score-based diffusion model applied to two-dimensional SU(2) lattice pure gauge theory with the Wilson action, extending previous work on U(1) gauge theories.

The SU(2) group manifold is handled through a quaternion parameterisation. The model is trained on 10,000 configurations generated via Hybrid Monte Carlo (HMC) at a fixed coupling $\beta_0 = 2.0$ on an $8 \times 8$ lattice, augmented to 20,000 samples via random gauge transformations. Through physics-conditioned sampling exploiting the linear $\beta$-dependence of the score function, configurations at different values of the coupling can be generated without retraining.

## Repository Structure

```
├── README.md
├── train_diffusion_model.py   # Main training script
└── Source/
    ├── DiffusionModel.py      # Diffusion model: U-Net, noise schedule, sampling
    └── su2_hmc.py             # SU(2) algebra, lattice gauge field, HMC, observables
```

## File Descriptions

### `train_diffusion_model.py`

Main entry point. Loads HMC-generated SU(2) configurations, sets up the diffusion model, runs the training loop, and saves checkpoints.

### `DiffusionModel.py`

Key components:

* **`DiffusionModel`**: Core class handling the forward noising process and reverse (score-based) sampling on quaternion-valued lattice configurations. Includes physics-conditioned generation at arbitrary $\beta$.
* **`DiffusionUNet`**: U-Net architecture with sinusoidal positional embeddings for time conditioning, operating on 8-channel inputs (4 quaternion components $\times$ 2 link directions).
* **`NoiseSchedule`**: Linear noise schedule for the forward diffusion process.
* **`compute_plaquette_for_generated`** / **`validate_model`**: On-the-fly validation by computing average plaquettes of generated configurations and comparing against exact analytical values.

### `su2_hmc.py`

Key components:

* **`SU2`**: SU(2) matrix operations in quaternion representation: multiplication, inverse, trace, exponentiation, random element generation.
* **`SU2Algebra`**: Lie algebra utilities for the su(2) algebra, used in HMC molecular dynamics.
* **`LatticeGaugeField2D`** / **`WilsonAction2D`**: 2D lattice gauge field storage and Wilson plaquette action computation.
* **`HMC2D`**: Hybrid Monte Carlo sampler for generating training configurations.
* **`exact_plaquette_2d_su2`**: Exact analytical result for the average plaquette via modified Bessel functions.
* **Observables**: Wilson loops, string tension extraction, spatial correlators, correlation length, topological charge, and finite-volume studies.
* **Gauge transformations**: Random gauge transformation generation and application for data augmentation.

## Requirements

* Python ≥ 3.9
* PyTorch ≥ 2.0
* NumPy, SciPy, Matplotlib

Install dependencies:

```
pip install torch numpy scipy matplotlib
```

## Usage

### Train the diffusion model

```
python train_diffusion_model.py
```

Training configurations (lattice size, coupling $\beta$, number of diffusion steps, etc.) can be adjusted inside the script.

## Citation

If you use this code, please cite:

```
@article{Alharazin:2026su2,
    author    = {Alharazin, Herzallah and Panteleeva, Julia Yu. and Sun, Bao-Dong},
    title     = {Diffusion Models for SU(2) Lattice Gauge Theory in Two Dimensions},
    year      = {2026},
    eprint    = {2602.09045},
    archivePrefix = {arXiv},
    primaryClass  = {hep-lat}
}
```
