

# 1.TodoTool 

## 1.1 状态升级 

之前的TODO

```
{
  "id": "1",
  "content": "修复登录流程",
  "status": "in_progress",
  "priority": "high"
}
```

之前只知道任务是什么，状态是什么，不知道什么是完成、用什么检查、证据在哪里

现在的todo
```
{
  "id": "1",
  "content": "修复登录流程",
  "status": "in_progress",
  "priority": "high",
  "acceptanceCriteria": [
    "已有 DeepSeek API Key 时显示复用当前 Key 的选项",
    "没有 API Key 时不显示复用选项，直接要求输入新 Key"
  ],
  "verificationCommand": "bun test src/ui/LoginWizard.test.tsx",
  "evidenceRefs": [],
  "verifiedAt": null
}
```

|新规则|为什么需要|
|---|---|
|必须有 `acceptanceCriteria`|先定义“什么叫完成”|
|必须有 `verificationCommand`|先定义“怎么检查完成”|
|不能新建时直接 `completed`|防止模型跳过执行过程|
|必须先经过 `in_progress`|表示这个任务真的被执行过|
|`completed` 时必须有 `evidenceRefs`|完成必须引用证据|
|`evidenceRefs` 必须是真实成功工具结果|防止模型编造证据|
|`verifiedAt` 由系统写入|防止模型自己伪造验证时间|

# 2.Task Graph 

### 2.1 The definition 

```
 Break down complex work with TaskCreate as a dynamic task graph, not a fixed script.
```

`TaskCreate`**Creates task nodes with explicit dependencies and acceptance criteria.**
`TaskUpdate`Advances task status, provides evidence, and modifies dependencies.
`TaskGet` / `TaskList`Queries the current state of tasks.

**Trigger Conditions:** When a request involves more than 3 separable steps, spans multiple files, or is expected to run for a long time, the model should first use `TaskCreate` to build a task graph, and then execute tasks in their dependency order.

## 2.2 Features

#### 1. Upstream Affects Downstream

> **Scenario One**

**你现在逐层完成：A → B → C。D 也完成了。**
**然后你发现 A 的证据失效了（比如 API 文档有更新）— 你把 A 标记为** **invalidated****：**
**TaskUpdate(taskId: A,** **status****:** **"invalidated"****,** **notes****:** **"Provider documentation changed."****)**
**reconcileTaskGraph** **自动传播：**
**A: invalidated   ← 你手动触发**
**B: invalidated   ← 依赖 A，自动级联**
**C: invalidated   ← 依赖 B，自动级联**
**D: completed     ← 无依赖，不受影响**

> Scenario Two

**A: completed**
**B:** **pending    ← A 的依赖满足了，****blocked** **自动解除**
**C:** **blocked**    **←** **B** **还没完成，C 仍被阻塞**

#### 2. Every completed task needs an evidence 

```
{
  "criterionId": "criterion-1",
  "claim": "登录向导在已有 DeepSeek Key 时显示复用选项",
  "source": "bun test src/ui/LoginWizard.test.tsx，退出码为 0",
  "confidence": "high",
  "assumptions": [
    "该测试调用的是生产环境中的 LoginWizard 组件"
  ]
}
```


#### 3.How invalidation is triggered?

```
If evidence later becomes false, mark its task invalidated so dependent results are invalidated locally and can be repaired.
```

模型自己触发，**当模型在后续工作中自己发现某个已完成任务的证据不再成立时，它应当主动调用** **TaskUpdate** **把那个任务标为** **invalidated**


# 3. Long Task 

## 3.1 Evidence Critique 

```
每一步：Task Graph 强制检查 acceptanceCriteria + evidence
整件事：/goal evaluator 检查最终完成条件及对话中的真实结果
收尾前：evidence critique 检查这些真实结果是否足够可靠
```

验证
- insufficient_evidence 含义：缺少外部证据（没有命令输出、没有 render 结果等） **— 连测试都没跑，只有一句"应该是好了"**
- risk_coverage_gap 含义：有检查但遗漏了关键失败路径  **— 测试跑了且过了，但只测了开心路径**
- goal_shift 含义：模型偷偷降低验证标准（和阶段 1 的反作弊呼应）**— 把测试删了、跳过、断言改成** **expect(true).toBe(true)**


