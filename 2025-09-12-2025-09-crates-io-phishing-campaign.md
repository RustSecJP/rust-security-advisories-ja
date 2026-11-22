{% raw %}
# crates.io を狙ったフィッシングキャンペーン {#cratesio-phishing-campaign}

- 原文の題名: crates.io phishing campaign
- 公開日: 2025-09-12
- 著者: Rust Security Response WG、crates.io team
- 原文の URL: <https://blog.rust-lang.org/2025/09/12/crates-io-phishing-campaign/>

crates.io のユーザーを狙ったフィッシングキャンペーン（`rustfoundation.dev` というドメイン名から送られてくるもの）について、複数の報告を受け取りました。このメールは、私たちのインフラが侵害されたと述べ、クレートへの被害を抑えるために認証を行うようユーザーに求めています。

これらのメールは悪意あるものです。送信元のドメイン名は Rust Foundation が管理しているものではなく（Rust Project が管理しているものでもなく）、あなたの GitHub の認証情報を盗むことが目的とみられます。crates.io のインフラが侵害されたという証拠はありません。

私たちは、このドメイン名を停止させるための手続きと、crates.io 上の不審な活動の監視を進めています。このようなメールを受け取っても、メール内のリンクは一切開かないでください。また、お使いのメールプロバイダーでフィッシングとして報告してください。

ほかにご質問があれば、<security@rust-lang.org> と <help@crates.io> までご連絡ください。
{% endraw %}
