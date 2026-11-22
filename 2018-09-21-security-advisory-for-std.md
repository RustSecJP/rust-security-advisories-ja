{% raw %}
# 標準ライブラリのセキュリティ勧告 {#security-advisory-for-the-standard-library}

- 原文の題名: Security advisory for the standard library
- 公開日: 2018-09-21
- 著者: The Rust Core Team
- 原文の URL: <https://blog.rust-lang.org/2018/09/21/Security-advisory-for-std/>

Rust チームは先日、標準ライブラリの `str::repeat` 関数に影響するセキュリティ脆弱性について通知を受けました。この関数は、大きな数を渡されると整数オーバーフローを起こし、境界外書き込み（out of bounds write）につながるおそれがあります。`str::repeat` を使っていなければ、影響はありません。

現在、この脆弱性の CVE 番号を申請しているところです。この問題の修正は、Rust リポジトリの stable / beta / master の各ブランチに取り込まれました。修正を含む nightly と beta は今夜作成され、stable の Rust 向けの修正を含む 1.29.1 は 2018-09-25 にリリースされます。

発表の全文は、私たちの rustlang-security-announcements メーリングリストの[こちら][here]で読めます。

[here]: https://groups.google.com/forum/#!topic/rustlang-security-announcements/CmSuTm-SaU0
{% endraw %}
