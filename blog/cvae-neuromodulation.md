---
layout: post
title: "Detecting hidden neuromodulation effects through deep generative modeling"
date: 2025-06-14
tags: [neuroscience, deep learning, fMRI, TMS, cVAE]
---


In this post, I share how I used a conditional variational autoencoder (cVAE) to analyze resting-state fMRI data and uncover individual-specific effects of TMS. While this analysis is part of a larger research project, I wrote this post separately to spotlight how advances in deep learning can empower experimental neuroscience in ways that were previously out of reach.


## Problem statement

Understanding how neuromodulation affects the brain is a fundamental challenge in cognitive neuroscience. In this project, I applied a **conditional variational autoencoder (cVAE)** to model and detect stimulation-induced changes in resting-state functional connectivity (FC). The cVAE was trained to capture the structure of FC patterns across different TMS conditions, while accounting for individual variability.

## Why a cVAE?

Variational autoencoders (VAEs) are generative models that learn a latent representation of data. By extending this to a **conditional** setup, the model can learn how the latent structure shifts based on external factors—in this case, the **stimulation condition** (cTBS vs. sham).

This approach is particularly powerful for fMRI analysis because:

- Functional connectivity (FC) patterns can be highly similar within individuals, potentially masking stimulation-related effects.
- Stimulation may induce **individual-specific** effects that are not aligned across participants, making traditional group comparisons less sensitive.


## The data at a glance

To explore how brain stimulation affects the brain at rest, I collected data from **48 participants**. Each person came in for multiple sessions, including:

- **1 baseline session** with no stimulation (just resting)
- **2 sessions with real brain stimulation** (called *cTBS*)
- **4 sessions with sham stimulation** (like a placebo, with no real effect)

This design allows us to compare brain activity **within each person**, detecting subtle changes that wouldn’t show up in group averages. Note that there's an inherent imbalance between sham and real sessions (for the other experiment purpose, not detailed here) and later we will talk about how to avoid bias of it during the VAE training.



## From scans to signals

In each session, we recorded the brain's activity using resting-state fMRI. I then converted this activity into a **functional connectivity (FC) matrix**, which basically maps how different regions of the brain are communicating.

These matrices were used as input to the deep learning model.

This is the code to load the data.

```python
# Encode FC matrix
fc_vector = fc_matrix[np.triu_indices(fc_matrix.shape[0], k=1)]
```

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

Key characteristics:
- Input: FC vector from each ROI to AAL region
- Condition: One-hot vector encoding of subject identity
- Target: Reconstruction of the original FC pattern


## Controlling for Session Imbalance

Due to our experimental design, each subject has data with more **sham** than **cTBS** sessions. To avoid bias, I implemented a **weighted random sampling strategy** during training, assigning higher sampling weights to the underrepresented condition. This ensured balanced representation and prevented the model from overfitting to sham patterns.

## Results: Latent Representation of Stimulation Effects

One way we quantified stimulation effects was by measuring the **Euclidean distance** in latent space between each session and the subject’s unstimulated (null) baseline.







