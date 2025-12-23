# Cargo

`cargo` は Rust のビルドツール


## cargo コマンド


| コマンド | 説明 |
| --- | --- |
| `cargo --version`             | バージョン確認 |
| `cargo help <subcommand>`     | ヘルプ |
| `cargo new <projectname>` | 新しいプロジェクトを作成 |
| `cargo init` | 既存のディレクトリをプロジェクトとして初期化 |
| `cargo check` | コードをコンパイルせずに、エラーや警告がないかチェック(ビルドより高速) |
| `cargo build` | 現在のプロジェクトをコンパイル(デフォルトはデバッグビルドで `--release` でリリースビルド) |
| `cargo run` | プロジェクトをコンパイルし、生成された実行可能ファイルを実行 |
| `cargo doc —open` | プロジェクトとその依存関係のドキュメントを生成し、ブラウザで開く |
| `cargo test` | プロジェクト内のテスト(単体テスト、統合テスト)を実行 |
| `cargo clean` | ビルド成果物(target ディレクトリ)を削除し、プロジェクトをクリーンな状態に戻す |
| `cargo update` | Cargo.toml の指定に基づいて、依存関係を最新バージョンに更新し Cargo.lock を再生成 |
| `cargo install` | クレートをローカル環境にインストールし、バイナリ（コマンド）として使えるようにする |
| `cargo uninstall` | cargo install でインストールしたクレートをアンインストール |
| `cargo publish` | パッケージ(クレート)を crates.io （Rustの公式レジストリ）に公開 |
| `cargo search` | crates.io でクレートを検索 |
| `cargo add` | Cargo.toml ファイルに新しい依存関係(クレート)を簡単に追加 |
| `cargo remove` | Cargo.toml ファイルから依存関係を削除 |
| `cargo fmt` | rustfmt を使って、プロジェクトのコードを整形 |
| `cargo clippy` | clippy(リンター)を使って、コードの潜在的な問題や改善点をチェック |
| `cargo bench` | プロジェクトのベンチマークを実行 |
| `cargo tree` | プロジェクトの依存関係ツリーを表示 |



## プロジェクト作成

`cargo new` または `cargo init` を利用する


```shell
cargo new hello_world
cd hello_world
```

- `--lib` ライブラリクレート用のプロジェクトとなり src/lib.rs が生成される
- `--vcs none git` リポジトリの生成が不要な場合


既存ディレクトリに作成する場合は `cargo init`

```shell
mkdir hello_world
cd hello_world
cargo init
```

以下のような構成となる

```
hello_world/
 ├── .git/
 ├── .gitignore
 ├── Cargo.toml
 └── src/
     └── main.rs
```

通常のソース・ファイルの構成は以下のようになる

```
 ├── Cargo.toml
 ├── src/
 │   ├── lib.rs
 │   ├── main.rs
 │   └── bin/
 │       ├── named-executable.rs
 │       ├── another-executable.rs
 │       └── multi-file-executable/
 │           ├── main.rs
 │           └── some_module.rs
 ├── benches/
 │   ├── large-input.rs
 │   └── multi-file-bench/
 │       ├── main.rs
 │       └── bench_module.rs
 ├── examples/
 │   ├── simple.rs
 │   └── multi-file-example/
 │       ├── main.rs
 │       └── ex_module.rs
 └── tests/
     ├── some-integration-tests.rs
     └── multi-file-test/
         ├── main.rs
         └── test_module.rs
```

## ビルド

```shell
cargo build
```
実行ファイルは `target/debug` に生成される

リリース向けビルドは `--release` を付与

```shell
cargo build --release
```

リリース向けの実行ファイルが `target/release` に生成される


`-timings` でクレートのビルド時間を確認可能

```shell
cargo build -timings
```


プロジェクトのビルドと実行を1ステップで行うには `run`

```shell
cargo run
```


## テスト

`check` は、実行ファイルは生成せずに、コードがコンパイルできるかをチェックする

```shell
cargo check
```

`test` でユニットテストとパッケージのインテグレーションテストを実行

```shell
cargo test
```

テストのオプションは以下で確認できる

```shell
cargo test -- --help
```

以下のようなテストがあった場合、

```rust
#[cfg(test)]
mod tests {

  #[test]
  fn add_two_and_two() { ... }
    
  #[test]
  fn add_three_and_two() { ... }

  #[test]
  fn one_hundred() { ... }
}
```

関数名を指定して特定のテストだけを実行できる

```shell
cargo test one_hundred
```

以下のようにすると、add から始まる2つのテストが実行される

```shell
cargo test add
```

## ビルドプロファイル

|コマンドライン|Cargo.tomlセクション|
|---|---|
|`cargo build`|`[profile.dev]`|
|`cargo build --release `|`[profile.release]`|
|`cargo test`|`[profile.test]`|



## クレート

`search` は crates.io からパッケージを検索。例えば serde を検索時は以下

```shell
cargo search serde
```

`add` は `Cargo.toml` に依存を追加する(Cargo.toml を直接修正しても良い)

例えば `regex` を追加時は以下

```shell
cargo add regex
```

削除時は以下

```shell
cargo remove regex
```

Cargo.toml でのバージョン指定はセマンティック バージョニング(SemVer)で指定する

* `0.4` とするとバージョンは `>=0.4.0 <0.5.0` の範囲のバージョン
* `0.4.6`(`^0.4.6` の省略形)とすると、`>=0.4.6 <0.5.0` の範囲内のバージョン

初回ダウンロードしたバージョンは、プロジェクトフォルダにある `Cargo.lock` に反映され、次回以降は同バージョンが利用される


`update` は `Cargo.lock` に記録されたバージョンを更新(公開されているクレートの新しいバージョンが反映される)

```shell
cargo update
```

`tree` は依存グラフを表示

```shell
cargo tree
```

`publish` は crates.io にパッケージを公開する

`crates.io` のアカウントを作成し、 `https://crates.io/me/` でAPIトークンを取得し、APIキーを以下で保存する

```shell
cargo login <APIキー>
```

これによりAPIトークンが `~/.cargo/credentials` に保存される。これを削除するには `cargo logout` を実行

`Cargo.toml` に必要なメタデータを定義後、以下でアップロード公開

```shell
cargo publish
```


## その他


`fix` はコンパイラ警告を自動修正する

```shell
cargo fix
```


`doc` はパッケージのドキュメントを生成

```shell
cargo doc --open
```

ドキュメントは `target/doc` に生成される。オプション --open でブラウザ起動

`clean` は生成された成果物を削除

```shell
cargo clean
```


`install` / `uninstall` はバイナリクレートをローカルにインストールする(他人が crates.io に共有したツールを利用する便利な方法を提供する)

例えば、ripgrep ツールをインストールするには以下

```shell
cargo install ripgrep
```

`$HOME/.cargo/bin` にインストールされる

アンインストールするには以下

```shell
cargo uninstall ripgrep
```

cargo では、`$PATH` にあるバイナリが `cargo-something` という名前の場合、`cargo something` として cargo のサブコマンドであるかのように実行できる

つまり、`cargo install` を使用して拡張をインストールし、組み込みの cargo コマンドのように実行できる

このような独自のコマンドは、 `cargo --list` で列挙できる。


