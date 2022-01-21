- Rust 中的大部分类型都可以安全的 move，标准库默认为他们实现了 `Unpin`，只有一种情况例外：自引用类型。
- 最常见的自引用类型是 [[Rust/std Future]]
	- ```rust
	  pub trait Future {
	      type Output;
	      fn poll(self: Pin<&mut Self>, cx: &mut Context<'_>) -> Poll<Self::Output>;
	  }
	  ```
-
- 为什么需要 Pin ？
	- ```rust
	  #[derive(Debug)]
	  struct Test {
	      a: String,
	      b: *const String,
	  }
	  
	  impl Test {
	      fn new(txt: &str) -> Self {
	          Test {
	              a: String::from(txt),
	              b: std::ptr::null(),
	          }
	      }
	  
	      fn init(&mut self) {
	          let self_ref: *const String = &self.a;
	          self.b = self_ref;
	      }
	  
	      fn a(&self) -> &str {
	          &self.a
	      }
	  
	      fn b(&self) -> &String {
	          assert!(!self.b.is_null(), "Test::b called without Test::init being called first");
	          unsafe { &*(self.b) }
	      }
	  }
	  
	  ```
	- 在这个例子当中，`self.b` 指向了 `self.a`。当 Test 发生 move 时，self.b 还在指向原先的地址：
	- ![image.png](../assets/image_1642055060917_0.png)
	- 当然，创建自引用类型本就是 unsafe 操作，开发者和使用者有必要详细的记录内存行为，开发者有权要求使用者在 move 之后手动更新 `self.b` 的地址。但是这太不好用了，而且非常容易出问题。
	- 于是 `Pin` 诞生了，它的作用是保证指针类型背后的值不会被 move。编译器会在编译期对 `Pin<T>` 做检查，如果发生了 move，就会给出一个编译错误，从而强迫开发者写出正确的代码。
-
- 常用的技巧
	- `Bin::pin()`
	- `futures::pin_mut`
		- Pins a value on the stack.
		- ```rust
		  #[macro_export]
		  macro_rules! pin_mut {
		      ($($x:ident),* $(,)?) => { $(
		          // Move the value to ensure that it is owned
		          let mut $x = $x;
		          // Shadow the original binding so that it can't be directly accessed
		          // ever again.
		          #[allow(unused_mut)]
		          let mut $x = unsafe {
		              $crate::core_reexport::pin::Pin::new_unchecked(&mut $x)
		          };
		      )* }
		  }
		  ```
-
- What is Pin?
	- > Pin exists to solve a very specific problem: self-referential datatypes, i.e. data structures which have pointers into themselves.
	- > The Pin type wraps pointer types, guaranteeing that the values behind the pointer won't be moved. For example, Pin<&mut T>, Pin<&T>, Pin<Box<T>> all guarantee that T won't be moved even if T: !Unpin.
- 参考资料
	- [Rust Async Book: Pinning](https://rust-lang.github.io/async-book/04_pinning/01_chapter.html)
	- [Pin, Unpin, and why Rust needs them](https://blog.cloudflare.com/pin-and-unpin-in-rust/)
	-