# 有时候，或许我们不需要Web Socket。

> 实现：~~web socket~~, Server send event(SSE)
>
> 应用场景：站内信，消息推送，日志显示，chatGPT...

直接上代码：

## 后端（nest）

```javascript
// sse接口每2s响应一段数据。 
 @Sse('stream')
  stream() {
    return new Observable((observer) => {
      observer.next({ data: { msg: 'start send message , waiting....' } });
      setInterval(() => {
        observer.next({ data: { msg: `this is a message` } });
      }, 2000);
    });
  }
```

## 前端（react）

```tsx
import { useEffect, useState } from 'react'

function App() {
  const [messages, setMessages] = useState<string[]>([])
  useEffect(() => {
    const eventSource = new EventSource('http://localhost:8686/sse/stream')
    eventSource.onmessage = ({ data }) => {
      setMessages((state) => [...state, JSON.parse(data).msg])
    }
  }, [])
  return (
    <div className="container">
      <ul>
        {messages.map((msg, index) => (
          <li key={index}>
            {index}.{msg}
          </li>
        ))}
      </ul>
    </div>
  )
}

export default App

```

## 效果

![image-20230901111330436](C:\Users\25704\AppData\Roaming\Typora\typora-user-images\image-20230901111330436.png)

![sse](C:\Users\25704\Desktop\sse.gif)