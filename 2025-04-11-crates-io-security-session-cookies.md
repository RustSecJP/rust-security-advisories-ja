{% raw %}
# crates.io のセキュリティ事件: 不適切に保存されていたセッション cookie {#cratesio-security-incident-improperly-stored-session-cookies}

- 原文の題名: crates.io security incident: improperly stored session cookies
- 公開日: 2025-04-11
- 著者: Adam Harvey（crates.io チームを代表して）
- 原文の URL: <https://blog.rust-lang.org/2025/04/11/crates-io-security-session-cookies/>

本日、crates.io チームは、`cargo_session` cookie の内容が、エラー監視サービスである [Sentry](https://sentry.io/welcome/) に保存されていたことを発見しました。crates.io のバックエンドでエラーが起きたときに送信されるイベントのペイロードの一部として保存されていたものです。この cookie の値は、現在ログインしているユーザーを識別する署名つきの値です。したがって、これらの cookie の値を使えば、ログイン中の任意のユーザーになりすますことができました。

Sentry にアクセスできるのは、crates.io チーム、Rust インフラチーム、crates.io のオンコール当番チームのうち、信頼された一部のメンバーに限られています。これらのメンバーは、もともと crates.io の本番環境にアクセスできます。これらの値がアクセスされたり使われたりしたという証拠はありません。

それでも念には念を入れて、本日、次の対応を行いました。

1. [Sentry のすべてのイベントから、すべての cookie の値を伏せる変更をマージしてデプロイしました](https://github.com/rust-lang/crates.io/pull/10991)。
2. ログイン中のセッションをすべて無効にしました。これにより、Sentry に保存された cookie は使い物にならなくなります。実際のところ、これは crates.io のすべてのユーザーがブラウザーのセッションからログアウトされたことを意味します。

なお、API トークンはこの件の影響を**受けません**。API トークンは `Authorization` HTTP ヘッダーで送信され、イベントが Sentry に保存される前に、以前から適切に伏せられていました。既存の API トークンはすべて、引き続き使えます。

ご不便をおかけしたことをお詫びします。ほかにご質問があれば、[Zulip](https://rust-lang.zulipchat.com/#narrow/stream/318791-t-crates-io) または [GitHub](https://github.com/rust-lang/crates.io/discussions) でご連絡ください。
{% endraw %}
