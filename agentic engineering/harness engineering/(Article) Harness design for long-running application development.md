@Anthropic Published Mar 24, 2026
https://www.anthropic.com/engineering/harness-design-long-running-apps

[[context anxiety]] It means models start to wrap up prematurely as they approach what they believe is the context limit.
[[self-evaluation bias]] When asked to evaluate the work they've produced, agents tend to confidently praise their own work, even to a human observer, it's obviously mediocre. 


# Planner, Generator and Evaluator Work Mode



- Planner only focuses on defining deliverables (full product spec), and decomposes this spec into sprints.
- Generator uses sprint methodology to deliver work
- Evaluator sets evaluation standards to evaluate work from generator 

[[sprint contract]]
Before each sprint, the generator and evaluator will negotiate a sprint contract, that is to agree what done looked like before any code was written. The generator proposes what it would build and how success  will look like and evaluator review that proposal to make sure the generator is building the right thing. 
