
# 1. Structure 

## 1.1. Predefined Structure 

### 1.1.1 Sequential 
### 1.1.2 Peer-to-peer (No central node)
### 1.1.3 Hierarchical 

It's a Tree Topology where top-level tree where top-level nodes are responsible for planning and coordinating the parallel work of lower-level specialized agents.

### 1.1.4 Ensemble
### 1.1.5 Environment-mediated 


## 1.2. Dynamic Structure 

- routing 
- dynamic graph 

## 1.3. Automation Structure 

- Search
- Code Generation (Map natural language requirements into executable programs)
- Self-Organizing Dynamic Orchestration

# 2. Interaction 

## 2.1. Communication Paradigms 

How agents convey messages 

### 2.1.1. Natural Language (Human Language)
[[Subagent System#3. Coordinator → Worker：自然语言指令 + XML 通知]]
It is flexible but human natural language is open to interpretation, and is prone to ambiguity.

### 2.1.2. Structured Language (JSON/Schema/XML)
[[Subagent System#结构化协议消息（通过 `StructuredMessage` Schema）]]
Structured language can eliminate the ambiguity
- Collaboration Protocol: Agent and agent communciation
- Operation Interface: Agent and tools interaction

### 2.1.3. Implicit Vector Communication 

- **Agent A 独自思考：** Agent A 在它的 Transformer 模型里输入了大量的设定，它的注意力机制（Attention）在内部生成了复杂的 **KV-Cache（键值缓存）**。这个 KV-Cache 里面包含了这个城市所有的“精髓”——霓虹灯的折射率、下雨天的潮湿感、贫民窟与高楼的对比。这些都是极难用一两句话描述清楚的**高维特征向量**。
    
- **不转成文字，直接对齐通道：** Agent A **不写任何总结报告**。底层工程系统直接把 Agent A 刚刚计算好的 **KV-Cache 矩阵**，通过高速网络直接复制或映射到 Agent B 的注意力机制层中。
    
- **Agent B 瞬间“心领神会”：** Agent B 拿到这些 KV-Cache 后，它的模型在处理下一句话时，可以直接在这个“继承来”的注意力背景下进行计算。Agent B 不需要读 Agent A 写的长篇大论，它的“大脑”里直接就有了那个城市的画面感。
    
- **无缝续写：** Agent B 直接输出：“_细雨打在斑驳的全息广告牌上，折射出紫色的光斑……_”

#### **KVCOMM (FastMAS/KVCOMM)** $\rightarrow$ 顶会 NeurIPS 正式开源

- **它是做什么的：** 专门解决多智能体“套娃式对话”导致 Token 爆炸的问题。它提出了“跨上下文 KV 缓存通信”，让 Agent 之间不传文本，而是像传接力棒一样直接传 KV-Cache。
    
- **惊人战绩：** 在 5 个 Agent 协同写代码/做数学题的场景下，首字延迟（TTFT）从 430ms 暴跌到 55ms，**速度提升 7.8 倍**。
    
- **技术亮点：** **不需要训练模型（Training-free）**，直接在推理阶段动态对齐。

####  **LRAgent (hjeon2k/LRAgent)** $\rightarrow$ 多 LoRA 智能体专属缓存共享

- **它是做什么的：** 很多 Agent 是用不同的 LoRA 微调出来的（比如 Agent A 是程序员 LoRA，Agent B 是测试员 LoRA，但它们共享同一个 Llama 底座）。这个项目实现了让这些**不同 LoRA 的 Agent 之间共享最基础的底层 KV-Cache**，只保留轻量级的 LoRA 差异，极大地省了显存。


## 2.2 Collective Decision Making 

How agents make decisions collectively

### 2.2.1. Statistical Aggregation

- Confidence-based weighting
- Majority Voting

### 2.2.2. Collaborative Refinement 
[[Building Effective Agents (Published Dec 19, 2024)#2.5 Evaluator-optimizer loop]]]
- Reviewer & Generator 
- Fast-Slow Agents 
	slow-thinking agents retrospectively calibrating the reasoning paths of fast-thinking agents

### 2.2.3. Competitive Debate 

Expands Reasoning Boundaries and resolved latent cognitive stances

### 2.2.4 Final Decision 

claude code 实现

```
// SendMessageTool.ts:442（handlePlanApproval）
if (!isTeamLead(appState.teamContext)) {
  throw new Error(
    'Only the team lead can approve plans. Teammates cannot approve their own or other plans.',
  )
}
```

# 3. Evolution

## 3.1 Prompt-level Evolution 

### Textual Back propagation 

- **文中的表述：** `maps execution errors to text-level gradient signals...`
    
- **通俗解释：** 这模仿了深度学习里的“反向传播”算法。当 Agent 协同工作出错时（比如 A 传给 B 的数据格式不对导致报错），系统会把这个“错误报告”当成一个文本信号，反向推导是谁的 Prompt 没写清楚。然后系统会自动给这个 Agent 的协作 Prompt 加上一句：_“注意，下次传数据给 B 的时候，必须使用 JSON 格式”_。这就完成了提示词的定向精准更新。

## 3.2 Restructuring the Role 

- **文中的表述：** `multi-round evaluation and restructuring of role definitions...`
    
- **通俗解释：** 在系统正式上岗干活前（构建阶段），系统会自己进行多轮模拟考试。如果发现某个智能体角色（比如“市场调研员”）在模拟中表现不好，系统就会直接重写（Restructuring）它的角色定义和职责描述，直到模拟跑通为止。

## 3.3 Monte Carlo Search

- **文中的表述：** `Monte Carlo search or self-repair mechanisms to achieve adaptive evolution...`
- **通俗解释：** 当系统发现当前的协作效果不佳时，它不仅会小修小补一两个词，还会使用算法（如蒙特卡洛树搜索）去尝试各种完全不同的 Prompt 组合策略，或者触发“自我修复”机制，重构整个智能体团队的行为守则。

## 3.2 Memory-Level Evolution 

### Feedback Consolidation 

Agent 做完一个任务后，会写一份复盘总结：“这次写代码踩了 A 坑，下次别的项目里只要遇到类似场景，绝对不能再犯。”这种长期记忆能大幅减少以后重复试错（Trial-and-error）的成本。

- **环境反馈（Environment Feedback）的鼻祖：ReAct 框架与 Reflexion 框架**
    
    - **文献：** _Shinn et al., "Reflexion: Language Agents with Internal Monologue and Explicit Memory"_
        
    - **原理：** Agent 丢一段代码给 Python 编译器（环境），编译器报错，Agent 拿到这个报错（Environment Feedback）进行自我修正。
        
- **智能体评测反馈（Critic/Validator Feedback）的鼻祖：ChatEval 与 多智能体辩论机制**
    
    - **文献：** _Chan et al., "Chateval: Towards better llm-based evaluators through multi-agent debate"_
        
    - **原理：** 显式地设立一个“裁判 Agent”（Critic）去审查、打分和纠正其他“工人 Agent”的输出。
        
- **用户反馈（User Feedback）的鼻祖：RLHF 与 人机协同 Agent**
    
    - **文献：** _Ouyang et al., "Training language models to follow instructions with human feedback" (OpenAI RLHF 奠基作)_
        
    - **原理：** 人类（User/Human）在环路中（Human-in-the-loop），通过点赞、点踩或修改意见来引导 Agent 进化。

## Operational Memory 

Agent 查了 100 次数据库后，记住了最快、最省算力的查询命令组合，下次直接套用这个行为模式。


## Parameter-level Evolution 

Gradient Updates 

在多智能体合作赢了一场“游戏”或搞定一个大项目后，很难说清楚到底是谁的功劳最大（这就是功劳分配难题）。Co-PPO 和分层 GRPO 算法就是专门的数学机制，能够精准地给每一个立功的 Agent 对应的模型参数发“奖金”（更新权重），从而在扁平或上下级架构中，同时把所有 Agent 的底层模型给训练得更默契。
