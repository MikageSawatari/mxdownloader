# mXD — mikage X Downloader

**X (Twitter) のいいね・ブックマークしたメディアを自動保存する Windows トレイアプリ。**

> 🧪 **v0.1.0 Beta** — 自分用運用では十分実用できるレベルです。フィードバックは [Issues](https://github.com/MikageSawatari/mxdownloader/issues) へどうぞ。

## できること

- システムトレイに常駐し、15 分間隔で X のいいね・ブックマークを自動取得
- 画像・動画・アニメ GIF を CDN から直接ダウンロード(画像本体は API 課金外)
- 投稿者・ツイート URL・本文・投稿時刻を EXIF / XMP メタデータとして埋め込み
  - [mIV (mimageviewer)](https://github.com/MikageSawatari/mimageviewer) で開くと「元ツイートを開く」「投稿者タイムライン」ボタンが利用可能
- 引用ツイート・リツイートをいいねした場合、**元ツイートの画像も自動保存**
- 漫画スレッドの続編(同一会話 + 同作者のリプライ)を自動追跡
- いいね・ブックマークを個別に ON / OFF(コスト調整用)
- Windows 起動時自動開始
- 初回バックフィル量を選択(100 件 / 500 件 / 最大 800 件)

## ダウンロード

👉 [**最新リリース**](https://github.com/MikageSawatari/mxdownloader/releases/latest)

`mXDownloader-v*-setup.exe`(約 72 MB)をダウンロードして実行してください。

- **Windows 10 / 11 (64bit)** が必要
- **.NET 8 Runtime は同梱**(self-contained ビルド)
- **管理者権限不要**(ユーザーホーム配下 `%LOCALAPPDATA%\Programs\mXD\` に per-user インストール)
- Windows SmartScreen 警告が出たら「詳細情報」→「実行」で進んでください(コード署名は未対応)

## セットアップ

mXD は **BYOK (Bring Your Own Key)** 方式です。各ユーザが自分の X Developer アカウントで Client ID と Pay-Per-Use プランを用意する必要があります。

詳細手順: **[docs/byok-setup.md](docs/byok-setup.md)**

## 料金について(重要)

X API の Pay-Per-Use 従量課金なので、運用方法次第で月額が大きく変わります。

| 運用形態 | 月額目安 |
|---|---|
| ブックマークのみ + 漫画スレッド検索 OFF | **$5 〜 $15** |
| いいね + ブックマーク + 漫画スレッド検索 OFF | **$15 〜 $30** |
| いいね + ブックマーク + 漫画スレッド検索 ON(既定) | **$50 〜 $80** |

※ 1 日 20 件ペースのいいね前提の目安です。実際の請求は [console.x.com](https://console.x.com) で確認してください。

**必ず Spending Cap(月額上限)を設定**して想定外の課金を防いでください。詳細は [セットアップマニュアル](docs/byok-setup.md) の「ステップ 3-2」を参照。

## スクリーンショット

作成中です。

## ライセンス

- mXD 本体: **MIT License**([LICENSE](LICENSE))
- 同梱する ExifTool バイナリ: GPL / Artistic のデュアルライセンス(詳細は [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md))

## ソースコード

ソースは非公開リポジトリで管理しています。Issue や機能要望は**本リポジトリの [Issues](https://github.com/MikageSawatari/mxdownloader/issues)** で受け付けています。

## 関連プロジェクト

- [mIV (mimageviewer)](https://github.com/MikageSawatari/mimageviewer) — mXD と連携する高速 Windows 11 画像ビューア。mXD が保存したファイルの XMP メタデータを読み、元ツイートへのリンクをワンクリックで開けます

## 作者

Mikage Sawatari ([@mikage](https://x.com/mikage))
