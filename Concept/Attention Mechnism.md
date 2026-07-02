
# 1. Definition 

The attention mechanism is a resource allocation scheme. It allows the neural networks to dynamically assign different weights to different parts of input data. The parts with larger weights exert a greater influence on the current input.

**Query ($Q$):** The current target that needs attention or processing (e.g., the current word being translated, or the current pixel). It acts as an active seeker, asking: "I am looking for someone who has specific relationship with me, who fits the bill."
**Key ($K$):** The labels/indices of all elements in the input sequence (e.g., each individual word in the source sentence). Relevance is calculated by comparing $Q$ with $K$.  It acts as "passive tag waiting to be matched"
**Value ($V$):** The actual content of all elements in the input sequence. Once the attention weights are determined, a weighted sum of these Values is computed.


# 2. Category 

## 2.1 Self-Attention Mechanism 

### 2.1.1 Definition  
The self-attention mechanism allows each element in an input sentence to interact with all other elements in the sequence to determine their mutual correlations.

### 2.1.2 Self-attention function



$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$

used: [[Softmax Function]]

### 2.1.3 Self-attention Core Steps 

1. Given an input feature matrix $X$, we first project it using three different linear transformations (weight matrices $W_Q, W_K, W_V$) to generate the **Query ($Q$)**, **Key ($K$)**, and **Value ($V$)** matrices
$$Q = XW_Q$$

$$K = XW_K$$

$$V = XW_V$$
2. Compute the Raw Attention Score
$$\text{Raw Score} = Q K^T$$

3. Scaling operation to prevent the dot product is too large.
$$\text{Scaled Score} = \frac{Q K^T}{\sqrt{d_k}}$$
4. Softmax process 
	$$\text{Attention}(Q, K, V) = \text{Softmax}\left(\frac{Q K^T}{\sqrt{d_k}}\right) V$$

## 2.2 Cross-Attention Mechanism

### 2.2.1 Definition

The cross attention mechanism allows one sequence (usually generation side) to dynamically extract features from the other sequence (typically input data) during information processing. 


## 2.3 Multi-Head Attention Mechanism

### 2.3.1 Definition

Multi-head Attention Mechanism allows model to attend to different information from different representation subspaces at different positions. With a single head, averaging inhibits this.

**Mathematical Definition:**

$$\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, \dots, \text{head}_h)W^O$$

$$\text{where } \text{head}_i = \text{Attention}(QW_i^Q, KW_i^K, VW_i^V)$$

"Where the projections are parameter matrices $W_i^Q \in \mathbb{R}^{d_{\text{model}} \times d_k}$, $W_i^K \in \mathbb{R}^{d_{\text{model}} \times d_k}$, $W_i^V \in \mathbb{R}^{d_{\text{model}} \times d_v}$ and $W^O \in \mathbb{R}^{h d_v \times d_{\text{model}}}$."

### 2.3.2 Why Multi-Head Attention Mechanism is needed


**Without Multi-Head Attention**

The 512-dimensional vector of "apple" passes through a set of parameter matrices to directly generate 512-dimensional $Q, K$, and $V$ vectors. This single vector is a tangled mixture of all potential semantics (fruit, electronic products, speed, grammatical positions, etc.).

“The apple laptop is really fast”
When "apple" is used to calculate attention score with other token. 
1. when comparing with "laptop", it wants to absorb technology property
2. when comparing with "fast", it wants to absorb speed/performance property
however, because softmax is exlusive [[Softmax Function#2. Features#2.1 The sum of all weights must be equal to 1]] , If it wants to assign 90% of weight to "laptop", then only 10% left to "fast". Conversely, if it wants to balance both, then it has to assign "45%" to each.

**With Multi-Head Attention**

The 512-dimensional vector of "apple" is multiplied by 8 different sets of matrices, projecting it simultaneously into 8 independent 64-dimensional subspaces.
- **Subspace of Head 1:** Tasked with capturing "noun-noun modification relationships."
	-I**n the 64-dimensional space of Head 1:** "apple" unhesitatingly allocates a massive weight of 0.9 to "laptop." In this specific space, it definitively locks in its identity—I am an electronics brand.
- **Subspace of Head 2:** Tasked with capturing "subject-verb / adjective-state relationships."
	-**In the 64-dimensional space of Head 2:** "apple" equally unhesitatingly assigns a high weight of 0.8 to "fast." In this space, it fully captures its core attribute—I am very fast.

The model horizontally concatenates the 64-dimensional vectors from all 8 heads, piecing them back together into $8 \times 64 = 512$ dimensions.
