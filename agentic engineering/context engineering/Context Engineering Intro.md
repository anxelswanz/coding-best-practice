
## 1 The Limitation of Prompt Engineering

- Prompt Engineering focuses on the quality of <u>single interaction</u>. It's often static. But in many complex business scenarios (multi-turn queries or in agentic system), the robot needs to **access <u>real-time data**</u> in response to this dynamic environment.
- [[Lost in the Middle Problem]]

## 2 Definition 

Context Engineering is science of curating of deciding what goes into the limited context window from a constantly evolving universe of information.


## 3  Context Window Elements 

Standard Example 
- System Prompt 
- Memory & State 
- Retrieved Information (RAG)
- Tool Outputs 
- Conversation History 
- Current Query


Claude Code Example 
- 1 System Prompt (Role, Tools/Hooks/Actions Guidance, Tone Style, Output Efficiency)
- 2 Memory & State (~/.claude/memory/)
- 3 Context 
	-  System Context (git status /branch/recent commit)
	-  User Context (Claude.md / rules.md [[System Context & User Context]]) 
- 4 Messages 
	- 4a Compact Summary 
	- 4b Attachment Messages [[Messages#Attachment Messages]]
	- 4c conversation history 
		- user/assistant queries 
		- tool_use / tool_result

