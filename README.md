# Zero 2 Production

## Inner Development Loop
The inner development loop consists of:
1. Make a change
2. Compile the application
3. Run tests
4. Run the application

The speed of this loop determines how many iterations can be completed in a given time. A significant portion of the Rust build time is spent during the linking phase. To optimize this, we use [lld](https://lld.llvm.org/), a faster linker developed by the LLVM project.

### Optimizing the Linking Phase
To speed up the linking phase, you must install the alternative linker (`lld`) on your machine.

#### Windows
```sh
cargo install -f cargo-binutils
rustup component add llvm-tools-preview
```

#### Ubuntu
```sh
sudo apt-get install lld clang
```

#### Arch Linux
```sh
sudo pacman -S lld clang
```

#### macOS
```sh
brew install llvm
echo 'export PATH="/opt/homebrew/opt/llvm/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
brew install lld
```
After installing lld with brew, you must brew install the missing dependencies listed in `brew info llvm`

### Reducing Perceived Compilation Time
Even with an optimized linker, waiting for `cargo check` or `cargo run` can feel slow. We can mitigate this by reducing perceived compilation time using `cargo-watch`.

#### Installing Cargo Watch
```sh
cargo install cargo-watch
```

#### Using Cargo Watch
`cargo-watch` monitors source code changes and automatically triggers commands when files change. This allows compilation to start in the background while you continue working.

For example:
```sh
cargo watch -x check
```
This runs `cargo check` after every code change, keeping the compiler ahead while you review your modifications.

You can also chain commands:
```sh
cargo watch -x check -x test -x run
```

This executes:
1. `cargo check`
2. If successful, `cargo test`
3. If tests pass, `cargo run`

That's our inner development loop!


## Future Improvements
This README will be expanded as the project evolves.