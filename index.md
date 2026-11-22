# Rust 公式ブログのセキュリティ勧告（Rust Blog 日本語訳）

Rust 公式ブログ（[blog.rust-lang.org](https://blog.rust-lang.org/)）に掲載されたセキュリティ勧告と、crates.io のセキュリティ事件・悪意あるクレートの報告記事の、非公式な日本語訳です。rustc、標準ライブラリ、Cargo、rustdoc、mdBook、regex クレートの脆弱性（CVE）の勧告、crates.io で起きた事件の報告、サプライチェーン攻撃や標的型攻撃への注意喚起を、公開日の古い順に全 29 本収めています。リリース告知や一般の記事は含めていません。

> **ご注意**: この翻訳は Rust Project や各記事の著者の確認や公認を受けたものではありません。内容が食い違う場合は原文が優先されます。各記事は公開された時点の状況を述べたものです。影響を受けるバージョンや対処の方法を確かめるときは、必ず原文と最新の公式情報を確かめてください。

## 記事の一覧 {#articles}

### 2018 年 {#y2018}

- 2018-07-06 — [rustdoc のセキュリティ勧告]({{ '/2018-07-06-security-advisory-for-rustdoc.html' | relative_url }})
- 2018-09-21 — [標準ライブラリのセキュリティ勧告]({{ '/2018-09-21-security-advisory-for-std.html' | relative_url }})
- 2018-10-19 — [2018 年 10 月 15 日の crates.io の事件についての続報]({{ '/2018-10-19-update-on-crates.io-incident.html' | relative_url }})

### 2019 年 {#y2019}

- 2019-05-13 — [標準ライブラリのセキュリティ勧告]({{ '/2019-05-13-security-advisory.html' | relative_url }})
- 2019-09-30 — [Cargo のセキュリティ勧告]({{ '/2019-09-30-security-advisory-for-cargo.html' | relative_url }})

### 2020 年 {#y2020}

- 2020-07-14 — [crates.io のセキュリティ勧告]({{ '/2020-07-14-crates-io-security-advisory.html' | relative_url }})

### 2021 年 {#y2021}

- 2021-01-04 — [mdBook のセキュリティ勧告]({{ '/2021-01-04-mdbook-security-advisory.html' | relative_url }})
- 2021-11-01 — [rustc のセキュリティ勧告（CVE-2021-42574）]({{ '/2021-11-01-cve-2021-42574.html' | relative_url }})

### 2022 年 {#y2022}

- 2022-01-20 — [標準ライブラリのセキュリティ勧告（CVE-2022-21658）]({{ '/2022-01-20-cve-2022-21658.html' | relative_url }})
- 2022-03-08 — [regex クレートのセキュリティ勧告（CVE-2022-24713）]({{ '/2022-03-08-cve-2022-24713.html' | relative_url }})
- 2022-05-10 — [セキュリティ勧告: 悪意あるクレート rustdecimal]({{ '/2022-05-10-malicious-crate-rustdecimal.html' | relative_url }})
- 2022-09-14 — [Cargo のセキュリティ勧告（CVE-2022-36113、CVE-2022-36114）]({{ '/2022-09-14-cargo-cves.html' | relative_url }})

### 2023 年 {#y2023}

- 2023-01-10 — [Cargo のセキュリティ勧告（CVE-2022-46176）]({{ '/2023-01-10-cve-2022-46176.html' | relative_url }})
- 2023-08-03 — [Cargo のセキュリティ勧告（CVE-2023-38497）]({{ '/2023-08-03-cve-2023-38497.html' | relative_url }})
- 2023-09-01 — [crates.io の事後報告: ユーザーがアップロードしたマルウェア]({{ '/2023-09-01-crates-io-malware-postmortem.html' | relative_url }})

### 2024 年 {#y2024}

- 2024-04-09 — [標準ライブラリのセキュリティ勧告（CVE-2024-24576）]({{ '/2024-04-09-cve-2024-24576.html' | relative_url }})
- 2024-09-04 — [標準ライブラリのセキュリティ勧告（CVE-2024-43402）]({{ '/2024-09-04-cve-2024-43402.html' | relative_url }})

### 2025 年 {#y2025}

- 2025-04-11 — [crates.io のセキュリティ事件: 不適切に保存されていたセッション cookie]({{ '/2025-04-11-crates-io-security-session-cookies.html' | relative_url }})
- 2025-09-12 — [crates.io を狙ったフィッシングキャンペーン]({{ '/2025-09-12-2025-09-crates-io-phishing-campaign.html' | relative_url }})
- 2025-09-24 — [crates.io: 悪意あるクレート faster_log と async_println]({{ '/2025-09-24-crates.io-malicious-crates-fasterlog-and-asyncprintln.html' | relative_url }})
- 2025-12-03 — [crates.io: 悪意あるクレート evm-units と uniswap-utils]({{ '/2025-12-03-crates.io-malicious-crates-evm-units-and-uniswap-utils.html' | relative_url }})
- 2025-12-05 — [crates.io: 悪意あるクレート finch-rust と sha-rust]({{ '/2025-12-05-crates.io-malicious-crates-finch-rust-and-sha-rust.html' | relative_url }})

### 2026 年 {#y2026}

- 2026-02-13 — [crates.io: 悪意あるクレートの通知方針の更新]({{ '/2026-02-13-crates.io-malicious-crate-update.html' | relative_url }})
- 2026-03-21 — [Cargo のセキュリティ勧告]({{ '/2026-03-21-cve-2026-33056.html' | relative_url }})
- 2026-05-25 — [Cargo のセキュリティ勧告（CVE-2026-5222）]({{ '/2026-05-25-cve-2026-5222.html' | relative_url }})
- 2026-05-25 — [Cargo のセキュリティ勧告（CVE-2026-5223）]({{ '/2026-05-25-cve-2026-5223.html' | relative_url }})
- 2026-08-20 — [arrayref へのサプライチェーン攻撃]({{ '/2026-08-20-supply-chain-attack-on-arrayref.html' | relative_url }})
- 2026-09-17 — [ご注意ください: 著名な Rustacean を狙った標的型攻撃]({{ '/2026-09-17-targeted-attacks.html' | relative_url }})
- 2026-09-21 — [Miri の出力をキャッシュすると GitHub Actions でシークレットが漏れる問題]({{ '/2026-09-21-github-actions-leaking-secrets-when-miri-output-is-cached.html' | relative_url }})

## 出典 {#source}

- 原文: [Rust Blog](https://blog.rust-lang.org/)（ソースは [rust-lang/blog.rust-lang.org](https://github.com/rust-lang/blog.rust-lang.org) の `content/` 以下。各記事の原文の URL は、それぞれのページの冒頭に示しています）
- 原著者: 各記事の著者・チーム（The Rust Project）。著者は各ページの冒頭に示しています。
- ライセンス: MIT または Apache-2.0
- 訳した版: 2026-11-20 時点（コミット `8666f7c`）
- 翻訳日: 2026-11-22
