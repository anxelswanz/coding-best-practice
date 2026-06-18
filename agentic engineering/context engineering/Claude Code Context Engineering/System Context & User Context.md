
## 1 System Context 

git branch 
git status 
git user 
git recent commits (5 max, 2000 chars max)

# 2 User Context 

## 1.1 CLAUDE.md 

## 1.2 rules/\*.md

例如，项目可能包含 API 层规范、数据库规范和前端组件规范。如果将它们一股脑全部写进单一个 `CLAUDE.md` 文件中，那么无论当前进行什么操作，这些规则都会被全部塞进 Context（上下文）中。这不仅白白浪费 Token，还会给模型带来不必要的噪音干扰。

而 **Rules 机制** 允许你将规范拆分成多个独立的 `.md` 文件，并存放在 `.claude/rules/` 目录下。通过在文件顶部的 Frontmatter 中使用 `paths:` 字段指定 Glob 匹配规则——**只有当 Claude 实际操作到匹配路径的文件时，对应的规则才会被注入到 Context 中**。

## 1.3 Current Date 

Today's date is 2026-06-17.