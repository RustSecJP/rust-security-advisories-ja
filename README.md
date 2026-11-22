# Rust 公式ブログのセキュリティ勧告（Rust Blog 日本語訳）

> **これは Rust 公式ブログ（[blog.rust-lang.org](https://blog.rust-lang.org/)）に掲載されたセキュリティ勧告と、crates.io の事件・悪意あるクレートの報告記事の、非公式な日本語訳です。** Rust Project や各記事の著者による確認や公認を受けたものではありません。
>
> **読むのはこちら: https://rustsecjp.github.io/rust-security-advisories-ja/**
>
> This is an unofficial Japanese translation of the security advisories and crates.io incident reports published on the Rust Blog.

rustc、標準ライブラリ、Cargo、rustdoc、mdBook、regex クレートの脆弱性（CVE）の勧告、crates.io で起きたセキュリティ事件の報告、悪意あるクレートの報告、サプライチェーン攻撃や標的型攻撃への注意喚起を、2018 年から 2026 年まで公開日の古い順に全 29 本収めています。リリース告知や一般の記事は含めていません。

各記事は公開された時点の状況を述べたものです。影響を受けるバージョンや対処の方法を確かめるときは、必ず原文と最新の公式情報を確かめてください。

原文のリポジトリの README はブログの開発者向けの説明なので、この README は日本語訳の案内だけを載せています。

## 訳した版

| | |
|---|---|
| 原文の版 | 2026-11-20 時点（コミット [`8666f7c`](https://github.com/rust-lang/blog.rust-lang.org/commit/8666f7c3a86260ec58e4d914d451b14613c690a4)） |
| 翻訳日 | 2026-11-22 |

原文がこの版より後に更新されている場合、訳は古い内容のままです。この版より後に公開された記事は含まれていません。

## 出典とライセンス

- 原文: https://blog.rust-lang.org/ （ソース: [rust-lang/blog.rust-lang.org](https://github.com/rust-lang/blog.rust-lang.org) の `content/` 以下。`content/inside-rust/` の 1 本を含む）
- 原著者: 各記事の著者・チーム（The Rust Project）。各ページの冒頭に、原文の題名・公開日・著者・原文の URL を示しています。
- ライセンス: MIT または Apache-2.0。全文は [LICENSE-MIT.txt](LICENSE-MIT.txt) と [LICENSE-APACHE.txt](LICENSE-APACHE.txt)。
- 訳した版: 2026-11-20 時点（コミット 8666f7c3a86260ec58e4d914d451b14613c690a4）
- 変更点: 日本語への翻訳。前付け（題名・公開日・著者）を本文冒頭の箇条書きに変換。ファイル名に公開日を付与。記事どうしのリンクをこの訳のページに向け直し。見出しにリンク先の目印を付与。表示用の書式の調整。

出典の詳細は [NOTICE.md](NOTICE.md) にあります。

誤訳の指摘や訳語の提案は、Issue か Pull Request でお願いします。

RustSecJP は非公式のコミュニティです。Rust Project、Rust Foundation、crates.io、RustSec とは提携していません。
