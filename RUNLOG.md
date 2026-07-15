# RUNLOG.md

## Environment
- Framework: PyTorch (CPU)
- Training Data: train_corpus.txt (provided)
- Evaluation: evaluate.py on dev_eval.txt
- Maximum Training Steps: 2000
- Parameter Budget: <= 2,000,000

---

## Run 0 – Baseline

**Configuration**
- Starter GPT model
- Adam optimizer
- Constant learning rate
- No warmup
- No learning rate schedule
- No gradient clipping
- Untied embeddings

**Result**
- Train Loss: ~1.97
- Dev BPB: 2.6699

**Observation**
The baseline converged but validation BPB indicated room for improvement. Training was stable but the optimizer and architecture were intentionally simple.

---

## Run 1 – Weight Tying

**Hypothesis**

Sharing the input embedding matrix with the output projection reduces redundant parameters and generally improves language modeling performance.

**Changes**
- Enabled `tie_weights = True`.

**Result**
- Slight improvement in validation performance.
- Reduced effective parameter redundancy.

**Conclusion**
Weight tying is a standard optimization used in GPT-2 and similar autoregressive language models and was retained.

---

## Run 2 – Training Improvements

**Hypothesis**

A better optimization strategy should improve convergence within the strict 2000-step budget.

**Changes**
- Replaced Adam with AdamW.
- Weight decay = 0.1
- Betas = (0.9, 0.95)
- Added linear warmup.
- Added cosine learning-rate decay.
- Added gradient clipping (max norm = 1.0).

**Result**
- Training became smoother.
- Lower average training loss.
- Better generalization on the development set.

**Conclusion**
The improved optimizer and scheduler provided faster convergence under the limited training budget and were retained.

---

## Run 3 – Model Improvements

**Hypothesis**

Modern activation functions and cleaner attention layers should improve representational capacity without significantly increasing computational cost.

**Changes**
- Replaced GELU activation with SiLU.
- Removed unnecessary bias terms from attention projection layers.
- Retained Pre-LayerNorm architecture.
- Kept dropout disabled since only 2000 optimization steps are allowed.

**Result**
- Stable training.
- Similar training speed.
- Slight improvement in language modeling performance.

**Conclusion**
These architectural changes were lightweight and beneficial, so they were included in the final model.

---

## Run 4 – Capacity Scaling

**Hypothesis**

The starter model used significantly fewer parameters than the assignment limit. Increasing model capacity while remaining below the 2M parameter cap should improve performance.

**Changes**
- Increased model depth and embedding dimension while ensuring the final parameter count remained below the maximum allowed limit.

**Result**
- Improved model capacity.
- Better utilization of the available parameter budget.
- Final model selected based on the best development BPB.

**Conclusion**
Using more of the permitted parameter budget resulted in a stronger model without violating assignment constraints.

---

# Final Configuration

- GPT-based Transformer
- Weight Tied Embeddings
- AdamW Optimizer
- Linear Warmup
- Cosine Learning Rate Decay
- Gradient Clipping
- SiLU Activation
- Bias-free Attention Projections
- Pre-LayerNorm
- Final model selected based on the lowest development BPB.
