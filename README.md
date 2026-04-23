# mXD — mXDownloader

**Windows ネイティブな X (Twitter) メディア自動保存トレイアプリ。**

> ⚠️ **v0.2.1 Beta.** 自分用運用で十分実用的なレベルですが、細部にラフな箇所が残っています。変更履歴は [CHANGELOG.md](CHANGELOG.md)、バグ報告・要望は [公開リポジトリの Issues](https://github.com/MikageSawatari/mxdownloader/issues) へどうぞ。

いいね / ブックマークしたツイートのメディア(画像・動画・animated GIF)を、15 分間隔でバックグラウンド取得してローカルに保存します。

## 主な機能

- システムトレイ常駐(Windows 10 / 11)
- OAuth 2.0 PKCE 認証(BYOK: 各ユーザが自分の X API Client ID を使用)
- いいね・ブックマークの差分取得(設定でどちらか片方のみも可)
- 引用ツイート / リツイートをいいねすると、元ツイートの画像も自動保存
- 漫画スレッドの続編(同一 conversation / 同じ作者)を自動追跡
- EXIF / XMP メタデータ埋め込み(ツイート URL・投稿者・本文・投稿日・発見日等)
- SQLite による重複排除 & 差分カーソル管理
- ローカル API ログからの使用状況表示
- Windows 起動時自動開始

## ユーザー向け配布

バイナリ配布とセットアップマニュアルは公開リポジトリにあります:

👉 **https://github.com/MikageSawatari/mxdownloader**

本リポジトリ(`mxdownloader-src`)は**ソースコード用で非公開**です。

## プロジェクト構成

```
src/
  mXDownloader.Core/   共通ロジック (認証、API、保存パイプライン、メタデータ)
  mXDownloader.Cli/    コマンドライン版 (開発・検証用)
  mXDownloader.Tray/   トレイ常駐 WPF アプリ (本体)
tools/
  exiftool.exe         メタデータ埋込用 (同梱、GPL/Artistic)
installer/
  mXD.iss              Inno Setup 6 スクリプト
scripts/
  publish.ps1          self-contained single-file 生成
  dev-deploy.ps1       開発中の一発再配備(shutdown.flag → publish → copy → 起動)
  build-release.ps1    publish + Inno Setup で setup.exe 生成
docs/
  byok-setup.md        エンドユーザー向けセットアップマニュアル
  miv-integration.md   mIV 連携仕様
  miv-feedback-*.md    mIV 側セッションからのフィードバック
```

## ビルド

```powershell
# 開発ビルド
dotnet build

# 開発中の動作確認(installed 位置に配備して起動)
powershell -File scripts\dev-deploy.ps1

# リリース用インストーラ生成
powershell -File scripts\build-release.ps1
```

要件:
- .NET 8 SDK
- Inno Setup 6 (`winget install JRSoftware.InnoSetup` で入る)
- `tools/exiftool.exe` が `tools/` 配下に存在すること

## ユーザーデータ配置

- 設定・認証トークン・DB: `%LOCALAPPDATA%\mXDownloader\`
- 実行バイナリ: `%LOCALAPPDATA%\Programs\mXD\`
- 保存先の既定: `%USERPROFILE%\mXD\`(Settings で変更可能)

## ライセンス

- mXD 本体: **MIT License**([LICENSE](LICENSE) 参照)
- 同梱サードパーティ: [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) 参照

## 関連プロジェクト

- [mIV (mimageviewer)](https://github.com/MikageSawatari/mimageviewer) — 連携相手の高速 Windows 11 画像ビューア。mXD が書き込む XMP `xtw:*` を読んで「元ツイートを開く」等のボタンを提供
