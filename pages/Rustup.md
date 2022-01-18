- toolchain file
	- ```toml
	  [toolchain]
	  channel = "nightly-2020-07-10"
	  components = [ "rustfmt", "rustc-dev" ]
	  targets = [ "wasm32-unknown-unknown", "thumbv2-none-eabi" ]
	  profile = "minimal"
	  ```
	- 可以用 `rust-toolchain.toml` 来指定一组特定的 rust 版本
	- `rustc`，`cargo` 这些内置命令执行的时候，rustup 都会首先选择使用哪个 toolchain
		- 选择顺序参考 [Overrides](https://rust-lang.github.io/rustup/overrides.html#overrides)
	- 所以在 CI 等环境里面，实际上不需要做额外的 setup
- 参考资料
	- [Rustup Introduction](https://rust-lang.github.io/rustup/index.html#introduction)