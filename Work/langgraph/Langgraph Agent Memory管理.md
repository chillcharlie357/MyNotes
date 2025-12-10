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
title: Langgraph Agent Memory管理
date:  2025-12-08 17:12
modified:  2025-12-10 10:12
---

# 一、Agent Memory 概述

## 1.1 基本概念

- **Agent Memory定义**：赋予AI智能体记忆能力的技术架构
- **核心价值**：使Agent能够记住过往交互，保持上下文一致性，适应用户偏好
- **记忆分类**：短期记忆（会话级别）和长期记忆（应用级别）

## 1.2 记忆工作机制

```
记忆存储 → 记忆更新 → 记忆检索
```

# 二、LangGraph记忆架构

## 2.1 记忆类型

### 短期记忆

- **短期记忆**：通过Checkpointer实现，维护对话上下文
	- 对话messages

### 长期记忆

- **长期记忆**：通过Store实现，跨会话持久化存储
	- 事实 facts ([semantic memory](https://docs.langchain.com/oss/python/concepts/memory#semantic-memory))，如用户画像
	- 经验 experiences ([episodic memory](https://docs.langchain.com/oss/python/concepts/memory#episodic-memory))，过去成功完成任务的事件或行动
	- 规则 rules ([procedural memory](https://docs.langchain.com/oss/python/concepts/memory#procedural-memory)).

| Memory Type                                                                           | What is Stored | Human Example              | Agent Example       |
| ------------------------------------------------------------------------------------- | -------------- | -------------------------- | ------------------- |
| [Semantic](https://docs.langchain.com/oss/python/concepts/memory#semantic-memory)     | Facts          | Things I learned in school | Facts about a user  |
| [Episodic](https://docs.langchain.com/oss/python/concepts/memory#episodic-memory)     | Experiences    | Things I did               | Past agent actions  |
| [Procedural](https://docs.langchain.com/oss/python/concepts/memory#procedural-memory) | Instructions   | Instincts or motor skills  | Agent system prompt |

## 2.2 核心组件

| 组件           | 功能     | 特点                   |
| ------------ | ------ | -------------------- |
| Checkpointer | 状态快照管理 | 自动化保存图状态             |
| Thread       | 对话线程管理 | 唯一标识符（thread_id）区分会话 |
| Store        | 长期存储   | 键值数据库，支持语义检索         |

# 三、短期记忆实现详解

## 3.1 内存存储（InMemorySaver）

```python
from langgraph.checkpoint.memory import InMemorySaver
from langchain.chat_models import init_chat_model
from langgraph.prebuilt import create_react_agent

# 初始化检查点保存器
checkpointer = InMemorySaver()

# 创建带有记忆能力的Agent
model = init_chat_model(
    model="gpt-4",
    model_provider="openai",
    base_url=BASE_URL,
    api_key=TOKEN,
    temperature=0,
)

agent = create_react_agent(
    model=model,
    tools=[],
    checkpointer=checkpointer  # 关键：注入持久化能力
)

# 使用thread_id维持对话状态
config = {"configurable": {"thread_id": "1"}}

# 第一次对话
response1 = agent.invoke(
    {"messages": [{"role": "user", "content": "你好，我叫ada！"}]},
    config
)

# 第二次对话（保持记忆）
response2 = agent.invoke(
    {"messages": [{"role": "user", "content": "你还记得我叫什么吗？"}]},
    config  # 相同thread_id维持记忆
)
```

## 3.2 数据库持久化存储

```python
from langgraph.checkpoint.postgres import PostgresSaver
from langgraph.graph import StateGraph, MessagesState, START

# PostgreSQL配置
DB_URI = "postgresql://user:pass@localhost:5432/dbname"

with PostgresSaver.from_conn_string(DB_URI) as checkpointer:
    checkpointer.setup()  # 首次调用必须setup
    
    def call_model(state: MessagesState):
        response = model.invoke(state["messages"])
        return {"messages": response}
    
    # 构建图
    builder = StateGraph(MessagesState)
    builder.add_node(call_model)
    builder.add_edge(START, "call_model")
    graph = builder.compile(checkpointer=checkpointer)
    
    # 持久化对话
    config = {"configurable": {"thread_id": "session_123"}}
    graph.invoke({"messages": [{"role": "user", "content": "Hello"}]}, config)
```

## 3.3 记忆管理策略对比

| 策略        | 实现方式              | 适用场景    | 优缺点         |
| --------- | ----------------- | ------- | ----------- |
| **修剪消息**​ | trim_messages()   | 上下文窗口有限 | 简单高效，但信息丢失  |
| **删除消息**​ | RemoveMessage     | 清理冗余信息  | 精确控制，需自定义逻辑 |
| **总结消息**​ | SummarizationNode | 长期对话    | 保留语义，计算成本高  |

# 四、长期记忆实现详解

## 4.1 Store架构设计

```python
from langgraph.store.memory import InMemoryStore
from typing import Annotated
from langgraph.config import get_store

# 初始化Store
store = InMemoryStore()

# 定义数据结构
class UserProfile(TypedDict):
    name: str
    preferences: Dict[str, Any]
    conversation_history: List[str]

# 存储长期记忆
def save_user_profile(
    user_info: UserProfile, 
    config: RunnableConfig
) -> str:
    store = get_store()
    user_id = config["configurable"].get("user_id")
    
    # 使用命名空间组织数据
    namespace = ("user_profiles",)
    store.put(namespace, user_id, user_info)
    
    return "用户信息已保存"

# 检索长期记忆
def get_user_profile(config: RunnableConfig) -> str:
    store = get_store()
    user_id = config["configurable"].get("user_id")
    
    profile = store.get(("user_profiles",), user_id)
    return profile.value if profile else "未找到用户信息"
```

## 4.2 语义搜索实现

```python
from langgraph.store.base import BaseStore
from langchain.embeddings import HuggingFaceEmbeddings

class SemanticMemoryStore:
    def __init__(self):
        self.embedder = HuggingFaceEmbeddings()
        self.store = InMemoryStore()
    
    def add_memory(self, namespace: tuple, key: str, content: str):
        # 生成嵌入向量
        embedding = self.embedder.embed_query(content)
        
        # 存储原始内容和向量
        memory_data = {
            "content": content,
            "embedding": embedding,
            "timestamp": datetime.now()
        }
        
        self.store.put(namespace, key, memory_data)
    
    def search_memories(self, namespace: tuple, query: str, limit: int = 5):
        # 语义搜索
        query_embedding = self.embedder.embed_query(query)
        all_memories = self.store.get_all(namespace)
        
        # 计算相似度
        similarities = []
        for key, memory in all_memories.items():
            similarity = cosine_similarity(query_embedding, memory.value["embedding"])
            similarities.append((similarity, memory))
        
        # 返回最相关的记忆
        similarities.sort(reverse=True)
        return similarities[:limit]
```

## 4.3 记忆更新机制

```python
from langgraph.types import Command

def update_user_memory(
    tool_call_id: Annotated[str, InjectedToolCallId],
    config: RunnableConfig
) -> Command:
    """更新用户记忆的工具函数"""
    store = get_store()
    user_id = config["configurable"].get("user_id")
    
    # 从对话中提取关键信息
    latest_message = state["messages"][-1].content
    extracted_info = extract_key_information(latest_message)
    
    # 更新长期记忆
    namespace = ("user_preferences",)
    current_prefs = store.get(namespace, user_id) or {}
    updated_prefs = {**current_prefs, **extracted_info}
    
    store.put(namespace, user_id, updated_prefs)
    
    return Command(update={
        "user_profile": updated_prefs,
        "messages": [ToolMessage("记忆已更新", tool_call_id=tool_call_id)]
    })
```

# 五、记忆管理策略详解

## 5.1 智能修剪实现

```python
from langchain_core.messages.utils import trim_messages
import tiktoken

def smart_trim_messages(messages: List[BaseMessage], 
                       max_tokens: int = 4000,
                       strategy: str = "last") -> List[BaseMessage]:
    """
    智能消息修剪策略
    """
    token_counter = tiktoken.get_encoding("cl100k_base")
    
    # 计算当前token数量
    total_tokens = sum(len(token_counter.encode(msg.content)) for msg in messages)
    
    if total_tokens <= max_tokens:
        return messages
    
    if strategy == "last":
        # 保留最近的消息
        return trim_messages(
            messages,
            strategy="last",
            token_counter=token_counter.encode,
            max_tokens=max_tokens,
            start_on="human",
            include_system=True
        )
    elif strategy == "first":
        # 保留系统消息和最早的用户消息
        return trim_messages(
            messages,
            strategy="first", 
            token_counter=token_counter.encode,
            max_tokens=max_tokens,
            include_system=True
        )
```

## 5.2 分层记忆管理

```python
class HierarchicalMemoryManager:
    def __init__(self):
        self.short_term = InMemorySaver()  # 短期记忆
        self.long_term = InMemoryStore()   # 长期记忆
        self.working_memory = {}           # 工作记忆
    
    def process_conversation(self, messages: List[BaseMessage], thread_id: str):
        # 1. 更新短期记忆
        self.short_term.save_state(thread_id, {"messages": messages})
        
        # 2. 提取关键信息到长期记忆
        key_info = self.extract_key_information(messages)
        if key_info:
            self.long_term.put(("conversation_keypoints",), 
                             f"{thread_id}_{datetime.now()}", key_info)
        
        # 3. 维护工作记忆（最近3轮对话）
        self.working_memory[thread_id] = messages[-6:]  # 最近3轮
        
    def get_context(self, thread_id: str) -> Dict:
        """获取完整的对话上下文"""
        return {
            "working_memory": self.working_memory.get(thread_id, []),
            "long_term_memories": self.retrieve_relevant_memories(thread_id),
            "short_term_state": self.short_term.get_state(thread_id)
        }
```

## 5.3 记忆检索优化

```python
def advanced_memory_retrieval(query: str, 
                             user_id: str, 
                             context: Dict) -> List[Any]:
    """
    高级记忆检索：结合多种策略
    """
    results = []
    
    # 1. 精确匹配检索
    exact_match = store.get(("exact_matches", user_id), query)
    if exact_match:
        results.append(("exact", exact_match.value))
    
    # 2. 语义相似性检索
    semantic_results = store.search(
        ("semantic_memories", user_id), 
        query=query, 
        limit=3
    )
    results.extend([("semantic", r.value) for r in semantic_results])
    
    # 3. 时间相关性检索（最近记忆优先）
    recent_memories = store.get_recent(("temporal_memories", user_id), limit=5)
    results.extend([("temporal", r.value) for r in recent_memories])
    
    # 4. 综合评分和排序
    scored_results = self.rank_memories(results, query, context)
    
    return scored_results[:5]  # 返回Top-5最相关记忆
```

## 5.4 生产环境配置示例

```python
# 生产环境记忆管理系统配置
MEMORY_CONFIG = {
    "short_term": {
        "checkpointer": "PostgresSaver",
        "db_uri": "postgresql://user:pass@db.example.com:5432/agent_memory",
        "retention_days": 30  # 短期记忆保留30天
    },
    "long_term": {
        "store": "RedisStore",
        "redis_url": "redis://cache.example.com:6379",
        "indexing": {
            "embedding_model": "text-embedding-3-large",
            "vector_dim": 3072
        }
    },
    "policies": {
        "auto_summarize_after": 20,  # 20轮对话后自动总结
        "max_short_term_messages": 50,  # 短期记忆最多保存50条消息
        "pruning_strategy": "adaptive"  # 自适应修剪策略
    }
}
```

# 八、总结

本文详细阐述了基于LangGraph框架的AI智能体长短期记忆管理完整解决方案，从基础概念到生产环境实践，涵盖了记忆存储、检索、更新和管理的各个方面，为构建具有持久记忆能力的智能体系统提供了全面指导。

**核心价值**：通过有效的记忆管理，AI智能体能够实现真正的连贯性、上下文感知和个性化交互，显著提升用户体验和系统智能水平。

# 参考链接

- [让AI智能体拥有像人类的持久记忆：基于LangGraph的长短期记忆管理实践指南](https://mp.weixin.qq.com/s/sARM1GWhKQAHEInhEheZiA)
- [Short-term memory - Docs by LangChain](https://docs.langchain.com/oss/python/langchain/short-term-memory#trim-messages)
- [Memory overview - Docs by LangChain](https://docs.langchain.com/oss/python/concepts/memory)