{% raw %}
# ご注意ください: 著名な Rustacean を狙った標的型攻撃 {#be-alert-targeted-attacks-on-prominent-rustaceans}

- 原文の題名: Be alert: targeted attacks on prominent Rustaceans
- 公開日: 2026-09-17
- 著者: Adam Harvey（crates.io チームと Security Response WG を代表して）
- 原文の URL: <https://blog.rust-lang.org/2026/09/17/targeted-attacks/>

rust-lang のメンバーや人気のあるクレートの所有者を狙った攻撃キャンペーンが、現在進行中であると私たちは考えています。その狙いは、端末やアカウントを侵害し、それらを使ってマルウェアを公開することです。

## 私たちが確認していること {#what-weve-seen}

何か前向きな用件（仕事の話かもしれませんし、プロジェクトの話かもしれませんし、契約の機会かもしれません）のためにビデオ通話が設定されます。そして、その通話が、標的のコンピューターに何かをインストールさせる（たとえば、足りないと称する音声コーデック）か、別のコマンドを実行させる（たとえば、クリップボードにコマンドを入れる）ための経路として使われます。

こうした攻撃者は、ざっと調べられた程度では見破られないように、新しいけれども正規に見える会社のプロフィールを用意しています。もっともらしい LinkedIn の情報もそこに含まれます。

6 月には、[この形の以前の攻撃][grack]が多くの著名な Rust 開発者を狙いました。そして先月には、[同様の攻撃によって `arrayref` クレートが一時的に侵害されました][arrayref]。これらがすべて同じキャンペーンの一部なのかどうかは、現時点では分かっていません。

この攻撃の手口は、[DPRK（北朝鮮）が使うことで知られており][dprk]、[Rust コミュニティの外でも確認されています][ashishb]。

## あなたにできること {#what-you-can-do}

当面は、いつも以上に注意してください。面識の無い相手からの突然の連絡は相応に疑い、初めての相手との通話は必ず、あなたが信頼するプラットフォームで行うようにしてください。理想的には、あなたがすでに使っているプラットフォームで、あなたのほうから通話を設定するようにしてください。

あわせて、アカウントの状態に異常が無いかを、あらためて確認してください。多要素認証（MFA）が有効になっているか、ログインを追跡できるプラットフォームで身に覚えのないログインが無いか、などです。

アカウントについて少しでも心配なことがあれば、[help@crates.io](mailto:help@crates.io)（crates.io のアカウントに関する心配ごと）や [security@rust-lang.org](mailto:security@rust-lang.org)（それ以外の心配ごと）までご連絡ください。喜んでお手伝いします。


[arrayref]: 2026-08-20-supply-chain-attack-on-arrayref.html
[ashishb]: https://ashishb.net/security/contagious-interview/
[dprk]: https://kudelskisecurity.com/research/how-dprks-contagious-interview-campaign-targets-developers
[grack]: https://grack.com/blog/2026/06/25/dissecting-a-failed-nation-state-attack/
{% endraw %}
