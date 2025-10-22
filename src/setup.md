
# Rust の導入

## Rust のインストール

`rustup` は Rust のツールチェーン管理用コマンド。


### rustup のインストール

```shell
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

rustc や catgo などの Rust 用ツールが `~/.cargo/bin` にインストールされる。


### アップデート

```shell
rustup update
```

rustup 自身と、ツールチェーンの最新版にアップデートされる。

rustup だけを更新する場合は `rustup self update` を利用する。



### アンインストール

```shell
run rustup self uninstall
```


## 各種ツールの導入


### Rust ソースコードのダウンロード

```shell
rustup component add rust-src
```

`<toolchain root>/lib/rustlib/src/rust` にソースがダウンロードされる。


### Rust のドキュメントを開く

```shell
$ rustup doc
```


### rustfmt

```shell
rustup component add rustfmt
```

```shell
cargo fmt
```


### クロスコンパイルターゲットの追加

デフォルトでは自身のプラットフォーム向けのツールチェーンのみインストールされる。

他のプラットフォーム向けのビルドが必要な場合はターゲットの追加が必要。

```shell
rustup target add x86_64-pc-windows-gnu
```

その他代表的なものは以下。

|プラットフォーム             | triple                    |
|---------------------------|---------------------------|
| Apple Silicon             | `aarch64-apple-darwin`    |
| 64-bit OSX (10.7+, Lion+) | `x86_64-apple-darwin`     |
| 64-bit Linux (2.6.18+)    | `x86_64-pc-windows-gnu`   |
| 64-bit MinGW (Windows 7+) | `x86_64-pc-windows-msvc`  |
| 64-bit MSVC (Windows 7+)  | `x86_64-unknown-linux-gnu`|


プラットフォームの一覧は以下で参照できる。

```shell
rustup target list
```

ツールチェーンを指定したビルドは `--target` で、導入したターゲットトリプルを指定する。

```shell
cargo build --target x86_64-pc-windows-gnu
```

不要になったターゲットは以下で削除できる。

```shell
rustup target remove x86_64-pc-windows-gnu
```

