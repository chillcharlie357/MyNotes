---
aliases: 
tags: 
categories:
sticky:
thumbnail:
cover: 
excerpt: false
mathjax: true
comment: true
title: function call实现
date:  2025-09-19 14:09
modified:  2025-09-19 14:09
---
我来详细解释SWE-agent中如何使用litellm支持function calling。让我先查看相关的代码实现：

[4 tools called]

现在让我创建一个详细的任务列表来解释如何使用litellm支持function calling：

[1 tool called]

## SWE-agent中使用litellm支持Function Calling的完整指南

### 1. 核心实现机制

SWE-agent通过`LiteLLMModel`类实现了对litellm function calling的支持。关键代码位于`sweagent/agent/models.py`：

#### **初始化阶段 - 模型兼容性检查**

```python
class LiteLLMModel(AbstractModel):
    def __init__(self, args: GenericAPIModelConfig, tools: ToolConfig):
        # 检查模型是否支持function calling
        if tools.use_function_calling:
            if not litellm.utils.supports_function_calling(model=self.config.name):
                msg = (
                    f"Model {self.config.name} does not support function calling. "
                    "If your model does not support function calling, you can use "
                    "`parse_function='thought_action'` instead."
                )
                self.logger.warning(msg)
```

#### **查询阶段 - 工具传递**

```python
def _single_query(self, messages, n=None, temperature=None):
    extra_args = {}
    
    # 关键：当启用function calling时，将工具定义传递给litellm
    if self.tools.use_function_calling:
        extra_args["tools"] = self.tools.tools
    
    # 调用litellm completion
    response = litellm.completion(
        model=self.config.name,
        messages=messages,
        **extra_args,  # 包含tools参数
        **completion_kwargs,
    )
```

#### **响应处理 - 工具调用提取**

```python
# 处理模型响应中的tool_calls
if self.tools.use_function_calling:
    if response.choices[i].message.tool_calls:
        tool_calls = [call.to_dict() for call in response.choices[i].message.tool_calls]
    else:
        tool_calls = []
    output_dict["tool_calls"] = tool_calls
```

### 2. 工具定义格式转换

SWE-agent将内部的`Command`对象转换为litellm兼容的OpenAI function calling格式：

#### **Command类定义**

```python
class Command(BaseModel):
    name: str
    docstring: str
    arguments: list[Argument]
    
    def get_function_calling_tool(self) -> dict:
        """转换为OpenAI function calling格式"""
        tool = {
            "type": "function",
            "function": {
                "name": self.name,
                "description": self.docstring or "",
            },
        }
        
        properties = {}
        required = []
        
        if self.arguments:
            for arg in self.arguments:
                properties[arg.name] = {
                    "type": arg.type, 
                    "description": arg.description
                }
                if arg.required:
                    required.append(arg.name)
                if arg.enum:
                    properties[arg.name]["enum"] = arg.enum
                    
        tool["function"]["parameters"] = {
            "type": "object", 
            "properties": properties, 
            "required": required
        }
        return tool
```

#### **工具集合生成**

```python
class ToolConfig(BaseModel):
    @cached_property
    def tools(self) -> list[dict]:
        """将所有命令转换为function calling工具列表"""
        return [command.get_function_calling_tool() for command in self.commands]
```

### 3. 配置示例

#### **启用Function Calling的配置**

```yaml
# config/sweagent_0_7/07_fcalling.yaml
agent:
  tools:
    parse_function:
      type: function_calling  # 关键：使用function calling解析器
    bundles:
      - path: tools/registry
      - path: tools/windowed
      - path: tools/search
      - path: tools/windowed_edit_linting
      - path: tools/submit
    enable_bash_tool: true
```

#### **默认配置（也支持function calling）**

```yaml
# config/default.yaml
agent:
  tools:
    parse_function:
      type: function_calling  # 默认就是function calling
    bundles:
      - path: tools/registry
      - path: tools/edit_anthropic
      - path: tools/review_on_submit_m
    enable_bash_tool: true
```

### 4. 工具定义示例

#### **内置Bash工具**

```python
BASH_COMMAND = Command(
    name="bash",
    signature="<command>",
    docstring="runs the given command directly in bash",
    arguments=[
        Argument(
            name="command",
            type="string",
            description="The bash command to execute.",
            required=True,
        )
    ],
)
```

转换为litellm格式后：

```python
{
    "type": "function",
    "function": {
        "name": "bash",
        "description": "runs the given command directly in bash",
        "parameters": {
            "type": "object",
            "properties": {
                "command": {
                    "type": "string",
                    "description": "The bash command to execute."
                }
            },
            "required": ["command"]
        }
    }
}
```

### 5. 解析器实现

#### **FunctionCallingParser**

```python
class FunctionCallingParser(AbstractParseFunction):
    def __call__(self, model_response: dict, commands: list[Command]):
        message = model_response["message"]
        tool_calls = model_response.get("tool_calls", None)
        
        # 验证只有一个工具调用
        if tool_calls is None or len(tool_calls) != 1:
            num_tools = len(tool_calls) if tool_calls else 0
            error_code = "missing" if num_tools == 0 else "multiple"
            raise FunctionCallingFormatError(msg, error_code)
        
        tool_call = tool_calls[0]
        action = self._parse_tool_call(tool_call, commands)
        return message, action
```

### 6. 支持的模型

litellm支持function calling的模型包括：
- **OpenAI**: gpt-3.5-turbo, gpt-4, gpt-4-turbo等
- **Anthropic**: claude-3-sonnet, claude-3-opus等
- **Google**: gemini-pro等
- **其他兼容OpenAI格式的模型**

### 7. 使用步骤总结

1. **配置工具**: 在YAML配置中设置`parse_function.type: function_calling`
2. **定义工具**: 通过`Command`类定义工具，或使用现有的工具包
3. **模型初始化**: `LiteLLMModel`自动检查模型兼容性
4. **工具传递**: 查询时将工具定义传递给litellm
5. **响应解析**: `FunctionCallingParser`解析模型返回的tool_calls
6. **命令执行**: 将解析结果转换为可执行的bash命令

[1 tool called]

### 8. 实际使用示例

如果你想在自己的项目中使用类似的function calling功能，可以这样实现：

```python
import litellm
from litellm import completion

# 定义工具
tools = [
    {
        "type": "function",
        "function": {
            "name": "get_weather",
            "description": "获取指定城市的天气信息",
            "parameters": {
                "type": "object",
                "properties": {
                    "city": {
                        "type": "string",
                        "description": "城市名称"
                    }
                },
                "required": ["city"]
            }
        }
    }
]

# 调用模型
response = completion(
    model="gpt-3.5-turbo",
    messages=[{"role": "user", "content": "北京今天天气怎么样？"}],
    tools=tools,
    tool_choice="auto"
)

# 处理响应
if response.choices[0].message.tool_calls:
    tool_call = response.choices[0].message.tool_calls[0]
    function_name = tool_call.function.name
    function_args = json.loads(tool_call.function.arguments)
    print(f"调用函数: {function_name}, 参数: {function_args}")
```

这就是SWE-agent中litellm function calling的完整实现机制。它通过标准化的工具定义、自动的格式转换和强大的解析能力，实现了LLM与工具的无缝集成。