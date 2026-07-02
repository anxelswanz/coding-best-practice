
## 【问题】 用户 allow/ always allow/ deny 的问题 

### 对于BashTool和PowerShell操作

always allow 会持久化到 .astraea/setting.local.json, 加入到内存configRules, 此次session自动放行，后续session也从文件读取，也会自动放行

## 对于FileWriteTool和FileEditTool

allow all edits in this session 会自动切换到 cruise mode
