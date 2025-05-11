---
title: rpcx vs gRPC vs Thrift
date: 2025-05-11 19:39:07
tags: rpc
categories: 架构
---
# 技术栈与生态系统
- gRPC
    - 语言支持：官方支持 C++、Java、Python、Go、Node.js 等多种语言，生态成熟，适合跨语言微服务。
    - 协议：基于 HTTP/2，支持多路复用、二进制分帧、头部压缩等特性，性能优越。
    - 序列化：默认使用 Protocol Buffers（Protobuf），高效且强类型。
- Thrift
    - 语言支持：支持 C++、Java、Python、Ruby 等，跨语言能力强，但部分语言的社区支持较弱。
    - 协议：支持多种传输协议（如 TCP、HTTP）和序列化方式（二进制、JSON、压缩格式），灵活性高。
    - 生态：由 Apache 维护，历史悠久，但活跃度低于 gRPC。
- rpcx
    - 语言支持：仅支持 Go 语言，专注于 Go 生态，适合纯 Go 项目。
    - 协议：基于 TCP，支持自定义协议（如 JSON、Protobuf、MessagePack），性能优化针对 Go 语言特性。
    - 生态：Go 社区活跃，提供服务发现、负载均衡、断路器等微服务组件。
# 性能对比
- gRPC
    - 基于 HTTP/2 的二进制协议，在长连接、高并发场景下表现优异。
    - Protobuf 序列化效率高，适合低延迟、高吞吐量的场景。
- Thrift
    - 二进制协议性能接近 gRPC，但灵活性更高（支持多种协议）。
    - 在 Go 语言中，Thrift 的长连接性能优于 gRPC（参考 CSDN 测试数据）。
- rpcx
    - 专为 Go 设计，避免跨语言开销，性能通常优于 gRPC 和 Thrift（尤其是在 Go 项目中）。
    - 测试数据显示，rpcx 在 Go 生态中的 QPS（每秒请求数）比 gRPC 高 20-50%。
# 功能特性
|特性|gRPC|Thrift|rpcx|
|---|---|---|---|
|服务发现|依赖第三方（如 Consul、Etcd）|依赖第三方（如 ZooKeeper）|内置支持 Etcd、Consul、ZooKeeper 等|
|负载均衡|内置简单负载均衡|需自定义或依赖第三方|内置多种负载均衡策略|
|流式传输|支持双向流|部分协议支持（如 HTTP/2）|支持请求 / 响应流|
|断路器|需第三方库（如 Hystrix-go）|需自定义|内置断路器|
|协议扩展性|主要依赖 Protobuf|支持多种协议和序列化方式|支持自定义协议（如 Protobuf、JSON）|
# 适用场景
- gRPC
    - 跨语言微服务架构，尤其是 Google Cloud、Kubernetes 生态。
    - 对性能和实时性要求高的场景（如金融、物联网）。
- Thrift
    - 需要兼容多种协议和序列化方式的遗留系统。
    - 跨语言场景，但对性能要求不如 gRPC 苛刻。
- rpcx
    - 纯 Go 语言的微服务架构，追求极致性能和简单集成。
    - 快速迭代的项目，依赖 Go 生态的工具链（如 Go Modules）。
# 社区与维护
- gRPC：Google 主导，开源社区活跃，更新频繁。
- Thrift：Apache 项目，维护稳定但创新较少。
- rpcx：Go 社区项目，更新快，适合快速变化的需求。
# 总结
- 选 gRPC：如果需要跨语言支持、HTTP/2 优势（如多路复用），且愿意学习 Protobuf。
- 选 Thrift：如果需要灵活性（多协议支持）和对遗留系统的兼容性。
- 选 rpcx：如果项目是纯 Go 语言，追求极致性能和简单配置。
