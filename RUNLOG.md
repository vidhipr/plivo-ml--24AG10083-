# RUNLOG.md

## Environment
- Framework: PyTorch (CPU)
- Dataset: train_corpus.txt
- Evaluation: evaluate.py using dev_eval.txt
- Maximum optimizer steps: 2000
- Maximum parameters: 2,000,000

---

## Run 0 – Baseline

### Configuration
- Starter GPT model
- Adam optimizer
- Constant learning rate
- Standard Transformer architecture

### Result
- Baseline BPB: 2.6699

### Observation
The baseline converged successfully but left room for improvement in optimization and model efficiency.

---

## Run 1 – Weight Tying

### Hypothesis
Sharing the embedding and output projection weights should improve parameter efficiency and generalization.

### Changes
- Enabled tied token embedding and language model head.

### Conclusion
Retained as part of the final model.

---

## Run 2 – Optimizer Improvements

### Hypothesis
Improving the optimizer should help the model converge better within only 2000 training steps.

### Changes
- AdamW optimizer
- Weight decay = 0.1
- Linear warmup
- Cosine learning-rate schedule
- Gradient clipping

### Conclusion
Training became more stable and these changes were retained.

---

## Run 3 – Architecture Improvements

### Changes
- Replaced GELU with SiLU activation.
- Removed bias from attention projection layers.
- Retained Pre-LayerNorm Transformer blocks.
- Standard GPT initialization (std = 0.02).

### Conclusion
These lightweight architectural improvements improved optimization while maintaining compatibility with the provided evaluation pipeline.

---

## Final Model

The submitted checkpoint uses:
- GPT-style Transformer
- Weight tying
- AdamW optimizer
- Warmup + cosine LR schedule
- Gradient clipping
- SiLU activation
- Bias-free attention
- Standard GPT initialization

The final checkpoint was selected based on the best development BPB while satisfying all assignment constraints.
