
Traces: Just what logs record what your server did during runtime, traces log what your AI did
Evals: your agents being tested 

The unit test doesn't work here as the same prompt produces different outputs on every single run.


## Three types of Evals 

1. Code Evals - deterministic, free, fast 
2. LLM-as-a-judge - semantic, flexible, free, fast
3. Human evaluations (can't scale)

## Another way to categorize evals 

- capability evals: can it do this new thing?
- regression evals: can it do the stuff it used to do?


## When to use which 
1. Code evals -> format, structure, constraints 
2. LLM judge -> accuracy, relevance, tone, faithfulness
3. Human review -> novel failures, calibrating judges 

## LLM-as-a-judge 

1. Characteristics 

- A second LLM grades outputs against a rubric 
- Handles meaning, not just strings 
- non-deterministic, needs calibration

2. trade-offs 
very expensive and needs prompts LLM

3. Three things are needed
- Judge model, the LLM model that does grading
- prompt template: rubric (criteria)
- data that's been evaluated

4. Evals
- evals for selection
- document relevance
- refusal detection

2. Customize Evals 
- Define the role 
- Define criteria  (criteria comes from error analysis)
- Present the data clearly
- Add an actionable example 

## Practical Tips 

1.Constrain the output 


方案 A：二分类法（Binary Result: 0 或 1）

**示例：** `Actionable` (可执行) vs `Not Actionable` (不可执行)

这是最干净、噪音最低的评估方式。

- **为什么好（Pros）：**
    
    - **彻底消除灰色地带：** 裁判（无论人还是 AI）不需要在“给 6 分还是 7 分”之间纠结。只有“行”或“不行”。
        
    - **业务联动直接：** 在工程上非常容易做自动化决策。例如：如果 Eval 结果是 `1 (Actionable)`，直接放行进下一步管道；如果是 `0`，直接触发拦截或告警。
        
- **局限性（Cons）：**
    
    - **信息丢失严重：** 没办法区分“差一点就完美的模型”和“错得一塌糊涂的模型”。在模型迭代初期，你很难看到微小的进步。
        
- **适用场景：** **生产环境的守门员（Guardrails）**。比如判断回答是否合规、提取的 JSON 是否合法、工具调用的参数是否齐备。


 方案 B：多分类法（More Nuanced: 3-4 个离散标签）

> **示例：** `Correct` (正确) / `Partially Correct` (部分正确) / `Incorrect` (错误)

这是目前业界（如 OpenAI、Anthropic 的 Evals）**最推崇**的折中方案。

- **为什么好（Pros）：**
    
    - **兼顾颗粒度与清晰度：** 它保留了“中间态”（Partially Correct），但由于只有 3 个选项，每个选项的定义可以写得极其黑白分明。
        
    - **消除主观噪音：**
        
        - `Correct`: 关键信息全对。
            
        - `Partially Correct`: 核心逻辑对，但漏掉了解题步骤或有小瑕疵。
            
        - `Incorrect`: 核心结论错了。 裁判不再需要靠直觉去衡量 6 分和 7 分的区别，只需做**条件匹配**。
            
- **适用场景：** **模型研发阶段的基准测试（Benchmarking）**。比如测试模型的数学推理、长文本总结、代码生成能力。


2.Chain-of-thought for judges





## Trace & Span 

Trace: A trace is a completed path composed of interconnected spans. It represents the entire life cycle of a task from start to finish.

Span: A span is the smallest component in a trace. It can be:
(1) A tool execution 
(2) An LLM invoke 
(3) Embedding retrieval 


## Where to get test data?
- Before production: synthetic data (LLM-generated queries)
- After production: real user queries from traces
- Diversity is critical - very phrasing, intent, complexity




What we use humans to do is to build ***Goden Test***
openinference