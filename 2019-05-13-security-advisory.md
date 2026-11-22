{% raw %}
# 標準ライブラリのセキュリティ勧告 {#security-advisory-for-the-standard-library}

- 原文の題名: Security advisory for the standard library
- 公開日: 2019-05-13
- 著者: The Rust Core Team
- 原文の URL: <https://blog.rust-lang.org/2019/05/13/Security-advisory/>

これは[公式のセキュリティ勧告][official]の転載です。公式の投稿には、私たちの PGP 鍵で署名した版も含まれています。

この脆弱性の CVE は [CVE-2019-12083][cve] です。

[official]: https://groups.google.com/forum/#!topic/rustlang-security-announcements/aZabeCMUv70
[cve]: https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-12083

---

Rust チームは先日、`Error::type_id` の手書きの実装と、標準ライブラリの `Error::downcast` 系の関数との相互作用に影響する、セキュリティ脆弱性について通知を受けました。あなたのコードが `Error::type_id` を手書きで実装していなければ、あなたのコードは影響を受けません。

## 概要 {#overview}

標準ライブラリの `Error::type_id` 関数は、2019-04-11 の 1.34.0 リリースで安定化されました。この関数を使うと、もとになるエラー型の具体的な `TypeId` を取得して、元の型へダウンキャストできます。この関数には標準ライブラリに既定の実装がありますが、下流のクレートで上書きすることもできます。たとえば、次のコードは現在、Rust 1.34.0 と Rust 1.34.1 で許されています。

```rust
struct MyType;

impl Error for MyType {
    fn type_id(&self) -> TypeId {
        // Enable safe casting to `String` by accident.
        TypeId::of::<String>()
    }
}
```

これを `Error::downcast*` 系のメソッドと組み合わせると、ある型を誤った型へ safe なコードでキャストできてしまい、境界外の読み取りや書き込みなどのセキュリティ上の問題を引き起こします。

1.34.0 のリリースより前は、この関数は安定化されておらず、stable の Rust では実装することも呼び出すこともできませんでした。

## 影響を受けるバージョン {#affected-versions}

`Error::type_id` 関数は、2019-04-11 にリリースされた Rust 1.34.0 で初めて安定化されました。2019-04-25 に公開された Rust 1.34.1 のリリースも影響を受けます。`Error::type_id` 関数は、1.0.0 以降の Rust のすべてのリリースに不安定な機能として存在していました。つまり、nightly でコンパイルしたコードは、どの時点でも影響を受けていた可能性があります。

## 緩和策 {#mitigations}

このバグを直ちに緩和するには、`Error::type_id` の手書きの実装を取り除き、代わりに、安全性の観点から正しい既定の実装を継承する必要があります。`Error::type_id` がほかの型の `TypeId` インスタンスを返すことは、意図された使い方ではありません。

長期的な緩和策として、私たちはこの関数の安定化を取り消します。残念ながらこれは、`Error::type_id` を呼び出しているユーザーと、`Error::type_id` を上書きしているユーザーにとって、破壊的変更です。上書きしているユーザーのコードは、メモリ安全でない可能性が高いです。一方、`Error::type_id` を呼び出しているユーザーが stable でそれをできるようになったのは、前回の 1.34.0 リリース以降の数週間にすぎません。そのため、影響は乗り越えられないほど大きくはないと考えています。

2019-05-14（明日）に、[#58048][1] を取り消して `Error::type_id` 関数の安定化を取り消す、1.34.2 のポイントリリースを公開します。今後の 1.35.0 リリースと、beta / nightly チャンネルも、すべて安定化の取り消しを含むように更新されます。

`Error::type_id` API の最終的な扱いはまだ決まっておらず、[#60784][2] で議論されています。現時点では、安定化の取り消し以外の対応は予定していないため、nightly のコードではこの問題が引き続き起こりえます。標準ライブラリの中でこの問題を近いうちに完全に解決したいと考えています。

## 出来事の時系列 {#timeline-of-events}

* 2019 年 5 月 9 日（木）14:07 PM - バグが security@rust-lang.org に報告される
* 2019 年 5 月 9 日（木）15:10 PM - Alex が返信し、バグを確認
* 2019 年 5 月 10 日（金）- 緩和の方針を立て、実装
* 2019 年 5 月 13 日（月）- [stable][3] / [beta][4] / [master][5] の各ブランチ向けの PR を GitHub に投稿
* 2019 年 5 月 13 日（月）- セキュリティのメーリングリストにこの問題を通知
* （予定）2019 年 5 月 14 日（火）- この問題の修正を含む Rust 1.34.2 をリリース

## 謝辞 {#acknowledgements}

このバグを見つけ、私たちの[セキュリティポリシー][sec]に従って報告してくださった Sean McArthur 氏に感謝します。

[sec]: https://www.rust-lang.org/policies/security

[1]: https://github.com/rust-lang/rust/pull/58048
[2]: https://github.com/rust-lang/rust/issues/60784
[3]: https://github.com/rust-lang/rust/pull/60785
[4]: https://github.com/rust-lang/rust/pull/60786
[5]: https://github.com/rust-lang/rust/pull/60787
{% endraw %}
