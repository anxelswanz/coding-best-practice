
## 1. What is Context Rot 

Context rot refers to a phenomenon that the attention and comprehension ability of LLM significantly declines as  input length increases when dealing processing long contexts. 

- For example, a customer support agent has a system prompt saying "always be empathetic", but after 15 turns of conversation, it becomes blunt and robotic. That's because 15 turns of prompts diluted the system prompt.
-  A user asks claude code to refactor the auth.ts file, turn1 read tool 300 lines output, turn2 read tool 2oo more lines, turn3 bash output 500 lines ... turn45 user asks: did you remember the task? claude might forget what was the task 

## 2. How to deal with Context Rot?

Compact