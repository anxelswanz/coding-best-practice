


## Attachment Messages 

In standard LLM API architectures, LLM only recognizes two roles `user` and `assistant`. However, Claude Code generates vast amount of system level metadata that must be communicated to model in real time. For example 
- File Changes (diff message)
- Plan.md
- MCP delta
- Hooks (event)
full list: [[#Claude Code 内部拥有约 40+ 种不同类型的 Attachment，按用途主要可以划分为以下四类]]
























### Claude Code 内部拥有约 40+ 种不同类型的 Attachment，按用途主要可以划分为以下四类

#### ① 文件类（File-Based）— 动态带入代码资产

|**类型**|**触发时机与作用**|
|---|---|
|`file`|模型读取某文件后，将最新内容注入 Context|
|`compact_file_reference`|触发 Compact（压缩）后，重新注入最近读取过的 5 个核心文件|
|`plan_file_reference`|始终保持当前 `Plan.md` 的内容与模型同步|
|`invoked_skills`|将本会话（Session）中已激活或使用过的 Skill 文件内容带入|

#### ② 状态类（State-Based）— 显式约束模型行为

|**类型**|**作用与注入文案示例**|
|---|---|
|`plan_mode`|明确告知：“你现在处于 Plan Mode（规划模式），只能做架构设计，不能执行代码”|
|`auto_mode`|告知模型：“你现在处于自动模式，拥有自主决策权”|
|`task_reminder`|动态提醒：“你当前还有这些待办任务（Todo）尚未完成”|
|`token_usage`|同步当前的 Token 消耗水位线，便于模型评估上下文长度|

#### ③ 工具/MCP 类（Tool & MCP-Based）— 差量更新运行能力

|**类型**|**作用与机制**|
|---|---|
|`deferred_tools_delta`|差量通知模型新增了哪些延迟加载（Lazy Loading）的工具|
|`mcp_instructions_delta`|当 MCP（Model Context Protocol）服务器连接或断开时，进行差量更新|
|`agent_listing_delta`|动态更新当前可用异步 Agent 的列表变化|

#### ④ 事件类（Event-Based）— 记录系统异步痕迹

|**类型**|**作用**|
|---|---|
|`hook_success` / `hook_error`|记录自动化 Hook（如 Pre-commit、测试脚本）的执行结果|
|`task_status`|跟踪和记录后台异步 Agent 的实时运行状态|
|`date_change`|当运行时间跨天时，更新时间戳底座|

