
The sub agent has its own context window, system prompt and toolset 

  ▎ _"Do not spawn agents unless the user asks._ **_Each spawn starts cold and_** 
  ▎ **_re-derives context you already have — it's the expensive path on this plan._** 
  ▎ _A task with 'multiple angles,' 'thorough,' or several parts is not a request_
  ▎ _to spawn; handle it inline with your own tools."_


When main agent forks a sub agent, it will create a task brief

```
Brief the agent like a smart colleague who just walked into the room — it hasn't seen this conversation, doesn't know what you've tried, doesn't understand why this task matters.

- Explain what you're trying to accomplish and why.  
- Describe what you've already learned or ruled out.  
- Give enough context about the surrounding problem that the agent can make judgment calls rather than just following a narrow instruction.  
- If you need a short response, say so ("report in under 200 words").  
- Lookups: hand over the exact command. Investigations: hand over the question — prescribed steps become dead weight when the premise is wrong.
```
