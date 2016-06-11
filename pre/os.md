OS 复习笔记

OS Definition：
- 提供虚拟机抽象处理不同设备
- 整合资源，用户隔离
- 提供标准服务接口简化应用开发
- 容错性、错误恢复

SMT:
```
Simultaneous multithreading is a processor design that combines hardware multithreading with superscalar processor technology to allow multiple threads to issue instructions each cycle. Unlike other hardware multithreaded architectures (such as the Tera MTA), in which only a single hardware context (i.e., thread) is active on any given cycle, SMT permits all thread contexts to simultaneously compete for and share processor resources. Unlike conventional superscalar processors, which suffer from a lack of per-thread instruction-level parallelism, simultaneous multithreading uses multiple threads to compensate for low single-thread ILP. The performance consequence is significantly higher instruction throughput and program speedups on a variety of workloads that include commercial databases, web servers and scientific applications in both multiprogrammed and parallel environments.

```