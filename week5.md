# Week5 3.20

## Done

因为把 arceos 变基到了上游最新合并的 LoongArch64 支持，所以一开始在修变基之后的遗留问题：
- [arceos#17](https://github.com/oscomp/arceos/pull/17) 一处更改在变基时意外丢失
- [arceos#18](https://github.com/oscomp/arceos/pull/18) 修复了 LoongArch64 上异常处理程序未保存内核栈地址，疑似上游问题，已反馈给维护者

其他的工作也主要聚焦在 arceos：
- [arceos#19](https://github.com/oscomp/arceos/pull/19) 修复了一个很多同学都遇到了但是迟迟没修复的文件夹打开的问题
- [arceos#21](https://github.com/oscomp/arceos/pull/21) 允许下游应用不依赖 `axstd` 构建，类似于 Rust 的 `no_std`

## WIP

- [starry-next#16](https://github.com/oscomp/starry-next/issues/16) [starry-next#18](https://github.com/oscomp/starry-next/pull/18) 尝试解决 `starry-next` 与 `arceos_posix_api` 的冲突
