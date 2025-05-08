# Week12 5.8

## 已合并

- tls 隔离([arceos#35](https://github.com/oscomp/arceos/pull/35) [starry-next#39](https://github.com/oscomp/starry-next/pull/39))

## 已提交 PR

- [arceos#38](https://github.com/oscomp/arceos/pull/38) 命名空间功能增强、修复内存泄漏问题
- [starry-next#44](https://github.com/oscomp/starry-next/pull/44) 测例解析命令行

## 进行中

- 把之前跑测例时开的 dev 分支代码变基回主线上来，其中有一部分功能如进程等已经提交了 PR 或已经合并，变基回主线减少代码不一致，方便后续开发
- 继续把一些完成的部分功能提交 PR，如上面提到的解析测例命令行
- 进一步解决未通过测例，如 `libctest` 中的 `rlimit_open_files` 等等，`pthread` 相关的测例可能要等到信号处理的部分搞完
