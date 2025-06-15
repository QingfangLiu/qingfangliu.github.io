---
layout: post
title: "Detecting hidden neuromodulation effects through deep generative modeling"
date: 2025-06-14
tags: [neuroscience, deep learning, fMRI, TMS, cVAE]
---


In this post, I share how I used a conditional variational autoencoder (cVAE) to analyze resting-state fMRI data and uncover individual-specific effects of TMS. While this analysis is part of a larger research project, I wrote this post separately to spotlight how advances in deep learning can empower experimental neuroscience in ways that were previously out of reach.


## Introduction

Understanding how neuromodulation affects the brain is a fundamental challenge in cognitive neuroscience. In this project, I applied a **conditional variational autoencoder (cVAE)** to model and detect stimulation-induced changes in resting-state functional connectivity (FC). The cVAE was trained to capture the structure of FC patterns across different TMS conditions, while accounting for individual variability.

## Why a cVAE?

Variational autoencoders (VAEs) are generative models that learn latent representation of data. By extending this to a **conditional** setup, the model can learn how latent structure shifts based on external factors—in this case, **stimulation condition** (cTBS vs sham).

This approach is particularly powerful for fMRI because:

- The FC may be highly similar for each individual and therefore may obscure stimulation-related effects.
- There may be individual-level effects not aligned across participants.

## Dataset

Each participant (in total 48) underwent multiple resting-state scans across sessions with either **cTBS** or **sham** stimulation. in total, We have one null session (works as baseline), 2 cTBS sessions, and 4 sham sessions. 


## Preprocessing

From each session, I computed a functional connectivity matrix (Pearson correlations across ROIs), which served as the model input.

Key characteristics:
- Input: FC vector from each ROI to AAL region
- Condition: One-hot vector encoding of subject identity
- Target: Reconstruction of the original FC pattern


## Model Architecture

The cVAE consisted of:

- **Encoder**: Maps FC + condition to latent space
- **Decoder**: Reconstructs FC from latent vector + condition
- **Loss**: Standard ELBO, combining reconstruction loss and KL divergence

The training objective:

$$
\mathcal{L} = \mathbb{E}_{q_\phi(z|x, c)}[\log p_\theta(x|z, c)] - D_{KL}(q_\phi(z|x, c) \| p(z))
$$

Where:
- \( x \) is the observed FC
- \( c \) is the stimulation condition
- \( z \) is the latent variable

## Controlling for Session Imbalance

Due to our experimental design, each subject contributed more **sham** than **cTBS** sessions. To avoid bias, I implemented a **weighted random sampling strategy** during training, assigning higher sampling weights to the underrepresented condition. This ensured balanced representation and prevented the model from overfitting to sham patterns.

## Results: Latent Representation of Stimulation Effects

One way we quantified stimulation effects was by measuring the **Euclidean distance** in latent space between each session and the subject’s unstimulated (null) baseline.







