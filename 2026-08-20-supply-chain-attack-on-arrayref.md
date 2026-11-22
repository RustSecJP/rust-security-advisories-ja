{% raw %}
# arrayref へのサプライチェーン攻撃 {#supply-chain-attack-on-arrayref}

- 原文の題名: Supply chain attack on arrayref
- 公開日: 2026-08-20
- 著者: Manish Goregaokar（security-response チームを代表して）
- 原文の URL: <https://blog.rust-lang.org/2026/08/20/supply-chain-attack-on-arrayref/>

## 何が起きたか {#what-happened}

2026-08-20 の 7:15 UTC に、`proc-macro1` クレートが悪意あるものだという報告を受けました。

Rust Security Response Team は、これが事実であることを確認しました。このクレートには、悪意あるペイロードをダウンロードするビルドスクリプトが含まれていました。

この `proc-macro1` クレートと、同様のクレート（`proc-macro-en`、`aovine`、`arone`、`aronenao`、`tinymember`）は削除されました。

さらに、人気のある [`arrayref`](https://crates.io/crates/arrayref) クレートが最近あらためて公開され、このクレートに依存するように変えられていたこと、そして直近のバージョンが yank されていたことが分かりました。私たちは悪意あるバージョンを削除し、悪意を持って yank されたバージョンの yank を取り消しました。同じ作者のほかのクレート（[`internment`](https://crates.io/crates/internment)、[`append-only-vec`](https://crates.io/crates/append-only-vec)）も影響を受けていたので、それらにも同じ対応を行い、予防措置としてアカウントをロックしました。`arrayref` の作者が悪意を持って行動しているとは考えていませんが、作者のコンピューターか認証情報が侵害されている可能性が高く、私たちは作者への連絡を試みています。

## あなたがすべきこと {#what-you-need-to-do}

ローカルの依存先を確認して、これらのクレートが取り込まれていないことを確かめるようおすすめします。私たちが crates.io から削除した悪意あるバージョンは次のとおりです。

* `append-only-vec@0.1.9`: `2026-08-20T07:37:49Z` に公開、`2026-08-20T09:25:24Z` に削除。公開されていた時間は 107 分。
* `arrayref@0.3.10`: `2026-08-20T07:15:00Z` に公開、`2026-08-20T08:41:40Z` に削除。公開されていた時間は 86 分。
* `internment@0.8.7`: `2026-08-20T07:34:07Z` に公開、`2026-08-20T09:04:11Z` に削除。公開されていた時間は 90 分。
* `proc-macro1`、`proc-macro-en`、`aovine`、`arone`、`aronenao`、`tinymember`（すべてのバージョン）。

これらのクレートがローカルで使われたかどうかは、次のコマンドで `~/.cargo/registry/cache` を調べることで、すばやく確認できます。

```bash
find ~/.cargo/registry/cache -type f \( \
  -name 'append-only-vec-0.1.9.crate' -o \
  -name 'arrayref-0.3.10.crate' -o \
  -name 'internment-0.8.7.crate' -o \
  -name 'proc-macro1-*.crate' -o \
  -name 'proc-macro-en-*.crate' -o \
  -name 'aovine-*.crate' -o \
  -name 'arone-*.crate' -o \
  -name 'aronenao-*.crate' -o \
  -name 'tinymember-*.crate' \
\) -print
```

## 謝辞 {#thanks}

この件を最初に発見して報告してくださった、Nextron Systems GmbH の Research Team に感謝します。また、今回の対応に参加した Emily Albini、Manish Goregaokar、Marco Ieni、Tobias Bieniek、Ubiratan Soares、Walter Pearce にも感謝します。
{% endraw %}
