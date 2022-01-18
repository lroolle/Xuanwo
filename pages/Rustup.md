- How it works
	- `~/.cargo/bin` 下面的 `cargo` 和 `rustc` 等组件实际上是一个 proxy，会把对应的命令转发给具体的 toolchain
	- 实现 https://github.com/rust-lang/rustup/blob/master/src/bin/rustup-init.rs#L1-L12
	- ```rust
	          Some(n) => {
	              if TOOLS.iter().chain(DUP_TOOLS.iter()).any(|&name| name == n) {
	                  proxy_mode::main(n)
	              } else {
	                  Err(anyhow!(format!(
	                      "unknown proxy name: '{}'; valid proxy names are {}",
	                      n,
	                      TOOLS
	                          .iter()
	                          .chain(DUP_TOOLS.iter())
	                          .map(|s| format!("'{}'", s))
	                          .collect::<Vec<_>>()
	                          .join(", ")
	                  )))
	              }
	          }
	  ```
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