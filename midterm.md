# Week 8 中期报告 4.10

## 目前完成的工作

### Starry

- https://github.com/oscomp/starry-next/pull/22 初步模块化拆分 starry
- https://github.com/oscomp/starry-next/pull/34 基础进线程支持

### 为 arceos 的修复

- https://github.com/oscomp/arceos/pull/6 文件描述符表行为
- https://github.com/oscomp/arceos/pull/3 https://github.com/oscomp/arceos/pull/8 writev 行为
- https://github.com/oscomp/arceos/pull/4 https://github.com/oscomp/arceos/pull/11 修复地址空间克隆
- https://github.com/oscomp/arceos/pull/18 修复 LoongArch64 内核栈保存
- https://github.com/oscomp/arceos/pull/19 修复文件/文件夹打开行为
- https://github.com/oscomp/arceos/pull/21 no axstd
- https://github.com/oscomp/arceos/pull/29 改进 TrapFrame 和 UspaceContext
- https://github.com/oscomp/arceos/pull/30 改进 AxNamespace 允许线程间共享

### 为上游仓库的修复

- https://github.com/arceos-org/axio/pull/1 优化默认 read_to_end 实现
- https://github.com/arceos-org/flatten_objects/pull/2 提供变例方法
- https://github.com/arceos-org/page_table_multiarch/pull/17 修复页表查询

### 形成了一系列可复用的 crate

- https://github.com/Starry-OS/weak-map 弱引用 BTreeMap，自动清理过期引用
- https://github.com/Starry-OS/axprocess 抽象进程管理模块
- https://github.com/Starry-OS/axptr 用户访问权限检查

### 规范化类型

目前许多系统调用的参数类型是手动定义或通过手动编写的 c header 生成。目前我们找到了一个 [linux_raw_sys](https://docs.rs/linux-raw-sys/latest/linux_raw_sys/) crate，可以很好的解决这一问题。

## 后续计划

### lwext4 不支持并发

在多核测试中发现文件系统会发生异常行为，经检查发现是使用的 ext4 支持库的问题。目前找到了一个可能支持并发的分叉，接下来将尝试进行适配。

### 命名空间资源泄露

目前的 axns 设计没有考虑到回收，导致产生大量内存泄漏。计划重新设计这一模块。

### 规范化 POSIX API

- https://github.com/oscomp/starry-next/pull/18

原计划进行的 posix api 移植工作因为进程管理等事项搁置
