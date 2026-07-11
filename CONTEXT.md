# Token Proxy

Token Proxy 统一不同 AI Provider 的请求、响应、用量与计费语义，使 Dashboard 和成本统计使用同一套领域语言。

## Language

**Total Input**:
一次请求处理的全部输入 token，包括未缓存输入、缓存读取、缓存写入和图像输入。
_Avoid_: Prompt tokens（Provider 口径不一致）

**Cache Read**:
从既有提示缓存中复用的输入 token；只有 Cache Read 才构成缓存命中。
_Avoid_: Cached tokens（可能混入缓存写入）

**Cache Write**:
写入或创建提示缓存的输入 token，包括普通、5 分钟和 1 小时缓存写入；它属于输入，但不是缓存命中。
_Avoid_: Cache hit, cached input

**Cache Hit Rate**:
Cache Read 占 Total Input 的比例。
_Avoid_: Cache activity rate

**Usage Breakdown**:
将 Provider 原始用量拆成未缓存输入、Cache Read、各类 Cache Write、输出和图像 token 的规范化用量。
_Avoid_: Cached total
