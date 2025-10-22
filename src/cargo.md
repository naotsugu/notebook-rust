# Cargo

`cargo` は Rust のビルドツール。


## cargo コマンド

| コマンド                       | 説明 |
| ----------------------------- | ----- |
| `cargo --version`             | バージョン確認 |
| `cargo help <subcommand>`     | ヘルプ |
| `cargo new <myProject>`       | バイナリプロジェクト作成 |
| `cargo new --lib <myProject>` | ライブラリプロジェクト作成 |
| `cargo check`                 | コンパイルできるかを検証 |
| `cargo build`                 | ビルド |
| `cargo build --release`       | リリースビルド |
| `cargo run`                   | ビルド & 実行 |
| `cargo doc —open`             | ドキュメント生成してブラウザで開く |
| `cargo search <keyword>`      | crates.io からクレートを検索 |
| `cargo install <bin>`         | crates.io から `$HOME/.cargo/bin` にバイナリクレートをインストール |
| `cargo update`                | Cargo.toml に定義した依存でクレートのバージョンを更新 |



## Cargo.toml でのバージョン指定


セマンティック バージョニング(SemVer)で指定する

* `0.4` とするとバージョンは >=0.4.0 <0.5.0 の範囲のバージョン
* `0.4.6`(^0.4.6 の省略形)とすると、>=0.4.6 <0.5.0の範囲内のバージョン

初回ダウンロードしたバージョンは、プロジェクトフォルダにある `Cargo.lock` に反映され、次回以降は同バージョンが利用される

`cargo update` を実行すれば、公開されているクレートの新しいバージョンが反映される





