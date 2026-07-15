# NOTES.md

The final model is a GPT-style Transformer optimized within the 2000-step and 2M parameter limits. Weight tying was enabled to improve parameter efficiency. AdamW with weight decay, linear warmup, cosine learning-rate decay, and gradient clipping were used for stable optimization. The feed-forward network uses the SiLU activation function, and attention projections were made bias-free. Standard GPT initialization (std = 0.02) was retained. The final configuration was chosen based on the best development BPB while remaining fully compatible with the provided evaluation pipeline.
