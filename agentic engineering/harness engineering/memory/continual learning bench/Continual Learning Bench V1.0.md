# Definition
https://github.com/pgasawa/continual-learning-bench
Continual Learning Bench is a benchmark to test whether AI can use its memory to do better tasks compared with it without memories.

The metric is **gain**
```
gain = average score of stateful rollouts - average score of stateless rollouts
```



## Command 

```
clbench run exploitable_poker --schedule calling_station_then_fit_or_fold --system astraea --runs 1 --max-workers 4
```
