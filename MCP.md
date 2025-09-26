[TOC]

**MCP Course**

# 课程大纲

| Chapter |            Topic             | Description                                                  |
| :-----: | :--------------------------: | :----------------------------------------------------------- |
|    0    |          Onboarding          | 为您设置您将使用的工具和平台。                               |
|    1    | MCP 基础知识、架构和核心概念 | 解释模型上下文协议 (MCP) 的核心概念、架构和组件。并展示一个使用 MCP 的简单用例。 |
|    2    |   端到端用例：MCP 实际应用   | 构建一个简单的端到端 MCP 应用程序，您可以与社区共享。        |
|    3    |   已部署用例：MCP 实际应用   | 使用 Hugging Face 生态系统和合作伙伴的服务构建已部署的 MCP 应用程序。 |
|    4    |           附加单元           | 帮助您从课程中获得更多收获，与合作伙伴的库和服务合作。       |

推荐的其他课程
- [LLM 课程](https://huggingface.co/learn/llm-course/) 将引导您了解使用和构建 LLM 的基础知识。
- [智能体课程](https://huggingface.co/learn/agents-course/) 将引导您通过 LLMs 构建 AI 智能体。

# 关键概念和术语

## M*N 到 M+N

MCP 通过提供一个标准接口将 M*N 问题转化为 M+N 问题：每个 AI 应用只需实现一次 MCP 客户端，每个工具/数据源只需实现一次 MCP 服务器端。这大大降低了集成复杂性和维护负担

![Multiple Models and Tools](https://huggingface.co/datasets/mcp-course/images/resolve/main/unit1/1a.png)

![With MCP](https://huggingface.co/datasets/mcp-course/images/resolve/main/unit1/2.png)

## 功能
|  Capability   | Description                                                  | Example                                              |
| :-----------: | :----------------------------------------------------------- | :--------------------------------------------------- |
|   **Tools**   | AI 模型可以调用的可执行函数，用于执行操作或检索计算数据。通常与应用程序的使用案例相关。 | 一个天气应用的工具可能是一个返回特定位置天气的功能。 |
| **Resources** | 只读数据源，提供上下文但不进行大量计算。                     | 研究人员助理可能拥有科学论文的资源。                 |
|  **Prompts**  | 预定义的模板或工作流程，用于指导用户、AI 模型和可用功能之间的交互。 | 摘要提示。                                           |
| **Sampling**  | 服务器发起的请求，让客户端/主机执行 LLM 交互，允许递归操作，其中 LLM 可以审查生成的内容并做出进一步的决定。 | 一个写作应用审查其自身输出并决定进一步优化它。       |

![collective diagram](https://huggingface.co/datasets/mcp-course/images/resolve/main/unit1/8.png)

<center style="font-size:14px;color:#C0C0C0;text-decoration:underline"> A use case for a code agent</center> 

|  Entity   |    Name    | Description                                     |
| :-------: | :--------: | :---------------------------------------------- |
|   Tools   | 代码解释器 | 一个可以执行 LLM 编写的代码的工具。             |
| Resources |    文档    | 包含应用程序文档的资源。                        |
|  Prompts  |  代码风格  | 引导 LLM 生成代码的提示。                       |
| Sampling  |  代码审查  | 一个样本，允许 LLM 审查代码并做出进一步的决定。 |

## 组件

![MCP Architecture](https://huggingface.co/datasets/mcp-course/images/resolve/main/unit1/4.png)

###  **Host** ：用户直接交互的面向用户的 AI 应用程序。
> 例如，Anthropic 的 Claude 桌面、AI 增强的 IDE 如 Cursor、推理库如 Hugging Face Python SDK，或使用 LangChain 或 smolagents 库构建的定制应用程序。主机启动与 MCP 服务器的连接，并协调用户请求、LLM 处理和外部工具之间的整体流程。

Host 的职责包括：
- 管理用户交互和权限
- 通过 MCP 客户端启动与 MCP 服务器的连接
- 编排用户请求、LLM 处理和外部工具之间的整体流程
- 以连贯的格式将结果返回给用户

### **Client** ：host 应用程序中的一个组件，负责与特定的 MCP 服务器进行通信。
> 每个客户端与单个服务器保持1:1的连接
处理 MCP 通信的协议级细节
充当主机逻辑与外部服务器之间的中介
### **Server** ：通过 MCP 协议公开功能（Tools、Resources、Prompts）的外部程序或服务。

- 提供对特定外部工具、数据源或服务的访问
- 作为现有功能的轻量级包装
- 可以在本地（在主机所在的同一台机器上）或远程（通过网络）运行
- 以标准化的格式公开其功能，以便客户端可以发现并使用


## 通信流程

### JSON-RPC
在核心上，MCP 使用 JSON-RPC 2.0 作为客户端和服务器之间所有通信的消息格式。JSON-RPC 是一种轻量级的远程过程调用协议，使用 JSON 进行编码，这使得它：

- 可读性强，易于调试
- 语言无关，支持在任何编程环境中实现
- 已建立良好，具有明确的规范和广泛的应用

![message types](https://huggingface.co/datasets/mcp-course/images/resolve/main/unit1/5.png)

#### 请求

从 Client 发送到 Server 以启动操作。请求消息包括：

- 一个唯一标识符（`id`）
- 要调用的方法名称（例如，`tools/call`）
- 方法参数（如有）

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "tools/call",
  "params": {
    "name": "weather",
    "arguments": {
      "location": "San Francisco"
    }
  }
}
```

####  响应

从 Server 发送到 Client 的响应。响应消息包括：

- 与相应请求相同的 `id`
- 成功时的 `result` 或失败时的 `error`

```JSON
# 成功响应
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "temperature": 62,
    "conditions": "Partly cloudy"
  }
}
```

```JSON
# 错误响应
{
  "jsonrpc": "2.0",
  "id": 1,
  "error": {
    "code": -32602,
    "message": "Invalid location parameter"
  }
}
```

#### 通知

单向消息，无需回复。通常从 Server 发送到 Client，以提供事件更新或通知

```json
{
  "jsonrpc": "2.0",
  "method": "progress",
  "params": {
    "message": "Processing data...",
    "percent": 50
  }
}
```

### 传输机制

#### 标准输入 / 输出 (stdio)

1. stdio 传输用于本地通信，此时 Client 和 Server 在同一台机器上运行：

> Host 应用程序以子进程的形式启动 Server，并通过向其标准输入 (stdin) 写入和从其标准输出 (stdout) 读取与它进行通信。

2. 用例包括本地 Tools，如文件系统访问或运行本地脚本。
3. 这种传输方式的主要**优点**是简单、无需网络配置，并且由操作系统安全沙箱隔离

#### HTTP + SSE（服务器发送事件）/  流式 HTTP

1. HTTP+SSE 传输用于远程通信，客户端和服务器可能位于不同的机器上

> 通信通过 HTTP 进行，服务器使用服务器端事件（SSE）通过持久连接向客户端推送更新

2. 此传输的用例包括连接到远程 API、云服务或共享资源。
2. 此传输的**优点**主要包括它可以在网络上工作、支持与 Web 服务的集成，以及与无服务器环境兼容。
2. 近期对 MCP 标准的更新引入或完善了“流式 HTTP”，它通过允许服务器在需要时动态升级到 SSE 进行流式传输，从而提供了更多灵活性，同时保持与无服务器环境的兼容性

#### 交互生命周期

让我们看看这些组件在典型的 MCP 工作流程中是如何交互的：

1. **用户交互** ：用户与**Host**应用程序交互，表达意图或查询。
2. **Host 处理** ： **Host** 处理用户的输入，可能使用 LLM 来理解请求并确定可能需要的哪些外部功能。
3. **Client 连接** ：**Host** 将指令其 **Client** 组件连接到适当的 **Server**。
4. **能力发现** ：**Client** 查询 **Server** 以发现其提供的能力（Tools、Resources、Prompts）。
5. **能力调用** ：根据用户需求或 LLM 的判断，**Host** 指示 **Client** 从 **Server** 调用特定的能力。
6. **服务器执行** ：**Server** 执行请求的功能，并将结果返回给 **Client**。
7. **结果集成** ：**Client** 将这些结果反馈给 **Host** ，Host 将它们纳入 LLM 的上下文中或直接向用户展示。
