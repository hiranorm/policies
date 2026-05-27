# policies

個人開発アプリ群のプライバシーポリシーを集約する静的サイト。Cloudflare Workers Static Assets にデプロイ。

## 技術スタック

- 素の HTML（ビルドツールなし）
- ホスティング: Cloudflare Workers Static Assets（GitHub 連携、`main` push で自動デプロイ）
- スタイル: 最小限のインライン or `<style>` ブロック。CSS フレームワーク不使用

## ディレクトリ構成

```
policies/
  wrangler.toml      # Pages 設定（pages_build_output_dir = "public"）
  public/            # Cloudflare Workers Static Assets の配信ルート。ここ以外は公開されない
    _headers         # セキュリティヘッダ
    index.html       # トップ：アプリ一覧
    <appname>/
      index.html     # 日本語版
      en.html        # 英語版（必要なら）
```

各アプリは独立ページを持つ。**1 つの HTML 内に複数アプリのポリシーを混在させない**（Play / Apple の審査でツッコまれるリスク）。

## 新規アプリ追加手順

1. `public/<appname>/` を作成
2. `index.html`（日本語版）と必要なら `en.html` を追加
3. 既存アプリ（tanzaku）の HTML をコピーして編集が早い
4. `public/index.html` のアプリ一覧にリンクを追加
5. README のアプリ一覧にも 1 行追記

## ローカル確認

```bash
npx wrangler dev
# → http://localhost:8787/
```

## ポリシー本文に最低限含める項目

Play Console / App Store / AdMob / RevenueCat の要求を満たす最小セット:

- アプリ概要（何のアプリか）
- 取得・保存するデータ
  - 端末内（ローカルストレージ・AsyncStorage 等）
  - 外部送信（広告 ID、購入レシート、解析 ID 等）
- 第三者提供（AdMob / RevenueCat / その他 SDK）
- 利用目的
- 保存期間
- ユーザーが取れるオプション（広告 ID リセット手順、サブスク解約手順、データ削除依頼方法）
- 子ども向けかどうか（13 歳未満を対象とするか）
- 連絡先（メールアドレス）
- 最終更新日

## デプロイ

- Cloudflare Workers Static Assets（プロジェクト 1 個、GitHub 連携）
- 配信対象: `public/`（`wrangler.toml` の `[assets] directory`）
- ビルド: なし
- リリース手順: `main` への push で自動デプロイ（Cloudflare 側で `wrangler deploy` が走る）
- ダッシュボード設定: Framework preset `None` / Build command 空 / Root directory 空。Workers の場合 Build output directory 欄は出ない（`[assets]` で宣言する）

## 記述言語

ドキュメント・コミットメッセージは原則日本語。ポリシー本文は配信ストアの審査対象言語に合わせる（日本ストアなら日本語、海外配信時は英語版も追加）。
