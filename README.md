# 🌸 Mori

> 基于 AgentScope 的虚拟 AI 女友 Agent 系统

[![Python Version](https://img.shields.io/badge/python-3.10%2B-blue.svg)](https://www.python.org/downloads/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![AgentScope](https://img.shields.io/badge/AgentScope-1.0.8%2B-orange.svg)](https://github.com/modelscope/agentscope)
[![Code Style](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)

---

## 📖 项目简介

**Mori** 是一个基于 [AgentScope](https://github.com/modelscope/agentscope) 框架构建的虚拟 AI 女友 Agent 系统。项目充分利用 AgentScope 已有的功能（Model、Agent、Tool、Memory 等），专注于业务逻辑和用户体验的实现。

### 🎯 解决的问题

- **情感陪伴需求**：为用户提供温暖、贴心的虚拟陪伴体验
- **个性化交互**：通过长期记忆功能记住用户偏好，实现个性化对话
- **灵活扩展**：支持多 Agent 协作架构，可根据需求扩展功能

### 💡 核心价值

- **温柔体贴的人设**：Mori 具有温柔、善解人意、幽默风趣的性格特点
- **持久记忆能力**：能够记住用户的偏好、习惯和重要信息
- **多模型支持**：支持 OpenAI、通义千问、DeepSeek、Ollama 等多种 LLM
- **易于定制**：通过 Jinja2 模板系统轻松定制 Agent 人设和行为

---

## ✨ 功能特性

### 核心功能

- 🤖 **多 Agent 架构**：支持主 Agent 与子 Agent 协作，子 Agent 自动注册为主 Agent 的工具
- 🧠 **长期记忆**：基于 Mem0 实现，支持跨会话记忆用户偏好和重要信息
- 💬 **流式对话**：支持流式输出，提供更自然的对话体验
- 🔧 **工具调用**：支持自定义工具和并行工具调用
- 🌐 **MCP 协议**：预留 Model Context Protocol 集成接口

### 模型支持

| 模型类型 | 提供商 | 说明 |
|---------|--------|------|
| OpenAI | OpenAI | GPT-4、GPT-3.5-Turbo 等 |
| DashScope | 阿里云 | 通义千问系列 |
| DeepSeek | DeepSeek | DeepSeek-Chat 等 |
| Ollama | 本地 | Llama3、Qwen 等本地模型 |
| Gemini | Google | Gemini 系列 |

### 嵌入模型支持

- DashScope (text-embedding-v2/v3)
- OpenAI (text-embedding-3-small/large)
- Gemini (text-embedding-004)
- Ollama (nomic-embed-text, mxbai-embed-large)

---

## 🛠️ 技术栈

| 类别 | 技术 | 版本 |
|------|------|
| **核心框架** | AgentScope | 1.0.8+ |
| **模板引擎** | Jinja2 | 3.1.0+ |
| **配置验证** | Pydantic | 2.0+ |
| **GUI 框架** | Gradio | 4.0.0+ |
| **长期记忆** | Mem0AI | 0.1.0+ |
| **HTTP 客户端** | httpx | 0.25.0+ |
| **依赖管理** | uv | - |
| **代码规范** | black, ruff, pre-commit | - |
| **Python** | Python | 3.10+ |

---

## 🚀 快速开始

### 环境要求

- Python 3.10 或更高版本
- [uv](https://github.com/astral-sh/uv) 包管理器
- 一个 LLM API 密钥（OpenAI、DeepSeek、通义千问等）

### 安装步骤

#### 1. 克隆项目

```bash
git clone https://github.com/acgurl/mori.git
cd mori
```

#### 2. 创建虚拟环境并安装依赖

```bash
# 创建虚拟环境
uv venv

# 激活虚拟环境
# Windows PowerShell:
.venv\Scripts\activate
# Linux/Mac:
source .venv/bin/activate

# 安装项目依赖
uv pip install -e .

# 安装开发依赖（可选）
uv pip install -e ".[dev]"
```

#### 3. 配置文件

```bash
# Windows PowerShell:
Copy-Item config\models.yaml.example config\models.yaml
Copy-Item config\agents.yaml.example config\agents.yaml
Copy-Item config\config.yaml.example config\config.yaml

# Linux/Mac:
cp config/models.yaml.example config/models.yaml
cp config/agents.yaml.example config/agents.yaml
cp config/config.yaml.example config/config.yaml
```

#### 4. 设置 API 密钥

```bash
# Windows PowerShell:
$env:OPENAI_API_KEY="your-api-key-here"

# Linux/Mac:
export OPENAI_API_KEY="your-api-key-here"
```

#### 5. 运行应用

```bash
python gui/app.py
```

然后在浏览器中访问: http://localhost:7860

---

## ⚙️ 配置说明

### 配置文件结构

```
config/
├── models.yaml          # 模型配置
├── agents.yaml          # Agent 配置
├── config.yaml          # 全局配置
├── mcp.json             # MCP 配置（可选）
└── template/            # 自定义模板目录
    └── custom.jinja2    # 自定义提示词模板
```

### models.yaml - 模型配置

```yaml
models:
  # OpenAI 配置
  main_gpt4:
    model_name: gpt-4
    model_type: openai
    api_key: ${OPENAI_API_KEY}  # 从环境变量读取
    generate_kwargs:
      temperature: 0.7
      max_tokens: 2000

  # DeepSeek 配置（OpenAI 兼容接口）
  deepseek_chat:
    model_name: deepseek-chat
    model_type: openai
    api_key: ${DEEPSEEK_API_KEY}
    base_url: https://api.deepseek.com/v1
    generate_kwargs:
      temperature: 0.7

  # 通义千问配置
  qwen_max:
    model_name: qwen-max
    model_type: dashscope
    api_key: ${DASHSCOPE_API_KEY}

  # Ollama 本地模型
  local_llama3:
    model_name: llama3
    model_type: ollama
    base_url: http://localhost:11434

# 嵌入模型配置（用于长期记忆）
embedding_models:
  dashscope_embedding:
    model_name: text-embedding-v2
    model_type: dashscope
    api_key: ${DASHSCOPE_API_KEY}
```

### agents.yaml - Agent 配置

```yaml
# 指定主 Agent
primary_agent: mori

agents:
  # 主 Agent - 虚拟 AI 女友
 mori:
    model: main_gpt4           # 引用 models.yaml 中的配置
    template: mori             # 模板名称
    parallel_tool_calls: true
    memory_config:
      type: memory
      max_length: 100

    # 长期记忆配置
    long_term_memory:
      enabled: true
      mode: "agent_control"    # agent_control / static_control / both
      user_name: "user"
      embedding_model: "dashscope_embedding"
      storage_path: "data/memory/mori"
      on_disk: true
```

### config.yaml - 全局配置

```yaml
global:
  log_level: INFO
  log_dir: logs

server:
  host: 127.0.0.1
  port: 7860
 share: false
```

### 环境变量

| 变量名 | 说明 |
|--------|------|
| `OPENAI_API_KEY` | OpenAI API 密钥 |
| `DASHSCOPE_API_KEY` | 阿里云 DashScope API 密钥 |
| `DEEPSEEK_API_KEY` | DeepSeek API 密钥 |
| `GEMINI_API_KEY` | Google Gemini API 密钥 |

---

## 📁 项目结构

```
mori/
├── mori/                          # 核心模块
│   ├── __init__.py
│   ├── mori.py                    # Mori 核心封装类
│   ├── config.py                  # 配置加载和验证
│   ├── exceptions.py              # 自定义异常
│   │
│   ├── agent/                     # Agent 相关
│   │   ├── factory.py             # Agent 工厂
│   │   └── manager.py             # Agent 管理器
│   │
│   ├── model/                     # 模型相关
│   │   └── factory.py             # 模型工厂
│   │
│   ├── memory/                    # 记忆相关
│   │   └── factory.py             # 记忆工厂
│   │
│   ├── template/                  # 模板系统
│   │   ├── loader.py              # 模板加载器
│   │   ├── service.py             # 模板服务
│   │   └── internal_template/     # 内置模板
│   │       └── mori.jinja2        # Mori 提示词模板
│   │
│   ├── tool/                      # 工具系统
│   │   ├── factory.py             # 工具工厂
│   │   ├── agent_tools.py         # Agent 工具
│   │   └── internal_tools/        # 内置工具
│   │       └── example_tools.py   # 示例工具
│   │
│   ├── mcp/                       # MCP 集成（预留）
│   │   └── README.md
│   │
│   └── utils/                     # 工具函数
│       ├── model_wrapper.py       # 模型包装器
│       └── response.py            # 响应处理
│
├── gui/                           # GUI 界面
│   └── app.py                     # Gradio 应用
│
├── logger/                        # 日志系统
│   └── config.py                  # 日志配置
│
├── config/                        # 配置文件
│   ├── models.yaml.example        # 模型配置示例
│   ├── agents.yaml.example        # Agent 配置示例
│   ├── config.yaml.example        # 全局配置示例
│   └── template/                  # 自定义模板目录
│
├── tests/                         # 测试
│   ├── test_config.py
│   ├── test_template.py
│   └── ...
│
├── docs/                          # 文档
│   ├── ARCHITECTURE.md            # 架构设计
│   ├── QUICKSTART.md              # 快速开始
│   ├── LONG_TERM_MEMORY.md        # 长期记忆指南
│   └── ...
│
├── pyproject.toml                 # 项目配置
├── .pre-commit-config.yaml        # pre-commit 配置
├── LICENSE                        # MIT 许可证
└── README.md                      # 项目说明
```

---

## 📝 使用示例

### 基本对话

```python
import asyncio
from mori.mori import Mori

async def main():
    # 初始化 Mori
    mori = Mori(config_dir="config")

    # 发送消息
    response = await mori.chat("你好，今天过得怎么样？")
    print(response)

    # 继续对话
    response = await mori.chat("我今天工作有点累")
    print(response)

    # 重置对话历史
    await mori.reset()

if __name__ == "__main__":
    asyncio.run(main())
```

### 使用长期记忆

```python
import asyncio
from mori.mori import Mori

async def memory_example():
    mori = Mori(config_dir="config")

    # 分享偏好（Agent 会自动记录）
    response = await mori.chat("我喜欢喝拿铁咖啡")
    print(response)

    # 清空短期记忆（模拟新会话）
    await mori.reset()

    # 询问偏好（Agent 会从长期记忆检索）
    response = await mori.chat("我喜欢喝什么咖啡？")
    print(response)

asyncio.run(memory_example())
```

### 多 Agent 协作

```python
import asyncio
from mori.mori import Mori

async def multi_agent_example():
    mori = Mori(config_dir="config")

    # 主 Agent 会根据任务自动调用子 Agent
    response = await mori.chat("帮我写一首关于春天的诗")
    print(response)

    # 查看可用的 Agent
    print(f"可用 Agents: {mori.list_agents()}")
    print(f"主 Agent: {mori.get_primary_agent_name()}")

asyncio.run(multi_agent_example())
```

### 自定义模板

在 `config/template/` 目录下创建自定义模板：

```jinja2
{# config/template/custom.jinja2 #}
你是一个专业的技术助手。

## 当前信息
{% if current_time %}
- **当前时间**: {{ current_time }}
{% endif %}

## 你的职责
- 回答技术问题
- 提供代码示例
- 解释技术概念

请用专业、清晰的语言回答用户的问题。
```

然后在 `agents.yaml` 中引用：

```yaml
agents:
  tech_assistant:
    model: main_gpt4
    template: custom  # 使用自定义模板
```

---

## 🧪 开发指南

### 安装开发依赖

```bash
uv pip install -e ".[dev]"
pre-commit install
```

### 运行测试

```bash
# 运行所有测试
pytest tests/

# 运行特定测试
pytest tests/test_config.py

# 带详细输出
pytest tests/ -v
```

### 代码规范

```bash
# 运行所有检查
pre-commit run --all-files

# 格式化代码
black .

# 检查代码风格
ruff check .
```

### 添加自定义工具

在 `mori/tool/internal_tools/` 中创建新工具：

```python
from agentscope.tool import ToolResponse
from agentscope.message import TextBlock

async def my_custom_tool(param: str) -> ToolResponse:
    """我的自定义工具

    Args:
        param: 参数说明

    Returns:
        ToolResponse: 工具响应
    """
    result = f"处理结果: {param}"
    return ToolResponse(
        content=[TextBlock(type="text", text=result)]
    )

# 注册工具
def register_tools(toolkit):
    toolkit.register_tool_function(my_custom_tool)
```

---

## 🤝 贡献指南

我们欢迎所有形式的贡献！

### 如何贡献

1. **Fork** 本仓库
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 创建 **Pull Request**

### 贡献规范

- 遵循现有的代码风格（使用 black 和 ruff）
- 为新功能添加测试
- 更新相关文档
- 提交信息清晰明了

### 报告问题

如果你发现了 bug 或有功能建议，请在 [GitHub Issues](https://github.com/acgurl/mori/issues) 中提交。

---

## 📄 许可证

本项目采用 [MIT 许可证](LICENSE)。

```
MIT License

Copyright (c) 2025 ACGURL

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.
```

---

## 🔗 相关链接

- **项目仓库**: [https://github.com/acgurl/mori](https://github.com/acgurl/mori)
- **问题反馈**: [GitHub Issues](https://github.com/acgurl/mori/issues)
- **AgentScope**: [https://github.com/modelscope/agentscope](https://github.com/modelscope/agentscope)
- **AgentScope 文档**: [https://doc.agentscope.io/](https://doc.agentscope.io/)

---

## 📚 更多文档

- [架构设计](docs/ARCHITECTURE.md) - 详细的系统架构说明
- [快速开始](docs/QUICKSTART.md) - 5 分钟快速上手指南
- [长期记忆指南](docs/LONG_TERM_MEMORY.md) - 长期记忆功能详细说明
- [日志系统](docs/LOGGING.md) - 日志配置说明

---

<p align="center">
  Made with 💕 by <a href="https://github.com/acgurl">ACGURL</a>
</p>
