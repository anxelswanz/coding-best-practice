
Data masking refers to obfuscating, hiding or replacing sensitive information to prevent data leaking. 

- **遮蔽/打码（Masking）：** 把密码变成星号。例如：`admin123` $\rightarrow$ `******`
    
- **哈希/加密（Hashing）：** 变成一串乱码。例如：`my_key` $\rightarrow$ `a1b2c3d4...`
    
- **截断（Truncation）：** 只保留前几位。例如：`secret_token_999` $\rightarrow$ `secret_...`


