# Week10 4.24

## 已完成

- [arceos#33](https://github.com/oscomp/arceos/pull/33) 使用 [lock_api](https://docs.rs/lock_api) 实现 Mutex 抽象，解决了之前提到的模块依赖问题（给 axsignal 用）并支持更多方法（已合并）
- [arceos#35](https://github.com/oscomp/arceos/pull/35) 实现了完整的内核态与用户态 tls 隔离，现在终于可以正确地解决 tls 的问题和实现 CLONE_SETTLS，并且还带来了一个额外收益是可以在内核中编写 `#[thread_local]` 代码了
- [axns](https://github.com/Starry-OS/axns) 完成了新的命名空间模块的设计，但是在“获取当前命名空间”的接口上不是很满意，见下文

## WIP

`#[extern_trait]`

```rust
/// Types that can be passed through CPU registers according to platform ABI rules.
///
/// This marker trait indicates that a type's representation conforms to the
/// register-passing requirements of the target platform's ABI. It is required
/// for type-erased function calls where values must be transferred solely
/// through registers without stack allocation.
///
/// # Safety
///
/// Implementors must ensure all of these conditions on 64-bit architectures:
/// - As a return value or parameter: <= 16 bytes
/// - As a non-final parameter: <= 8 bytes
/// - Contains no implicit padding bytes
/// - Contains no floating-point types unless using soft-float ABI
///
/// On 32-bit architectures, similar rules may apply, but on x86, all arguments
/// are passed on the stack so this trait is not applicable.
///
/// Implementors must verify compliance with the specific ABI requirements of
/// each target platform. Violations may cause Undefined Behavior.
pub unsafe trait CompactABI {}
```

定义 trait：
```rust
#[extern_trait]
pub trait Foo {
    fn new() -> Self;
    fn foo(&self) -> usize;
}
```

生成类似这样的代码：
```rust
#[allow(unused)]
#[repr(transparent)]
struct FooImpl([*const (); 2]);

impl Foo for FooImpl {
    fn new() -> Self {
        unsafe extern "Rust" {
            safe fn __Foo_new() -> FooImpl;
        }
        __Foo_new()
    }

    fn foo(&self) -> usize {
        unsafe extern "Rust" {
            safe fn __Foo_foo(this: *const FooImpl) -> usize;
        }
        __Foo_foo(self)
    }
}

impl Drop for FooImpl {
    fn drop(&mut self) {
        unsafe extern "Rust" {
            safe fn __Foo_drop(this: *mut FooImpl);
        }
        __Foo_drop(self)
    }
}
```

实现者：
```rust
pub struct Bar(usize);

unsafe impl CompactABI for Bar {}

#[extern_trait]
impl Foo for Bar {
    fn new() -> Self {
        Bar(42)
    }

    fn foo(&self) -> usize {
        self.0
    }
}

impl Drop for Bar {
    fn drop(&mut self) {
        println!("Bar dropped {:x}", self.0);
    }
}
```

生成类似这样的代码：
```rust
#[allow(non_snake_case)]
const _: () = {
    #[unsafe(no_mangle)]
    fn __Foo_new() -> Bar {
        <Bar as Foo>::new()
    }
    #[unsafe(no_mangle)]
    fn __Foo_foo(v: &Bar) -> usize {
        <Bar as Foo>::foo(v)
    }
    #[unsafe(no_mangle)]
    fn __Foo_drop(v: &mut Bar) {
        unsafe { ::core::ptr::drop_in_place(v) };
    }
};
```
