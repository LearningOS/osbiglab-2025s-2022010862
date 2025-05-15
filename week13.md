# Week13 5.15

## Done

### ArceOS

- [命名空间](https://github.com/oscomp/arceos/pull/38)暂时搁置，考虑做成更通用的机制+和任务上下文切换结合在一起
- [重启 IRQ](https://github.com/oscomp/arceos/pull/38) 进行了一些审阅和调试工作 但是目前 @Mivik 测试出来还是有问题 应该在恢复上下文的时候再次禁用 IRQ

### starry-next

- [合并 arceos_posix_api](https://github.com/oscomp/starry-next/pull/47) 包括了我们分支主要的代码并修复了若干问题
- [修复 execve 内存泄漏问题](https://github.com/oscomp/starry-next/pull/51)

### 测例

- 基础测例：
  - riscv64 上编译的 abi （lp64）与提供的 libc （lp64d）不一致
  - musl 版本实际上是用 gnu 工具链编译出来的
  - 修完上面的问题之后又发现 loongarch64-linux-musl- 默认没开 PIE 导致无法运行
- libctest：动态库加载路径不对

## Pending

- 初始化脚本 可能要同步修改 ci 测试方法
- 逐渐暴露出文件系统越来越多缺陷，一同改进 axfs
