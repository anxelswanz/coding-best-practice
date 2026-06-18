When engineering prompts for AI agents, rules should be anchored to concrete **<u>triggers and decision points</u>** rather than <u>abstract concepts</u>. A prime example comes from Claude Code's memory verification workflow. When instructing the agent to double-check if remembered filenames or functions still exist, using the abstract heading _'Trusting what you recall'_ proved ineffective. However, changing the title to the action-oriented _'Before recommending from memory'_ dramatically improved performance. Because of the Transformer's attention mechanism, this heading in the agent's decision point **highly activates the model's attention weights**, forcing it to execute the verification checklist right when it matters most.


---

Gemini Explanation
## 1. Context Alignment

大语言模型（LLM）的工作原理是基于**下一个词的概率预测**。标题的作用不仅仅是“起名字”，它是在为后续的内容确立**条件概率的走向**。

- **“Trusting what you recall”（抽象名词）：** 这是一个**状态描述**或哲学讨论。模型在理解这个标题时，激活的知识网络通常是关于“记忆的可靠性 Memory Reliability”、“心理学现象 Psycology”或“概念定义/Concept”。它没有强烈的时序感。
    
- **“Before recommending...”（行动/时间副词）：** 这是一个**决策点（Decision Point）**。在自然语言的语料库中，“Before Doing X, you should...” 后面高概率跟随的是**具体的 Checklists（检查清单）和防错机制**。这个标题直接把模型拉入了“我准备要说话了，我得先检查一下”的**工作流场景**中。
    

## 2. 缩小了“关注焦点” (Attention Narrowing)

注意力机制（Attention Mechanism）是 Transformer 的核心。不同的标题会引导模型将注意力权重分配到不同的地方。

- 当标题是 **“Trusting...”** 时，正文中的 `check the file exists` 或 `grep for it` 被降维成了“用来解释为什么不要盲目信任回忆”的**举例说明**。模型认为核心任务是“理解这个道理”。
    
- 当标题是 **“Before recommending...”** 时，正文的每一点都变成了**立刻执行的指令（Actionable Items）**。模型在生成推荐代码或方案的那个瞬间（即“Before recommending”的临界点），这个标题的注意力权重会被高度激活，因为它与模型当下的行动完全匹配。

---

