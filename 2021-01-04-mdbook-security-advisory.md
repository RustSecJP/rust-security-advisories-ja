{% raw %}
# mdBook のセキュリティ勧告 {#mdbook-security-advisory}

- 原文の題名: mdBook security advisory
- 公開日: 2021-01-04
- 著者: Rust Security Response WG
- 原文の URL: <https://blog.rust-lang.org/2021/01/04/mdbook-security-advisory/>

> これは[公式のセキュリティ勧告][ml]の転載です。公式の投稿には、私たちの PGP 鍵で署名した版も含まれています。

[ml]: https://groups.google.com/g/rustlang-security-announcements/c/3-sO6of29O0

Rust Security Response Working Group は先日、mdBook の検索機能に影響するセキュリティ上の問題について通知を受けました。この問題により、攻撃者はページ上で任意の JavaScript コードを実行できるおそれがあります。

この脆弱性の CVE は [CVE-2020-26297][1] です。

## 概要 {#overview}

mdBook の検索機能（バージョン 0.1.4 で導入されました）は、クロスサイトスクリプティング（cross site scripting）の脆弱性の影響を受けていました。攻撃者は、ユーザーをだまして悪意ある検索クエリを入力させるか、悪意ある検索クエリがあらかじめ入力された検索ページへのリンクをクリックさせることで、ユーザーのブラウザー上で任意の JavaScript コードを実行できました。

mdBook 0.4.5 は、検索クエリを適切にエスケープすることで、この脆弱性を修正しています。

## 緩和策 {#mitigations}

mdBook で構築したウェブサイトの所有者は、mdBook 0.4.5 以降に更新し、それを使ってウェブサイトのコンテンツを再ビルドする必要があります。次のコマンドで、ローカルのシステムに mdBook 0.4.5 をインストールできます。

```
cargo install mdbook --version 0.4.5 --force
```

## 謝辞 {#acknowledgements}

[私たちのセキュリティポリシー][2]に従って、責任ある形でこの脆弱性を開示してくださった Kamil Vavra 氏に感謝します。

## 出来事の時系列 {#timeline-of-events}

時刻はすべて UTC です。

* 2020-12-30 20:14 - 問題が Rust Security Response WG に報告される
* 2020-12-30 20:32 - 問題の受領を確認し、調査を開始した
* 2020-12-30 21:21 - 脆弱性の原因を突き止め、パッチを用意した
* 2021-01-04 15:00 - パッチ済みのバージョンをリリースし、脆弱性を開示した

[1]: https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-26297
[2]: https://www.rust-lang.org/policies/security
{% endraw %}
