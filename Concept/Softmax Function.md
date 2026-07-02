# 1. Definition

$$P(y = i \mid \mathbf{z}) = \frac{e^{z_i}}{\sum_{j=1}^{K} e^{z_j}}$$

- **$z_i$**：输入向量中的第 $i$ 个元素的原始得分。
    
- **$e^{z_i}$**：对该得分取以 $e$（自然常数）为底的指数。
    
- **$\sum_{j=1}^{K} e^{z_j}$**：所有输入元素取指数后的**总和**（作为分母进行归一化）。

Take a bunch of raw scores—which can be positive, negative, big, or small—and first use exponents to magnify the gaps and turn them all into positive numbers. Then, calculate **each score's percentage** of the total, turning them into a set of probabilities that add up to exactly 100%.

# 2. Features 

## 2.1 The sum of all weights must be equal to 1

$$\sum a_i = a_1 + a_2 + \dots + a_n = 1$$

## 2.2 The Exponential Amplification Effects 

The Softmax function incorporates the base $e$ (approximately 2.718), which means it "ruthlessly amplifies any advantage." 
Suppose in a single-head attention mechanism, the raw score computed between "apple" and "laptop" is 5, while the raw score with "fast" is 4 (a difference of only 1 point).
Applying $e^x$ into the calculation: $e^5 \approx 148.4$, whereas $e^4 \approx 54.6$.
As a result, the final weights are distributed as follows: "laptop" receives 73%, while "fast" only gets 27%.