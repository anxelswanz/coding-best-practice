context resets: Deprecated

## Context Collapse 

- Claude Code forks a ctx-agent to evaluate the message history and give them risk score.
- ctx-agent determines which spans to be staged
- Once the context window hits 90% threshold, all staged messages are committed. The engine safely collapses the spans from the lowest risk scores to free up token spaces


## Auto Compact

### 1 PreCompact Hook

Append [[#1 BASE_COMPACT_PROMPT]] and [[#2 Additional Instruction]]

### 2 analysis & summary 

 模型被要求在写摘要前，先在  `analysis` 里**按时间顺序把整段对话过一遍**
 
```
  // Chronologically analyze each message and section of the conversation:
  // The <analysis> block is a drafting scratchpad that improves summary
  // quality but has no informational value once the summary is written.
```

有了 analysis 把思路理清，summary的质量更高


### 3 PostCompact Hook

Display compact_message to user 














# Reference 

## **1 BASE_COMPACT_PROMPT**

 要求模型对整个对话生成包含 9 个章节的摘要：

  1. Primary Request and Intent    ← 用户所有请求的详细描述

  2. Key Technical Concepts        ← 重要技术概念、框架

  3. Files and Code Sections       ← 具体文件 + 完整代码片段

  4. Errors and fixes              ← 遇到的错误及修复过程

  5. Problem Solving               ← 已解决的问题

  6. All user messages             ← 所有用户消息（非工具结果）

  7. Pending Tasks                 ← 待办任务

  8. Current Work                  ← 压缩前正在做的事（最重要）

  9. Optional Next Step            ← 下一步（含原文引用）

## 2 Additional Instruction

用户手动 /compact "关注类型错误"    ← 用户直接输入的指令