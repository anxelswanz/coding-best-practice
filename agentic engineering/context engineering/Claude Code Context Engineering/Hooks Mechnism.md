
# 1 What is it 

Claude Code hook system is an **<u>extensible mechanism</u>** based on lifecycle events, allowing users to inject custom shell commands at specific moments during the agent's execution. 

## 2 Why we need hooks?

[[SOLID Principle#5. 开闭原则 (Open-Closed Principle, OCP)]] for decoupling


## 3 How to configure Hooks 

https://code.claude.com/docs/en/hooks#mcp-tool-hook-fields

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "http",
            "url": "http://localhost:8080/hooks/pre-tool-use",
            "timeout": 30,
            "headers": {
              "Authorization": "Bearer $MY_TOKEN"
            },
            "allowedEnvVars": ["MY_TOKEN"]
          }
        ]
      }
    ]
  }
}

  {
    "hooks": {
      "PreCompact": [...],
      "PostCompact": [
        {
          "hooks": [
            {
              "type": "command",
              "command": "~/.claude/hooks/postcompact.sh"
            }
          ]
        }
      ]
    }
  }
```

## 4 Where does it live 

```
                    ┌──────────────────────┐
                    │   query loop / REPL  │  ← harness 主循环
                    └──────────┬───────────┘
                               │ 在关键时机调用
                    ┌──────────▼───────────┐
                    │   hooks.ts           │  ← 统一执行引擎
                    │  executeHooks()      │
                    │  getMatchingHooks()  │
                    └──────────┬───────────┘
                               │
          ┌────────────────────┼────────────────────┐
          ▼                    ▼                    ▼
   command hook          prompt hook           http hook
  (spawn /bin/sh)     (model invocation)    (HTTP request)
  
  ┌─────────┬──────────────────────┬───────────────────────────┐

  │  type   │       执行方式       │         你要提供          │

  ├─────────┼──────────────────────┼───────────────────────────┤

  │ command │ spawn /bin/sh 起进程 │ command：shell 命令字符串 (exit code) │

  ├─────────┼──────────────────────┼───────────────────────────┤

  │ http    │ 发 HTTP POST 请求    │ url：接收端地址           │

  ├─────────┼──────────────────────┼───────────────────────────┤

  │ prompt  │ 直接调模型           │ prompt：提示词文本        │

  ├─────────┼──────────────────────┼───────────────────────────┤

  │ agent   │ 起一个子 agent 验证  │ prompt：验证描述          │

  └─────────┴──────────────────────┴───────────────────────────┘
```
