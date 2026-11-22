{% raw %}
# セキュリティ勧告: 悪意あるクレート rustdecimal {#security-advisory-malicious-crate-rustdecimal}

- 原文の題名: Security advisory: malicious crate rustdecimal
- 公開日: 2022-05-10
- 著者: The Rust Security Response WG
- 原文の URL: <https://blog.rust-lang.org/2022/05/10/malicious-crate-rustdecimal/>

> これは[公式のセキュリティ勧告][advisory]の転載です。公式の勧告には、私たちの PGP 鍵で署名した版も含まれています。

[advisory]: https://groups.google.com/g/rustlang-security-announcements/c/5DVtC8pgJLw

Rust Security Response WG と crates.io チームは 2022-05-02、マルウェアを含む悪意あるクレート `rustdecimal` の存在について[通知を受けました][1]。このクレートの名前は、人気のある [`rust_decimal`][2] クレートの名前に意図的に似せてあり、被害者になりうる人が名前を打ち間違えることを狙ったものでした（「タイポスクワッティング（typosquatting）」と呼ばれる攻撃です）。

エコシステムの安全を守るため、crates.io チームはマルウェアの存在を知るとすぐに、このクレートをレジストリから恒久的に削除しました。crates.io 上のすべてのクレートの分析も行いましたが、同様のコードのパターンを持つクレートはほかに見つかりませんでした。

[`rust_decimal`][2] クレートは侵害されて**おらず**、引き続き安全に使えることに留意してください。

## クレートの分析 {#analysis-of-the-crate}

このクレートは、2022-03-25 の最初のリリース以来のダウンロード数が 500 未満で、crates.io レジストリ上にこれに依存するクレートはありませんでした。

このクレートは、`Decimal::new` 関数を除いて、正規の `rust_decimal` クレートと同一のソースコードと機能を含んでいました。

この関数が呼ばれると、環境変数 `GITLAB_CI` が設定されているかどうかを調べ、設定されていれば、バイナリのペイロードを `/tmp/git-updater.bin` にダウンロードして実行しました。バイナリのペイロードは Linux と macOS の両方に対応していましたが、Windows には対応していませんでした。

分析を行った時点でダウンロード URL がすでに機能しなくなっていたため、バイナリのペイロードの分析はできませんでした。

## 推奨事項 {#recommendations}

あなたのプロジェクトや組織が GitLab CI を動かしている場合は、2022-03-25 以降に、プロジェクトまたはその依存先のいずれかが `rustdecimal` クレートに依存していなかったかを確認することを強くおすすめします。このクレートへの依存に気づいた場合は、CI 環境が侵害されたものと考えるべきです。

一般的な対策として、依存先を定期的に監査すること、そして作者を信頼できるクレートだけに依存することをおすすめします。クレートのソースコードに不審な挙動を見つけた場合は、[Rust のセキュリティポリシー][3]に従って、Rust Security Response WG に報告してください。

## 謝辞 {#acknowledgements}

[この GitHub issue][1] で悪意あるクレートを特定してくださった、GitHub ユーザーの [`@safinaskar`][4] 氏に感謝します。

[1]: https://github.com/paupino/rust-decimal/issues/514#issuecomment-1115408888
[2]: https://crates.io/crates/rust_decimal
[3]: https://www.rust-lang.org/policies/security
[4]: https://github.com/safinaskar
{% endraw %}
