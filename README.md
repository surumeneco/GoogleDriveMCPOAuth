# GoogleDriveMCPOAuth

個人開発環境で利用する Google Drive MCP の OAuth アプリ向けに、**アプリのホームページ**と**プライバシーポリシー**を公開する静的サイトです。

Google が提供する公式アプリではなく、一般向けサービスでもありません。GitHub Pages 上のサイト自体には Google 認証処理や Google Drive へのアクセス処理を実装していません。認証と Drive API の操作は、利用者が構成するローカル MCP サーバーで行います。

## ファイル構成

- `index.html` — OAuth アプリのホームページ。目的、主な機能、処理の流れを説明。
- `privacy.html` — アクセスするデータ、利用・保存・送信・削除・問い合わせ方法。
- `style.css` — 両ページ共通のレスポンシブスタイル。
- `README.md` — 本書。公開とOAuth設定の手順。

依存関係、ビルド、JavaScriptは不要です。外部の計測・広告スクリプトは導入していません。

## GitHub Pages の公開（利用者側で実施）

1. リポジトリの [Settings → Pages](https://github.com/surumeneco/GoogleDriveMCPOAuth/settings/pages) を開く。
2. **Build and deployment** → **Source** を **Deploy from a branch** に設定。
3. **Branch** は `main`、公開ディレクトリは `/(root)` に設定して **Save**。
4. デプロイ完了後、ホームページとプライバシーポリシーが両方表示されることを確認。

独自ドメイン未設定時のGitHub Pagesの標準URLは通常次の形式です（公開前はアクセスできません）。

- ホームページ: https://surumeneco.github.io/GoogleDriveMCPOAuth/
- プライバシー: https://surumeneco.github.io/GoogleDriveMCPOAuth/privacy.html

**注意:** Google OAuth のブランド確認ではドメインの所有権確認が求められます。そのため、GoogleのOAuth設定に登録する際は、自分が管理する独自ドメインでPagesを公開することを推奨します。

## 独自ドメインの設定（利用者側で実施）

1. GitHubのPages画面にある **Custom domain** に、自分が管理するサブドメイン（例: `oauth.example.com`）を登録して **Save**。
2. DNS管理画面で、選択したサブドメインのCNAMEを `surumeneco.github.io` に設定。
3. DNSと証明書の状態を確認し、利用可能になったら **Enforce HTTPS** を有効にする。
4. ホームページと `/privacy.html` の両方がHTTPSでアクセスできることを確認。
5. [Google Search Console](https://search.google.com/search-console) で必要なドメインの所有権を確認する。

GitHubのPages設定から独自ドメインを登録すると、ブランチ公開方式では通常 `CNAME` ファイルが自動生成されます。**このリポジトリには仮の `CNAME` を置いていません。**

## Google Auth Platform への登録（利用者側で実施）

Google Cloud の同じ OAuth プロジェクトで、[Branding](https://console.cloud.google.com/auth/branding) と [Audience](https://console.cloud.google.com/auth/audience) を設定します。

| 項目 | 入力内容 |
| --- | --- |
| アプリ名 | 実際のOAuth同意画面に設定したアプリ名と一致させる |
| アプリのホームページURL | 公開した独自ドメインのトップページ（`https://<独自ドメイン>/`） |
| プライバシーポリシーURL | `https://<独自ドメイン>/privacy.html` |
| 利用規約URL | 任意。Google側から要求された場合は実際のページを別途用意する |
| Authorized domains | ホームページ等の登録可能ドメイン（通常はサブドメインではなく親の登録可能ドメイン） |
| デベロッパー連絡先 | 自分が連絡を受けられるメールアドレス（Google Cloud側に設定） |

公開ページの本文に記載する内容は、実際の利用構成に一致させてください。現時点の記載は **ローカルで動作するMCPと、Codex等のAIエージェントによるDrive参照・編集**を想定しています。Googleドライブのデータを外部AIに渡す可能性についても開示しています。

Google Workspace向けの公式MCP Developer Previewに参加する必要はありません。Google OAuthの `In production` と、ブランド確認・制限付きスコープ審査は別の概念です。公開状態を変更しただけでGoogleによる検証済みアプリになるわけではありません。

## 保守と安全性

- 認証情報（`gcp-oauth.keys.json`、`tokens.json`、Client Secret、アクセストークン）はこのリポジトリへ**絶対にコミットしない**。
- プライバシーポリシーと実際のMCP構成・Googleの認証スコープ・外部AIへのデータ送信実態が一致するよう維持する。
- 運用形態が変わったときは `privacy.html` の記載と「最終更新日」を修正する。
- ページ内リンクは相対パスなので、独自ドメイン設定時にHTMLを修正する必要はない。
- 更新後は公開URLを開いて表示を確認する。Google OAuthの設定画面にも同じURLを登録する。

## 参考資料

- [Google OAuth ブランド確認とホームページ要件](https://developers.google.com/identity/protocols/oauth2/production-readiness/brand-verification?hl=ja)
- [Google API Services User Data Policy](https://developers.google.com/terms/api-services-user-data-policy?hl=ja)
- [GitHub Pages 公開ソースの設定](https://docs.github.com/ja/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)
- [GitHub Pages 独自ドメインの設定](https://docs.github.com/ja/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)
- [使用しているサードパーティーMCPのソース](https://github.com/piotr-agier/google-drive-mcp)
