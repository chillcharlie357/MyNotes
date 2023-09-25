# Goals of DFS
1. Performance
2. Scalability
3. Reliability
4. Availability

# Assumptions

1. 系统由大量廉价组件组成，经常失效
2. 存储大文件
3. 工作负载：大规模的流式读取&小规模的随机读取
4. 大量、顺序写入
5. 为多客户端实现良好定义的语义
6. 高带宽比低延迟更重要

## Interface

目录的形式组织
no-POSIX