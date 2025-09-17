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
title: myc2rust
date:  2025-09-17 16:09
modified:  2025-09-17 17:09
---

# 项目环境配置

1. 需要一个Linux环境
2. 配置c2rust环境：[GitHub - immunant/c2rust: Migrate C code to Rust](https://github.com/immunant/c2rust)
	1. 安装`libclang-dev` 和 `llvm-dev` 包
3. 安装Bear: [GitHub - rizsotto/Bear: Bear is a tool that generates a compilation database for clang tooling.](https://github.com/rizsotto/Bear)

# 架构

[[项目架构图]]

## c2rust

1. 用 `bear make` 或 `intercept-build make` 生成 `compile_commands.json`
2. 跑 `c2rust transpile compile_commands.json -e --output-dir rust_code`
3. 得到一个可编译的 Rust crate（包含 `Cargo.toml`、`src/*.rs`）
4. 视需要用 `c2rust refactor` 批量清理/重构
5. 手工迁移剩余部分、替换 C 调用、优化代码