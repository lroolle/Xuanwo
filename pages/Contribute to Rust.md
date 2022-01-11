- 必要的准备
	- Clone [[Rust]]
		- ```shell
		  git clone https://github.com/rust-lang/rust.git
		  cd rust
		  git submodule update --init --recursive
		  ```
	- 使用 `x.py` 配置环境
		- ```shell
		  ./x.py setup
		  ```
		- 这里会自动的进行一些交互式的配置，参考 [Create a config.toml](https://rustc-dev-guide.rust-lang.org/building/how-to-build-and-run.html#create-a-configtoml)
		- 如果主要是做一些 lib 相关的开发(比如说标准库)，可以选择 `library` 即可
- 常用命令
	- Build: `./x.py build`
	- Check: `./x.py check`
	- Test: `./x.py test`
- 常用技巧
	- Rust 测试量很大，最好只测试跟自己相关的
		- ```shell
		  # test a crate
		  ./x.py test --stage 1 library/alloc
		  # test a mod
		  ./x.py test --stage 1 library/alloc --test-args sync
		  ```
	- 通过 `--stage 0|1|2` 来指定不同的 build stage
		- 标准库开发比较常用 `--stage 1`
		- 比如 `./x.py build --stage 1`
- 常用链接
	- [Guide to Rustc Development](https://rustc-dev-guide.rust-lang.org/getting-started.html)
	- [Rust Forge](https://forge.rust-lang.org/)