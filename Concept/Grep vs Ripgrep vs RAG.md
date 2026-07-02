
# 1. Grep (Global Regular Expression Print)

## 1.1 Definition

It takes the keyword/regular expression you provide, scan the files/text stream line by line, and prints out any matching lines on the spot. 

## 1.2 Strength

### · Boyer Moore Algorithm

```
It takes 2 seconds to search 6 GB files.
```

### · It supports ERE (Extended Regular Expression)

- for metacharacters, without ERE, you need to use backslash \ to escape it
```
grep "createFun\|useFun" code.tsx 
```

- with ERE 
```
grep -E "createFun|useFun" code.tsx
```

### · It supports pipeline 

Pass the output of the first command directly as the input of the next command.

```
ps -ef | grep unix 
```


# 2. ripgrep 

## 2.1 Defintion

It is also a `line-oriented` command tool using keyword or regular expression to print out the matching string on the spot.

## 2.2 Strengths 

### 1. Multi-threading

By defaults, ripgrep autimaticaclly enables multi-threaded parallel processing based on `the number of CPU cores` in your computer.

```
Regarding code files, ripgrep is 10x faster than Grep
```

### 2.Respect .gitignore 

Ripgrep automatically ignores .ignore / .gitignore files 


## 3. Why RAG is DEAD

## 3.1 性能对比 Performance Comparison 

### 1. RAG 

```
Data Preparation 

[1] Original File ---> Chunking to 512/1024 tokens (consider 1. token limits 2. overlap)
[2] chunked tokens - (HTTP) --> Embedding 
[3]  Storage (both text & embeddings anc create index)

Online Retrieval 
[4] user query - (HTTP) -> Embedding 
[5] Embedding TopK 
[6] TopK Reranking 
[7] Output - (HTTP) -> LLM Geneation
```

## 2. ripreg

```
user input -> [[Operating System#2. mmap]] -> regular expression matching -> print
```

*mmap = microseconds 


## 3.2 ripgrep 0 预处理 

## 1. RAG 

如果数据变化，需要重新切块
## 2. ripreg

on-the-fly，哪怕数据每秒都在变也可以


