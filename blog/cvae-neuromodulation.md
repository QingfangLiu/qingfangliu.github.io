---
layout: post
title: "Using cVAE to Identify Effects of Neuromodulation on Individuals"
date: 2025-06-14
tags: [neuroscience, deep learning, fMRI, TMS, cVAE]
---

In this post, I share how I used a conditional variational autoencoder (cVAE) to analyze resting-state fMRI data and uncover individual-specific effects of TMS. Traditional analyses often miss subtle or distributed effects—deep generative models offer a promising alternative.


## Introduction

Understanding how neuromodulation affects the brain is a fundamental challenge in cognitive neuroscience. While traditional resting-state fMRI analyses often rely on group-level statistical comparisons or predefined regions of interest, these approaches may overlook subtle or distributed effects, especially at the individual level.

In this project, I applied a **conditional variational autoencoder (cVAE)** to model and detect stimulation-induced changes in resting-state functional connectivity (FC). The cVAE was trained to capture the structure of whole-brain FC patterns across different TMS conditions, while accounting for individual variability.

## Why a cVAE?

Variational autoencoders (VAEs) are generative models that learn a compressed latent representation of data. By extending this to a **conditional** setup, the model can learn how latent structure shifts based on external factors—in this case, **stimulation condition** (cTBS vs sham).

This approach is particularly powerful for fMRI because:

- The data is noisy and high-dimensional.
- There may be individual-level effects not aligned across participants.
- Traditional statistical methods assume linearity or independence that often doesn’t hold.

## Dataset and Preprocessing

