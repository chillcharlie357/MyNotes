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

