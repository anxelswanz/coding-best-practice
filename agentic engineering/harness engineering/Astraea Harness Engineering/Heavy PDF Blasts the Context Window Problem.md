

## How Astraea Handles PDF


1.`Guardrail1`: Storage Size Pre-Check 
`Bun.file().size` to check the file size.  Setting the hard limit to 50 MB.

2.Load pdf file
 using unpdf with pagination, 20 pages max 
 
3.`Guardrail2`: MaxTokens 
FileReader Tool with maximum tokens gate, each model with different threshold 
$$\text{readMaxTokens} = \min(25000, \max(4000, \text{activeContextWindow()} \times 0.06))$$


## How Orchestrator & Worker Works Here in Astraea
[[Building Effective Agents (Published Dec 19, 2024)#2.4 Orchestrators - workers]]
The orchestrator delegates tasks to each subagent, and subagent only reads their own part of pages and output the summary, the orchestrator then synthesizes the summary and reduce it to a final summary. 
There are different reduce mode
- Map-Reduce: to combine all the summaries together at once and create summary
- Sequential Rolling (it follows a 'rolling snowball' process) : Subagent 1 summarizes paragraph 1, then Subagent 2 combines that summary with paragraph 2 to generate an updated summary, which is passed down sequentially.
- Hierarchical: it splits massive text into small segments to make initial summaries, and groups these summaries to create mid-level summaries, and then it repeats this recursively until they merge into one final master summary.
