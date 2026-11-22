{% raw %}
# Miri の出力をキャッシュすると GitHub Actions でシークレットが漏れる問題 {#github-actions-leaking-secrets-when-miri-output-is-cached}

- 原文の題名: GitHub Actions leaking secrets when Miri output is cached
- 公開日: 2026-09-21
- 著者: Manish Goregaokar（security-response チームを代表して）
- 原文の URL: <https://blog.rust-lang.org/2026/09/21/github-actions-leaking-secrets-when-miri-output-is-cached/>

Rust Security Response Team は、Miri がすべての環境変数を `target/` に保存しており、そのためにシークレットがキャッシュの中に残り続けるおそれがあるとの通知を受けました。

これは、それ単体では必ずしも脆弱性ではありません。しかし、GitHub Actions のキャッシュの挙動と組み合わさると、シークレットが PR に露出する可能性があります。


## 概要 {#overview}


GitHub Actions では、実行をまたいでディレクトリをキャッシュできます。典型的な構成では、`main`（やそのほかのブランチ）での CI の実行にはキャッシュへの*書き込み*を許し、PR にはキャッシュからの*読み取り*だけを許します（キャッシュの汚染を防ぐためです）。Rust のプロジェクトでは、`cargo install` でビルドしたバイナリや、ときには `target/` の内容をキャッシュして、CI を高速化することがよくあります。

PR の CI は、あなたのリポジトリに PR を開ける人なら誰でも起動できます。GitHub は*最初の* PR にはメンテナーの承認を求めますが、それ以降の PR では、プッシュのたびに CI が再実行されます。過去に一度でも変更を取り込まれたことのある人は誰でも、キャッシュされた `target/` から情報を抜き出す CI の実行を起動し、そのあと PR に 2 つ目のコミットをプッシュして痕跡を隠すことができます。

GitHub は、上書きされたコミットを UI 上で隠すことがあり、この種の攻撃を見つけにくくしています。CI の実行ログと上書きされたコミットも、数か月後には削除されます。

`cargo miri` が呼び出されると、Miri は、ビルドに関係する環境変数を実行をまたいで保持する必要があります[^1]。現在のコードは、[すべての環境変数を `target/` に保存する](https://github.com/rust-lang/miri/blob/165a9c3c96f0f4e6232278e62cb64648215bbc37/cargo-miri/src/util.rs#L40-L42)ことでこれを実現しています。当然ながら、`target/` がキャッシュされると、これも残り続けます。

あなたの環境にシークレットが含まれていた場合、PR はキャッシュを通じてそれにアクセスできることになります。

## 私たちの修正 {#our-fix}

これに対する[短期的な修正][miri-fix]は、Miri が保持する環境変数を、`CARGO_*`（`CARGO_*_TOKEN` を除く）と `OUT_DIR` だけにすることです。長期的には、関係する環境変数の一覧を Miri に伝えるもっと良い方法を、Miri と cargo で考え出すかもしれません。なお、このパッチは、まだ nightly では利用できない可能性があります。

私たちは GitHub リポジトリを対象にエコシステムのスキャンも行い、この問題を抱えるリポジトリを 1 つ、脆弱ではなさそうだが念のため注意すべきリポジトリを 7 つ特定しました。これらのメンテナーには連絡済みです。

 [miri-fix]: https://github.com/rust-lang/miri/pull/5337


## 自分は影響を受けるか {#am-i-affected}

私たちのスキャンは完全ではなかった可能性が高いので、Miri を実行している場合は、ご自身の GitHub Actions の設定を確認することをおすすめします。

次の条件に当てはまる場合、あなたは脆弱です。

 * CI で `cargo miri` を実行している
 * `cargo miri` を実行するステップが、環境変数として[シークレットにアクセスできる](https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-secrets#using-secrets-in-a-workflow)。経路は次のいずれかです。
     * そのステップ自体に環境変数として渡されている
     * ワークフローの `env` に設定されている
     * 以前のステップに渡され、そのステップが何らかの形で環境に残している
 * 使っているワークフローが `target` ディレクトリをキャッシュしている。通常は [`actions/cache`](https://github.com/actions/cache) や [`swatinem/rust-cache`](https://github.com/swatinem/rust-cache) で行われます
 * そのキャッシュに PR からアクセスできる（よくある構成で、多くの場合は意図された使い方です）

手早く行える対処には、次のようなものがあります。

 * そのジョブのキャッシュを無効にする。
 * シークレットの範囲を、そのジョブの中で Miri を呼ばないステップだけに絞る。
 * Miri を一時的に無効にする。

対処が済んだら、[キャッシュを消去してください](https://docs.github.com/en/actions/how-tos/manage-workflow-runs/manage-caches#deleting-cache-entries)。漏れた可能性のあるシークレットは、ローテーション（入れ替え）を検討してください。

次の nightly（2026-09-22）に含まれる Miri のリリースでは、この問題は無くなります。


**Miri を実行していない場合でも、** 公開のキャッシュに書き込めるジョブがシークレットにアクセスできないようにしてください。多くのツールはシークレットを特別扱いせず、環境全体をファイルシステムに書き出してよいものと想定しています。

## 脅威モデル {#threat-model}

私たちは、シークレットが簡単に紛れ込みうるキャッシュを持つことは、良くない習慣だと考えています。

`target/` をキャッシュするのであれば、`target/` を作るプロセス（`cargo` を呼び出すものすべて）への入力にシークレットが含まれないようにしておく価値があります。標準的な `cargo` のビルドやテストのサブコマンドがシークレットやトークンを必要とすることは、一般にまれです[^2]。そのため、これは主に、シークレットを環境変数としてジョブ全体に露出させないよう気をつける、という話になります。

Cargo / Miri / Rust は、環境変数が `target/` にコピーされないことを保証していません。私たちは念には念を入れて、これをセキュリティ上の問題として扱い、修正しますが、これは一般に当てにしてよいものではありません。公式の Rust のツール以外にも、ビルドスクリプトが、環境をコンパイル成果物の中に保存する結果になるような処理をしている可能性があります。

## 謝辞 {#acknowledgements}

この問題を報告してくださった OpenAI の [Predrag Gruevski](https://github.com/obi1kenobi) 氏に感謝します。また、エコシステムのスキャンは、OpenAI から寄贈された Codex の利用権とクレジットを使って行いました。この点についても OpenAI に感謝します。


問題のトリアージと修復は、Manish Goregaokar、Ralf Jung、Ben Kimock、Weihang Lo、Jacob Finkelman、Walter Pearce、Josh Stone、Mark Rousskov が行いました。


[^1]: Miri は、込み入った理由により、`cargo miri` から複数回呼び出されます
[^2]: 理論上は、ネットワークから読み込みを行うビルドスクリプトがあれば必要になりえます
{% endraw %}
