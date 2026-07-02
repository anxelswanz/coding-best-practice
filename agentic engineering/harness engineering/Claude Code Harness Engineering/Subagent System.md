


# Agent Communication Mechanism


## 总览：通信策略分层

| 通信链路                 | 机制                         | 主体形式                                    |
| -------------------- | -------------------------- | --------------------------------------- |
| Sub-agent → 父 Agent  | tool_result + XML envelope | **结构化信封 + 自然语言内容**                      |
| Teammate → Teammate  | Mailbox + SendMessage      | **自然语言**（协议类用 JSON discriminated union） |
| Coordinator → Worker | SendMessage                | **自然语言**                                |
| Worker → Coordinator | `<task-notification>` XML  | **结构化 XML**                             |
| ACP 跨实例              | AgentClientProtocol SDK    | **全结构化 typed message**                  |
| Fork 子进程             | 共享上下文                      | **隐式（继承对话历史）**                          |

**核心设计原则：Agent 间的"思考沟通"用自然语言，系统级生命周期/结果回传用结构化消息。**

---

## 1. Sub-agent（AgentTool）→ 父 Agent：Tool Result + XML 结构

子 Agent 通过 `mapToolResultToToolResultBlockParam()` 回传结果。内容是结构化文本——既不是纯 JSON，也不是纯自然语言，而是**带标记的结构化摘要**：

```xml
<usage>  
  <total_tokens>N</total_tokens>  
  <tool_uses>N</tool_uses>  
  <duration_ms>N</duration_ms>  
</usage>
```

内容本身是子 Agent 的自然语言输出，但包裹了 `<usage>`、`Scope:`、`Result:`、`Key files:` 等结构化段落。

异步 Agent 完成时，父 Agent 收到 `<task-notification>` XML：

```xml
<task-notification>  
  <task-id>agent-a1b</task-id>  
  <status>completed|failed|killed</status>  
  <summary>Agent "Investigate auth bug" completed</summary>  
  <result>Found null pointer in src/auth/validate.ts:42...</result>  
</task-notification>
```

**形式：结构化信封 + 自然语言内容。**

---

## 2. Agent Teams / Swarm：Mailbox 消息系统（双模）

SendMessage 工具（`packages/builtin-tools/src/tools/SendMessageTool/SendMessageTool.ts`）明确支持两种模式。

### 自然语言（默认）

```typescript
// typeof input.message === 'string'  
{"to": "researcher", "summary": "assign task 1", "message": "start on task #1"}
```

### 结构化协议消息（通过 `StructuredMessage` Schema）

```typescript
const StructuredMessage = z.discriminatedUnion('type', [  
  z.object({  
    type: z.literal('shutdown_request'),  
    reason: z.string().optional(),  
  }),  
  z.object({  
    type: z.literal('shutdown_response'),  
    request_id: z.string(),  
    approve: semanticBoolean(),  
    reason: z.string().optional(),  
  }),  
  z.object({  
    type: z.literal('plan_approval_response'),  
    request_id: z.string(),  
    approve: semanticBoolean(),  
    feedback: z.string().optional(),  
  }),  
])

```

通过 `writeToMailbox()` 投递到目标 Teammate 的 inbox。消息自动投递——Teammate 不需要轮询。

有限制条件：

- 结构化消息不能 broadcast（`to: "*"`）
    
- 结构化消息不能跨 session 发送（`bridge:` / `uds:` 目标只允许纯文本）
    
- `shutdown_response` 必须发送给 `"team-lead"`
    
    **形式：自然语言为主，结构化类型用于协议型交互（关机、计划审批）。**
    

---

## 3. Coordinator → Worker：自然语言指令 + XML 通知

Coordinator 通过 `SendMessage` 发送**纯文本自然语言**指令给 Worker。Worker 完成时回传 `<task-notification>` XML。

Coordinator 被剥夺了所有"动手"工具，只保留编排能力（Agent、SendMessage、TaskStop、subscribe_pr_activity）。其 System Prompt 明确要求 Coordinator **不能懒惰地委派理解**：

> [!"When workers report research findings, you must understand them before directing follow-up work. Never write 'based on your findings' or 'based on the research.' These phrases delegate understanding to the worker instead of doing it yourself. ]

**形式：自然语言发送指令，结构化 XML 回传结果。**

---

## 4. ACP（跨 Claude 实例）：全结构化二进制协议

ACP（Agent Client Protocol，`src/services/acp/agent.ts`）是 Claude Code 之间的跨进程/跨机器通信协议。使用 `@agentclientprotocol/sdk` 定义的类型化消息：

|消息类型|方向|用途|
|---|---|---|
|`InitializeRequest` / `InitializeResponse`|双向|握手和版本协商|
|`AuthenticateRequest` / `AuthenticateResponse`|双向|身份验证|
|`NewSessionRequest` / `NewSessionResponse`|客户端→服务器|创建新会话|
|`PromptRequest` / `PromptResponse`|客户端→服务器|发送提示词、接收终止原因|
|`SessionUpdate`|服务器→客户端|实时流式更新（content_block、usage、tool_use 等）|
|`CancelNotification`|客户端→服务器|取消进行中的会话|
|`LoadSessionRequest` / `LoadSessionResponse`|客户端→服务器|加载已有会话|
|`ForkSessionRequest` / `ForkSessionResponse`|客户端→服务器|Fork 已有会话|
|`CloseSessionRequest` / `CloseSessionResponse`|客户端→服务器|关闭会话|

这些是**纯结构化 typed 消息**，没有自然语言——通过 `AgentSideConnection` 传输。

回传的内容流由 `forwardSessionUpdates()`（`src/services/acp/bridge.ts`）将 SDKMessage 逐个转换为 SessionUpdate：SDKMessage 的 `system`、`user`、`assistant`、`stream_event`、`result`、`progress`、`tool_use_summary` 等类型各对应不同的 SessionUpdate 结构。

**形式：全结构化协议。零自然语言。**
