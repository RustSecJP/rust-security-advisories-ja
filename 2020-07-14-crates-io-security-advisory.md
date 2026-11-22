{% raw %}
# crates.io のセキュリティ勧告 {#cratesio-security-advisory}

- 原文の題名: crates.io security advisory
- 公開日: 2020-07-14
- 著者: Rust Security Response WG
- 原文の URL: <https://blog.rust-lang.org/2020/07/14/crates-io-security-advisory/>

これは[公式のセキュリティ勧告][ml]の転載です。公式の投稿には、私たちの PGP 鍵で署名した版も含まれています。

---

Rust Security Response Working Group は先日、[crates.io] のウェブアプリケーションにおけるトークン生成に影響するセキュリティ上の問題について通知を受けました。そして、その問題を調査する中で、crates.io の API トークンに影響する別の脆弱性も見つけました。

これが実際に悪用されたという証拠はありませんが、念には念を入れて、既存の API キーをすべて失効させることにしました。新しい API キーは [crates.io/me] で生成できます。

## 概要 {#overview}

最近まで、[crates.io] の API キーは PostgreSQL の random 関数を使って生成されていました。この関数は、暗号論的に安全な乱数生成器ではありません。つまり理論上は、攻撃者が十分な数の乱数値を観測すれば乱数生成器の内部状態を特定でき、その情報を使って、データベースサーバーが最後に再起動された時点までさかのぼって、過去に作られた API キーを割り出せることになります。

この調査の一環として、API キーが平文で保存されていたことも分かりました。これは、もし私たちのデータベースが何らかの形で侵害された場合、攻撃者が現在有効なすべてのトークンで API にアクセスできてしまうことを意味します。

## 緩和策 {#mitigations}

暗号論的に安全な乱数生成器を使うようにするコード変更を本番環境にデプロイし、データベースにトークンを保存する際のハッシュ化を実装しました。

どちらの問題も、実際に悪用するのは極めて非現実的であり、実際に悪用されたという証拠も見つかっていません。それでも念には念を入れて、既存の API キーをすべて失効させることにしました。新しい API キーは [crates.io/me] から生成できます。ご不便をおかけすることをお詫びします。

## 謝辞 {#acknowledgements}

[私たちのセキュリティポリシー][policy]に従って、乱数生成器の問題を責任ある形で開示してくださった [Jacob Hoffman-Andrews] 氏に感謝します。両方の問題への対処で Security Response WG を助けてくれた、crates.io チームの [Siân Griffin] と [Justin Geibel] に感謝します。この脆弱性への対応を調整した、Security Response WG の [Pietro Albini] に感謝します。

## 出来事の時系列 {#timeline-of-events}

時刻はすべて UTC です。

- 2020-07-11 17:43 - 問題が [security@rust-lang.org] に報告される
- 2020-07-11 20:56 - 問題の受領を確認し、crates.io チームのリードに連絡する
- 2020-07-11 23:48 - 問題を確認し、修正の方針に合意する
- 2020-07-13 08:00 - 修正の開発を開始する
- 2020-07-14 12:53 - ステージング環境で修正をテストする
- 2020-07-14 19:03 - 修正をデプロイし、既存のトークンを失効させ、問題を公に開示する

[ml]: https://groups.google.com/forum/?oldui=1#!topic/rustlang-security-announcements/wc5d_Qq35RA
[policy]: https://www.rust-lang.org/policies/security
[security@rust-lang.org]: mailto:security@rust-lang.org
[crates.io]: https://crates.io
[crates.io/me]: https://crates.io/me
[Jacob Hoffman-Andrews]: https://github.com/jsha
[Siân Griffin]: https://github.com/sgrif
[Justin Geibel]: https://github.com/jtgeibel
[Pietro Albini]: https://github.com/pietroalbini
{% endraw %}
