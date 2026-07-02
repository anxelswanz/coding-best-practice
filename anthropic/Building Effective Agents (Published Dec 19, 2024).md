
## 1. The difference between Workflow and Agents 

- **Workflows** are systems where LLMs and tools are <u>**orchestrated through predefined code paths**</u>. Workflow offers predictability 
- **Agents**, on the other hand, are systems where LLMs dynamically direct their own processes and tool usage, <u>maintaining control over how they accomplish tasks</u>. 


## 2. Different Types of Workflow

### 2.1 prompt chaining
[[A Survey on Large Language Model-based MultiAgent Systems Paradigms, Applications, andChallenges#1.1.1 Sequential]]
add programmatic checks (see "gate” in the diagram below) on any intermediate steps to ensure that the process is still on track.

### 2.2 Routing 
[[A Survey on Large Language Model-based MultiAgent Systems Paradigms, Applications, andChallenges#1.1.1 Sequential]]
Routing classifies an input and directs it to a specialized followup task.

### 2.3 Parallelization
[[A Survey on Large Language Model-based MultiAgent Systems Paradigms, Applications, andChallenges#1.1.4 Ensemble]]
LLM calls1 + calls2 + calls3 -> aggregator 

### 2.4 Orchestrators - workers 
[[A Survey on Large Language Model-based MultiAgent Systems Paradigms, Applications, andChallenges#1.1.3 Hierarchical]]
Central LLM breaks down tasks and send them to workers and synthesize the outputs 

### 2.5 Evaluator-optimizer loop 
One LLM generates a response while another provides feedback and evaluation in a loop

## 3. Agents 

 ### 3.1 when you should and should not use agent 
 Agentic systems often trade latency and cost for better task performance, and you should consider when this tradeoff makes sense. 
 Agents are better options flexibility and model-driven decision-making are needed at scale. Agents can be used for open-ended problems where it's difficult or impossible to to predict the required number of steps and where you can't hardcode a fixed path.


 