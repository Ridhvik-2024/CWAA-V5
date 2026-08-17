# CWAA — Complex Wave Associative Architecture

CWAA (Complex Wave Associative Architecture) is a language-model architecture that explores an alternative to conventional attention-based information mixing using **complex-valued wave dynamics and associative state evolution**.

The current experimental model, **CWAA-V5**, combines:

- Local causal complex wave dynamics
- Global complex associative dynamics
- Amplitude and phase representations
- Oscillatory complex decay
- Standard residual connections, normalization, feed-forward processing, and language-model decoding

CWAA-V5 was evaluated against a parameter-matched Transformer baseline on WikiText-103.

## Checkpoint

The trained CWAA-V5 checkpoint is available here:

[Download CWAA-V5 checkpoint]([YOUR_GOOGLE_DRIVE_LINK](https://drive.google.com/file/d/1VQAMTnv5w8pn7_etuQL-9RztbMro-Y3y/view?usp=sharing))

## Current Result

| Model | Parameters | Validation PPL | Test PPL |
|---|---:|---:|---:|
| **CWAA-V5** | 10.396M | **147.39** | **149.15** |
| Transformer | 10.373M | 154.84 | 156.53 |

CWAA-V5 achieved approximately **4.7% lower test perplexity** than the implemented parameter-matched Transformer baseline under the reported experimental setup.

The models were trained with the same dataset, tokenizer, context length, and training budget.

> These results are from a relatively small-scale experiment and should not be interpreted as evidence that CWAA universally outperforms Transformers.

---

# Architecture

At a high level:

```text
                    Token
                      │
                      ▼
               Token Embedding
                      │
                      ▼
              CWAA Processing
                      │
          ┌───────────┴───────────┐
          │                       │
          ▼                       ▼
   Local Wave Dynamics     Global Associative
                           Wave Dynamics
          │                       │
          └───────────┬───────────┘
                      ▼
                Residual Stream
                      │
                      ▼
                 Feed Forward
                      │
                      ▼
                  Next Layer
                      │
                     ...
                      │
                      ▼
                 LM Head
                      │
                      ▼
                 Prediction

```
				 
The architecture is intentionally a coupled system: the local and global mechanisms are designed to operate together rather than as independent replacements for individual Transformer components.

## Main Components

### Local Wave Dynamics

CWAA uses a causal complex-valued wave mixer for local information processing.

The input is transformed into a complex representation and processed using causal complex convolutional dynamics.

### Global Associative Dynamics

The global mechanism maintains an associative state that evolves over the sequence.

Conceptually, its state can be expressed as:

$$
S_t = \gamma_t S_{t-1} + v_t z_{k,t}^{H}
$$

where the complex propagation factor has the form:

$$
\gamma_t = \lambda e^{-i\omega}
$$

This allows the state to evolve through both magnitude and phase.

### Complex Representation

The global mechanism represents information using amplitude and phase:

$$
z = ae^{i\theta}
$$

The architecture therefore has degrees of freedom that are not present in a purely real-valued associative accumulation.

### Standard Components

CWAA-V5 does **not** replace every part of a language model with waves.

It retains conventional components including:

- Learned token embeddings
- Layer normalization
- Residual connections
- Feed-forward layers
- Language-model output projection
- Autoregressive next-token prediction

The wave-based processing is introduced within the information-mixing mechanisms.

---

# Experimental Setup

CWAA-V5 was trained on **WikiText-103** using the GPT-2 tokenizer.

## CWAA-V5 Configuration

| Configuration | Value |
|---|---:|
| Parameters | 10.396M |
| Layers | 12 |
| Embedding dimension | 128 |
| Heads | 4 |
| Context length | 256 |
| Training steps | 5,000 |
| Optimizer | AdamW |
| Initial learning rate | 3e-4 |
| Minimum learning rate | 3e-5 |
| Weight decay | 0.1 |
| Warmup | 500 steps |
| Batch size | 16 |
| Gradient accumulation | 2 |
| Dataset | WikiText-103 |
| Tokenizer | GPT-2 |
| Random seed | 100 |

The Transformer baseline contained approximately **10.373M parameters**.

---

# Results

## Validation

### CWAA-V5

- Loss: **4.9931**
- Perplexity: **147.39**

### Transformer

- Loss: **5.0424**
- Perplexity: **154.84**

## Test

The reported test evaluation used non-overlapping 256-token chunks.

### CWAA-V5

- Loss: **5.0050**
- Perplexity: **149.15**

### Transformer

- Loss: **5.0532**
- Perplexity: **156.53**

CWAA-V5 therefore produced approximately **4.7% lower test perplexity** in this experiment.

---

# Additional Measurements

## Causality

CWAA-V5 passed the causal-information-flow test.

The measured maximum difference before introducing a future token was:

The output changed after the future token was introduced, as expected for a causal model.

##Throughput

The current implementation is not optimized for maximum GPU throughput.

In the benchmark:

CWAA-V5:       ~39,946 tokens/s
Transformer:   ~93,395 tokens/s

The Transformer was therefore substantially faster in the current implementation.

This project does not currently claim a speed advantage.

##Memory

Peak VRAM in the benchmark was approximately:

CWAA-V5:       1.703 GB
Transformer:   1.703 GB
Context Scaling

Preliminary evaluations were performed at:

256
512
1024
2048

These measurements should be treated as preliminary because the model was trained with a 256-token context and the evaluations at longer contexts are therefore out-of-distribution.

##Wave Observatory

CWAA-V5 can also be inspected through a Wave Observatory, which extracts and visualizes internal complex-valued dynamics.

The observatory can examine:

Complex waveforms
Real and imaginary components
Amplitude
Phase
Layer evolution
Spectral characteristics
Global associative dynamics

The project also includes sonification of internal model activity.

The generated audio is not audio produced or understood by the model. It is a mapping of internal mathematical dynamics into an audible signal.

Example files:

V5_FULL_LAYER_EVOLUTION.wav
L12_REAL.wav

These files are included as supplementary exploratory material.

Scientific Interpretation

The current results demonstrate an empirical observation:

CWAA-V5 achieved lower perplexity than the implemented parameter-matched Transformer baseline under the reported experimental conditions.

The experiments do not yet establish a complete causal explanation for why the architecture performs better.

One working hypothesis is that complex phase provides an additional degree of freedom for associative information storage and propagation, while oscillatory decay allows information to evolve through both magnitude and phase.

This remains a hypothesis requiring further mechanistic investigation.

##Limitations

The current experiment has several limitations:

The model is relatively small.
Training was limited to 5,000 steps.
Evaluation was primarily performed on WikiText-103.
The current implementation is slower than the optimized Transformer baseline.
Long-context measurements are preliminary.
The current experiments do not completely isolate the contribution of every architectural component.
The causal mechanism behind the observed performance difference has not been fully established.
Token embeddings remain conventional learned embeddings rather than a fully wave-native token representation.

Therefore, CWAA-V5 should be viewed as an experimental architecture demonstrating a promising result rather than as evidence that wave-based processing is universally superior to attention.
