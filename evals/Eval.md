## why we use evals 

**把"感觉变好了"变成"可量化、可对比、可追踪"。**
Transform 'feeling better' into something quantifiable, comparable, and trackable."

- ** 客观选型** —— 模型 A vs B、prompt v1 vs v2,用数据决定,不是嘴炮。 [[Action-oriented Prompting]]
-  **改了东西,到底变好还是变坏?** —— LLM/agent 是非确定的黑盒。你改 prompt / 换模型 / 调工具 / 改 rubric
后,**靠肉眼根本说不清**。eval 给你一个分数:改前 40、改后 75,这才叫"真的改好了"。
- **规模化体检** —— 你没法手动看 500 次运行;eval 帮你扫一遍,把你肉眼发现不了的失败暴露出来。


## When to use programmable eval or LLM-As-A-Judge?

Can we objectively judge the 'success' of this task using code?

 这任务的成功能写代码客观判吗？
   ├─ 能（test/数值/文件/格式）─────────▶ 程序化（首选，能用就用）
   └─ 不能（质量/有没有帮助/写得好不好）────────▶ LLM-judge（配紧 rubric + 读 explanation）
  