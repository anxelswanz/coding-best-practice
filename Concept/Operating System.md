
## 1. atime, mtime, ctime 

In Linux and Unix System, atime/mtime/ctime are essential tags used by file system to record changes in different stages.

|**时间戳**|**全称**|**触发条件**|**简单一句话解释**|
|---|---|---|---|
|**`atime`**|**A**ccess Time (访问时间)|**读取**文件内容（如 `cat`, `more`, `vi` 打开查看）|“你最后一次**看**它是什么时候”|
|**`mtime`**|**M**odification Time (修改时间)|**修改**文件内容（如增加了文字、删除了代码并保存）|“你最后一次**改**它是什么时候”|
|**`ctime`**|**C**hange Time (状态改变时间)|**修改文件属性**（如改权限 `chmod`、改所属用户 `chown`、重命名、或者 **`mtime` 改变时**）|“这个文件的**元数据**最后一次变是什么时候”|

```
**➜**  **plans** cat test.txt 

hello

**➜**  **plans** stat -f "访问时间 (atime): %Sa%n修改时间 (mtime): %Sm%n状态改变 (ctime): %Sc" test.txt

访问时间 (atime): Jun 17 15:05:47 2026

修改时间 (mtime): Jun 17 15:05:31 2026

状态改变 (ctime): Jun 17 15:06:26 2026

**➜**  **plans** touch -a test.txt

**➜**  **plans** stat -f "访问时间 (atime): %Sa%n修改时间 (mtime): %Sm%n状态改变 (ctime): %Sc" test.txt

访问时间 (atime): Jun 17 15:09:21 2026

修改时间 (mtime): Jun 17 15:05:31 2026

状态改变 (ctime): Jun 17 15:06:26 2026

**➜**  **plans** echo "you"> test.txt

**➜**  **plans** stat -f "访问时间 (atime): %Sa%n修改时间 (mtime): %Sm%n状态改变 (ctime): %Sc" test.txt

访问时间 (atime): Jun 17 15:09:21 2026

修改时间 (mtime): Jun 17 15:09:35 2026

状态改变 (ctime): Jun 17 15:09:35 2026
```


## 2. mmap 

### 2.1 Traditional Read 

Step 1: Disk -> Kernel Buffer 
Step 2: Kernel Buffer -> User Buffer 

### 2.2 Mmap lazy loading mechanism

- **Establish the Mapping** 
	-OS points a segment of `virtual memory address space` directly to the file space on disk 
- **Trigger a page default**
	-If the program attempts to access this memory for the first time then it will trigger a page default 
- Lazy Loading
	-OS will transport the data to the physical memory that was just mapped to your program

