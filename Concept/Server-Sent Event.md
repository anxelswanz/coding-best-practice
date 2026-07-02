
# 1. Definition

It is a technology that allows the server to actively and in real time to push data to client using HTTP protocol. 
**SSE（Server-Sent Events，服务器发送事件）** 是一种允许服务器在有新数据时，**主动、实时**地将数据推送到浏览器（客户端）的技术。

# 2. How it works 

**Establish Long Connection** 

传统的网页交互是“请求-响应”模式（客户端要一次，服务器给一次）。而 SSE 是基于 HTTP 协议的长连接：

1. **客户端发起请求**：浏览器向服务器发送一个普通的 HTTP 请求，但其请求头中会特别注明它想要建立一个 SSE 连接（`Accept: text/event-stream`）。
    
2. **服务器保持连接**：服务器收到请求后，不关闭这个 HTTP 连接，而是将响应头设置为 `Content-Type: text/event-stream`。
    
3. **持续推送数据**：每当服务器有新消息时，就会通过这个不断开的通道，以特定的文本格式（格式通常为 `data: 你的内容\n\n`）实时发送给浏览器。

