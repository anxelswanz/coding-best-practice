It is an async generator
# 1. Three Layer Architecture 

 - The REPL.tsx that streams event real-time on terminal
 - The queryLoop(), which is a while loop, that deals with compact/budget/abort/permission and run calltools()
```
┌─ 每轮 while 迭代 ──────────────────────────────────────────┐
│ 1. yield { type:'stream_request_start' }   ← 告诉 UI "要发请求了"│
│ 2. 上下文整形（按需，多数轮 no-op）:                          │
│      applyToolResultBudget  限制工具结果体积                  │
│      snipCompact            裁剪历史                          │
│      microcompact           微压缩                           │
│      contextCollapse        上下文折叠                        │
│      autocompact            自动压缩（超阈值时摘要化历史）       │
│ 3. callModel() 发起流式请求，for await 消费:                   │
│      - yield 每个 StreamEvent / AssistantMessage             │
│      - 收集 toolUseBlocks，needsFollowUp = true              │
│      - StreamingToolExecutor 边流边并发执行工具                │
│ 4. 流结束后:                                                 │
│      if abort        → 补 tool_result，return aborted        │
│      if !needsFollowUp → 没工具调用，本轮收尾(stop hooks)→return (直接结束)│
│      else            → 执行剩余工具 → tool_result            │
│ 5. 把 assistantMessages + toolResults 塞回 messages         │
│      state = { messages:[...], ... }; continue   ← 下一轮     │
└────────────────────────────────────────────────────────────┘
```
 - Message Stream Event, claude.ts [[#2.Server-Side Event]]


## 2. MessageStreamEvent

Source: [[Server-Sent Event]]

## 2.1 The process

```
message_start                       ← 消息骨架（含初始 usage，output_tokens=0）
  content_block_start   (index 0)   ← 第 0 块开始（text / thinking / tool_use…）
    content_block_delta (index 0)   ← 第 0 块的增量（一个或多个）
    content_block_delta (index 0)
    ...
  content_block_stop    (index 0)   ← 第 0 块结束 → 此时产出一条完整 AssistantMessage
  content_block_start   (index 1)   ← 第 1 块（比如一个 tool_use）
    content_block_delta (index 1)   ← input_json_delta 增量拼 JSON
    ...
  content_block_stop    (index 1)
message_delta                       ← 最终 usage + stop_reason
message_stop                        ← 整条消息结束
```

- contentBlocks[index] 累计器
- content_block_start: empty block, marking start
- content_block_delta: core incremental accumulation
- content_block_end: marking end 

A Content Block = An assistant message, so if there's one text and three tool calls, it would be 4 assistant messages.

## 2.2 content_block_delta

```
case 'content_block_delta': {
  const contentBlock = contentBlocks[part.index]   // 找到对应 block
  if (!contentBlock) throw new RangeError('Content block not found')

  switch (delta.type) {
    case 'text_delta':           // 普通回复文本
      contentBlock.text += delta.text
      break
    case 'input_json_delta':     // 工具入参（JSON 片段，字符串拼接！）
      contentBlock.input += delta.partial_json
      break
    case 'thinking_delta':       // 思考过程
      contentBlock.thinking += delta.thinking
      break
    case 'signature_delta':      // 思考块的签名（一次性给全，不是累加）
      contentBlock.signature = delta.signature
      break
    case 'citations_delta':      // 引用（TODO，暂未处理）
      break
  }
}
```
