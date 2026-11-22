{% raw %}
# Cargo のセキュリティ勧告 {#security-advisory-for-cargo}

- 原文の題名: Security advisory for Cargo
- 公開日: 2019-09-30
- 著者: The Rust Security Team
- 原文の URL: <https://blog.rust-lang.org/2019/09/30/Security-advisory-for-cargo/>

> **注**: これは[公式のセキュリティ勧告][official security advisory]の転載です。公式の投稿には、私たちの PGP 鍵で署名した版も含まれています。

Rust チームは先日、新しいバージョンの Cargo で追加されたパッケージのリネーム機能を使うクレートを、古いバージョンの Cargo でビルドする場合のセキュリティ上の懸念について通知を受けました。2018-05-10 にリリースされた Rust 1.26.0 以降を使っている場合は、影響を受けません。

この脆弱性の CVE は [CVE-2019-16760][0] です。

## 概要 {#overview}

Cargo では、`Cargo.toml` の `[dependencies]` セクションを通じて、crates.io にあるものなど、さまざまなクレートに依存するように設定できます。クレートへの依存の仕方を設定する方法も複数あります。たとえば、`serde` に依存して `derive` フィーチャーを有効にする場合は、次のようになります。

```toml
serde = { version = "1.0", features = ['derive'] }
```

Rust 1.31.0 では [Cargo の新機能が導入され][1]、このマップに指定できる任意のキーの 1 つとして `package` が加わりました。これは[クレートをローカルでリネームする][2]方法です。たとえば、ローカルでは `serde` ではなく `serde1` という名前を使いたい場合、次のように書けます。

```toml
serde1 = { version = "1.0", features = ['derive'], package = "serde" }
```

Cargo がクレートを異なる形でコンパイルする原因になるのが、この `package` キーの追加です。この機能は Rust 1.26.0 で[最初に実装されました][3]が、当時は不安定（unstable）でした。一方、Rust 1.25.0 以前では、Cargo は `package` キーを無視し、依存の行を次のように書かれているものとして解釈していました。

```toml
serde1 = { version = "1.0", features = ['derive'] }
```

つまり、Rust 1.25.0 以前でコンパイルすると、`serde1` クレートをダウンロードしようとします。悪意あるユーザーは、crates.io で `serde1` という名前を先に占拠し、`serde 1.0.0` のように見せかけながら、ビルド時に悪意ある動作をさせることができました。

まとめると、`Cargo.toml` で依存先をリネームするための `package` キーの使用は、Rust 1.25.0 以前では無視されます。Rust 1.25.0 以前を使うと、Cargo は `package` を無視して誤った依存先をダウンロードします。その名前は crates.io で占拠され、悪意あるパッケージにされているおそれがあります。これは、あなたが自分でローカルに書いたマニフェストだけでなく、crates.io に公開されたマニフェストにも影響します。たとえば、`serde1` に依存するクレートを crates.io に公開した場合、あなたのクレートに依存するユーザーも、Rust 1.25.0 以前を使っていれば脆弱になる可能性があります。

## 影響を受けるバージョン {#affected-versions}

Rust 1.0.0 から Rust 1.25.0 は、Cargo がマニフェストの `package` キーを無視するため、この勧告の影響を受けます。Rust 1.26.0 から Rust 1.30.0 は影響を受けず、`package` キーが不安定であるため、通常はエラーを出します。Rust 1.31.0 以降は、Cargo が `package` キーを理解するため、影響を受けません。

Cargo のバージョンで言えば、Cargo 0.26.0 までの Cargo が影響を受けます。それより後のすべてのバージョンの Cargo は影響を受けません。

## 緩和策 {#mitigations}

影響を受けるバージョンのユーザーには、コンパイラーを利用可能な最新のものに更新することを強くおすすめします。この問題が起きないようにするには、コンパイラーを Rust 1.26.0 以降に更新する必要があります。

1.26.0 より前の Rust バージョン向けのパッチリリースは行いません。Rust 1.19.0 から Rust 1.25.0 のユーザーは、代わりに[提供されているパッチ][4]を適用して、この問題を緩和できます。

`package` キーを使って crates.io に公開されている既存のクレートの監査を行いましたが、この脆弱性が実際に悪用されたという証拠はありません。私たちの監査が対象としているのは、現時点で crates.io に公開されているクレートだけです。今後、この脆弱性を悪用しているクレートに気づいた場合は、[私たちのセキュリティポリシー][5]に従って、遠慮なく security@rust-lang.org までメールでお知らせください。

## 出来事の時系列 {#timeline-of-events}

* 2019 年 9 月 18 日（水）13:54 UTC - バグが security@rust-lang.org に報告される
* 2019 年 9 月 18 日（水）15:35 UTC - 報告を確認した旨を返信
* 2019 年 9 月 18 日（水）- Cargo、Core、crates.io の各チームが、最善の対処方法を協議
* 2019 年 9 月 19 日（木）- Elichai 氏と対応方針を確認し、既存のクレートの監査を継続
* 2019 年 9 月 23 日（月）- 勧告の草案を作成、パッチを開発、監査を完了
* 2019 年 9 月 30 日（月）- 勧告を公開、セキュリティのメーリングリストにこの問題を通知

## 謝辞 {#acknowledgments}

このバグを見つけ、私たちの[セキュリティポリシー][5]に従って報告してくださった Elichai Turkel 氏に感謝します。

[0]: https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-16760
[1]: https://blog.rust-lang.org/2018/12/06/Rust-1.31-and-rust-2018.html#cargo-features
[2]: https://doc.rust-lang.org/cargo/reference/specifying-dependencies.html#renaming-dependencies-in-cargotoml
[3]: https://github.com/rust-lang/cargo/pull/4953
[4]: https://gist.github.com/pietroalbini/0d293b24a44babbeb6187e06eebd4992
[5]: https://www.rust-lang.org/policies/security
[official security advisory]: https://groups.google.com/forum/#!topic/rustlang-security-announcements/rVQ5e3TDnpQ
{% endraw %}
