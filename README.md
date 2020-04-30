# WASM It

Presentation material about adding WASM support to the Amethyst game engine.

* Online book: https://azriel.im/wasm_it/

## Building Locally

See below for OS specific commands.

1. Install [`mdbook`](https://github.com/rust-lang/mdBook).
2. Install [`mdbook-graphviz`](https://github.com/dylanowen/mdbook-graphviz).
3. Install [`graphviz`](https://www.graphviz.org/download/)
4. Run `mdbook serve`.
5. Open <http://localhost:3000>

### Linux

```bash
sudo apt install graphviz
cargo install mdbook
cargo install mdbook-graphviz
mdbook serve
```

### Mac / OS X

```bash
brew install graphviz
cargo install mdbook
cargo install mdbook-graphviz
mdbook serve
```
