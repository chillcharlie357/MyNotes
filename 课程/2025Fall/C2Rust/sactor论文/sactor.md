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
title: sactor
date:  2025-09-15 23:09
modified:  2025-09-16 10:09
---

# divider

1. 处理无依赖项目
2. 处理循环依赖，整个加到result里

[[Divider_拓扑排序]]

拓扑排序的核心：
1. 找到 **没有前驱（入度为 0）** 的节点
2. 把它加入序列，并移除它的所有出边
3. 重复以上过程，直到所有节点处理完

基于DFS的拓扑排序：
1. 对未访问的节点做 DFS
2. 每次递归结束时，把节点加入栈（或结果列表前端）
3. 最后反转栈，即得到拓扑序列
4. 如果 DFS 过程中遇到回边（访问到正在访问的节点），说明有环


# Clang.Cindex


- `Index` and `TranslationUnit`: 
    
    The `Index` object manages Clang's internal data structures, and a `TranslationUnit` represents a parsed source file and its associated AST.
    
- `Cursor`: 
    
    `Cursor` objects represent nodes in the AST, allowing you to access their kind, display name, location, and children.
    
- `Token`: 
    
    `Token` objects represent individual lexical units (like keywords, identifiers, operators) in the source code.
    
- **Source Location and Range:** 
    
    `SourceLocation` and `SourceRange` objects provide information about the position of elements in the source files.


[Parsing C++ in Python with Clang - Eli Bendersky's website](https://eli.thegreenplace.net/2011/07/03/parsing-c-in-python-with-clang)



# CParser
### CParser 类
这是主要的C语言解析器类，使用libclang来解析C源代码文件。

### 核心方法

#### 1. 初始化方法
```python
def __init__(self, filename, extra_args=None, omit_error=False):
```
- **功能**: 初始化C解析器
- **参数**: 
  - `filename`: C源文件路径
  - `extra_args`: 额外的编译参数
  - `omit_error`: 是否忽略解析错误
- **作用**: 解析C文件生成AST，初始化数据结构存储解析结果

#### 2. 类型检查方法
```python
@staticmethod
def is_func_type(t: cindex.Type) -> bool:
```
- **功能**: 检查类型是否为函数类型
- **返回**: 布尔值，表示是否为函数类型

#### 3. 获取信息的方法

**获取结构体信息**:
```python
def get_struct_info(self, struct_name):
def get_structs(self) -> list[StructInfo]:
```
- **功能**: 获取指定结构体信息或所有结构体列表

**获取函数信息**:
```python
def get_function_info(self, function_name) -> FunctionInfo:
def get_functions(self) -> list[FunctionInfo]:
```
- **功能**: 获取指定函数信息或所有函数列表

**获取全局变量信息**:
```python
def get_global_var_info(self, global_var_name):
def get_global_vars(self) -> list[GlobalVarInfo]:
```
- **功能**: 获取指定全局变量信息或所有全局变量列表

**获取枚举信息**:
```python
def get_enum_info(self, enum_name):
def get_enums(self) -> list[EnumInfo]:
```
- **功能**: 获取指定枚举信息或所有枚举列表

#### 4. 提取方法

**提取类型别名**:
```python
def _extract_type_alias(self):
```
- **功能**: 从C文件中提取typedef类型别名
- **返回**: 字典，映射别名名称到原始类型

**提取结构体和联合体**:
```python
def _extract_structs_unions(self):
```
- **功能**: 解析C文件并提取结构体和联合体信息

**提取函数信息**:
```python
def _extract_functions(self):
```
- **功能**: 解析C文件并提取函数信息

#### 5. 依赖关系处理方法

**更新函数依赖**:
```python
def _update_function_dependencies(self, function: FunctionInfo):
```
- **功能**: 更新函数的依赖关系，找出函数内部调用的其他函数

**更新结构体依赖**:
```python
def _update_struct_dependencies(self, struct_union: StructInfo):
```
- **功能**: 更新结构体/联合体的依赖关系

#### 6. 代码提取方法

**提取函数代码**:
```python
def extract_function_code(self, function_name):
```
- **功能**: 从文件中提取指定函数的完整代码

**提取结构体定义代码**:
```python
def extract_struct_union_definition_code(self, struct_union_name):
```
- **功能**: 提取结构体或联合体的定义代码

**提取枚举定义代码**:
```python
def extract_enum_definition_code(self, enum_name):
```
- **功能**: 提取枚举的定义代码

**提取全局变量定义代码**:
```python
def extract_global_var_definition_code(self, global_var_name):
```
- **功能**: 提取全局变量的定义代码

#### 7. 辅助方法

**检查系统头文件**:
```python
def _is_in_system_header(self, node):
```
- **功能**: 判断节点是否在系统头文件中

**打印AST**:
```python
def print_ast(self, node=None, indent=0):
```
- **功能**: 打印抽象语法树，用于调试

**统计信息**:
```python
def statistic(self):
```
- **功能**: 生成解析结果的统计信息，包括函数、结构体、全局变量等的详细信息

## 主要用途

这个C解析器主要用于：
1. **代码分析**: 解析C源代码，提取函数、结构体、枚举、全局变量等信息
2. **依赖分析**: 分析代码中各种元素之间的依赖关系
3. **代码转换**: 为C到Rust的代码转换提供基础信息
4. **代码生成**: 基于解析结果生成测试代码或其他代码

这个解析器是sactor项目（C到Rust转换工具）的核心组件之一，为后续的代码转换和验证提供必要的信息。





# Combiner