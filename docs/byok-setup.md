# mXD セットアップマニュアル

mXD は **あなた自身の X Developer アプリ(Client ID)** を使って動作します。API の課金もあなたの口座に行きます(BYOK 方式)。このマニュアルに沿って 20 分程度でセットアップ完了します。

## 必要なもの

- Windows 10 / 11 (64bit)
- X (Twitter) のアカウント
- クレジットカード(従量課金の支払い用。月数百円〜数千円を想定)
- 20 分程度の時間

## 料金の目安(重要)

mXD は X API の Pay-Per-Use を使います。2026 年 4 月時点の単価:

| 種類 | 単価 | いつ発生 |
|---|---|---|
| 自分のいいね取得 (owned read) | $0.001 / post | 15 分間隔のポーリング |
| 自分のブックマーク取得 (owned read) | $0.001 / post | 同上 |
| 漫画スレッド検索 (non-owned, search/recent) | $0.005 / post | thread 展開 ON 時 |

**目安:**
- 1 日 20 件ペースでいいねするユーザー + thread 展開 ON → **月 $50〜80** 想定
- ブックマークのみ + thread OFF → **月 $5〜15** 想定
- 初回バックフィル(800 件)は一度きり → **$1〜5** 想定

実際の請求は [console.x.com](https://console.x.com) で確認できます。**必ず Spending Cap(月額上限)を設定**して想定外の課金を防いでください。

---

## ステップ 1: X Developer アカウント

### 1-1. サインアップ

1. [developer.x.com](https://developer.x.com/) にアクセス
2. X アカウントでログイン
3. **"Sign up for Free Account"** を押す
4. 用途質問には、以下のような回答で OK:
   - 「What's your use case?」→ 「Personal use — saving my own liked tweets' media to local disk」
   - 「Will you make Twitter content or derived information available to a government entity or government-affiliated entity?」→ **No**
5. 利用規約に同意

→ Free Account が発行されます。

### 1-2. プロジェクト + アプリ作成

1. 左メニュー **"Projects & Apps"** → **"+ Create Project"**
2. プロジェクト名: 「mXD personal」等、任意
3. ユースケース: 「Exploring the API」
4. アプリ名: 「mXD」等、任意(グローバル一意なので mXD-{自分のユーザー名} が無難)

---

## ステップ 2: アプリ設定(OAuth 2.0 PKCE)

### 2-1. User Authentication Settings

1. アプリのトップ → **"User authentication settings"** → **"Edit"**
2. 以下を設定:

| 項目 | 値 |
|---|---|
| App permissions | **Read** |
| Type of App | **Native App** (Public client。mXD は PKCE を使います) |
| Callback URI / Redirect URL | `http://127.0.0.1:8976/callback` ← **完全一致必須** |
| Website URL | 任意(GitHub プロフィール URL 等で OK) |

3. **Save** を押す

### 2-2. Client ID を取得

1. **"Keys and tokens"** タブ
2. **"OAuth 2.0 Client ID and Client Secret"** のセクションの **Client ID** をコピー
3. コピーしたもの(長い英数字文字列)をメモ帳などに保存

**⚠️ Client Secret は mXD では使いません**(PKCE は Public Client 用なので Secret 不要)。

---

## ステップ 3: 支払い方法の登録

### 3-1. クレジットカード登録

1. [console.x.com](https://console.x.com/) にアクセス(Developer Portal とは別の URL です)
2. **Billing** または **Plans** メニューを探す
3. **Pay-Per-Use** プランに登録
4. クレジットカード情報を入力 → 最低金額(通常 $5)をチャージ

### 3-2. Spending Cap 設定(必須)

1. **Spending limit** / **Monthly cap** の項目を探す
2. **$5 〜 $20** あたりを設定(後で変更可能)
   - 初回様子見なら $5 が安全
   - 本格運用なら月間推定額を見ながら調整

これをしないと、バグや仕様誤認で**予想外の高額請求**になるリスクがあります。必ず設定してください。

---

## ステップ 4: mXD のインストール

### 4-1. ダウンロード

1. [GitHub リリースページ](https://github.com/MikageSawatari/mxdownloader/releases) を開く
2. 最新の `mXDownloader-v*-setup.exe` をダウンロード(~70 MB)

### 4-2. インストール

1. ダウンロードした setup.exe を実行
2. Windows SmartScreen 警告が出たら「詳細情報」→「実行」(署名なしのため)
3. インストーラの指示に従う:
   - インストール先:既定のまま `%LOCALAPPDATA%\Programs\mXD`
   - **☐ デスクトップアイコン** — お好みで
   - **☐ Windows 起動時に自動開始** — 推奨 ON

### 4-3. 起動確認

インストール完了後、タスクトレイにピンクの **mXD** アイコンが表示されます。

初回はまだ Client ID 未設定なのでエラー通知が出ます(想定内)。

---

## ステップ 5: mXD に Client ID を登録

### 5-1. 設定画面を開く

1. タスクトレイの **mXD** アイコンを右クリック
2. **「設定...」** を選択

### 5-2. Client ID を貼り付け

1. **Client ID** 欄に、ステップ 2-2 でコピーした Client ID を貼り付け
2. **保存先フォルダ** は既定のまま or 任意のフォルダを選択
   - OneDrive 同期配下は避ける(容量問題)
3. **ポーリング間隔** は 3600 秒(60 分)既定。短くすると新規反映は早いがコスト増。長くして取りこぼしが発生した場合はアプリが検知して通知します
4. **静音時間帯**(ON/OFF)— 指定時間帯はポーリング停止(手動更新は常に可)。就寝中のコストをさらに削減
5. **取得対象**:
   - いいね ☑
   - ブックマーク ☑
   - 両方 ON が標準。コスト重視なら片方だけ
6. **漫画スレッド検索** は任意
   - ON: メディア付きかつ 48h 以内のツイートに対してリプライで続く漫画の続編を取得。単価 $0.005/post
   - OFF: 単発のツイート画像のみ
7. **Windows 起動時に自動開始** も任意で ON

8. **「保存」** ボタンを押す

### 5-3. 認証(OAuth 2.0 PKCE)

1. 設定ウィンドウの **「X アカウントで認証」** ボタンを押す
2. 既定ブラウザが開き、X の認可ページが表示される
3. 「Authorize app」(または同等)を押して許可
4. 「Authorization complete.」の画面が出たらブラウザは閉じて OK
5. 設定ウィンドウに戻り、「認証完了しました。ポーリングを開始します。」と表示されれば成功

以降、ポーリング間隔ごと(既定 15 分)に自動で取得が走ります。Client ID を変更した場合は同じボタンで再認証してください。

---

## ステップ 6: 初回バックフィル(任意)

過去のいいね・ブックマークを一括取得したい場合:

1. トレイアイコン右クリック → **「設定...」**
2. **「初回バックフィル」** セクションで取得量を選択:
   - 直近 100 件まで (~$0.1)
   - 直近 500 件まで (~$0.5)
   - 最大 800 件 (~$1〜3)
3. **「実行」** ボタン

数秒〜数分で完了します。進捗はトレイアイコンのツールチップで確認できます。

**バックフィルを押さなくても**、これ以降の新規いいね・ブックマークは 15 分間隔で自動的に取得されます。過去分が欲しい場合だけ押してください。

---

## ステップ 7: 運用

### 7-1. 保存フォルダ構造

```
{保存先}/
  Likes/
    2026-04-20/
      20260420_1530_9999_{convId}_p01_m01_@author.jpg
      20260420_1530_9998_{convId}_p01_m01_@other.mp4
      ...
  Bookmarks/
    2026-04-20/
      ...
```

ファイル名の意味:
- `20260420_1530` — 発見(ポーリング)時刻
- `9999` — バッチ内で新しい順(新規いいねが 9999 から降順)
- `{convId}` — スレッド ID
- `p01_m01` — スレッド内 1 番目のツイート / そのツイート内 1 番目のメディア
- `@author` — 投稿者ハンドル

### 7-2. トレイメニュー

- **状態を表示...** — ダブルクリックでも開く。API 呼び出し履歴など
- **保存フォルダを開く**
- **ログフォルダを開く** — 問題調査用
- **今すぐ更新** — 15 分待たずに即ポーリング(60 秒間隔デバウンス)
- **設定...**
- **終了**

### 7-3. メタデータ

保存された画像には以下が EXIF / XMP で埋め込まれています:

- 標準 EXIF: Artist, ImageDescription, Copyright
- XMP Dublin Core: dc:creator, dc:description, dc:source
- mXD カスタム XMP: tweet ID / URL / 投稿者 / 投稿時刻 / スレッド位置 / 引用元(もしあれば)

mIV(mimageviewer)で開くと「X ツイート情報」セクションに表示され、「元ツイートを開く」「投稿者タイムライン」ボタンでブラウザに飛べます。

---

## トラブルシューティング

### 「Client ID 未設定」通知が出る

設定画面で Client ID を貼り付けて保存 → mXD を再起動。

### 「402 CreditsDepleted」エラーが出る

console.x.com でクレジット残高を確認。ゼロなら追加チャージ。

### 「429 Too Many Requests」エラーが連発

短時間に多くの API を叩くと rate limit に当たります。通常 15 分ポーリングなら出ないはず。**「今すぐ更新」の連打**を控えてください。

### 再認証したい(Client ID 変えた等)

1. トレイから mXD 終了
2. `%LOCALAPPDATA%\mXDownloader\tokens.bin` を削除
3. mXD 再起動 → 認証フローが再度走る

### アンインストール

Windows 設定 → アプリ → mXD → アンインストール。
- **ユーザーデータ(認証トークン・DB・設定)を削除しますか?** という確認が出ます。
  - **いいえ** を選ぶと再インストール時に設定が引き継がれます
  - 完全にクリーンにしたい場合は **はい**

保存した画像フォルダはアンインストーラが **触りません**。手動で削除してください。

---

## 参考リンク

- [X Developer Portal](https://developer.x.com/)
- [X API Pricing](https://docs.x.com/x-api/pricing)
- [X Developer Console](https://console.x.com/) — クレジット・請求確認
- [mXD GitHub](https://github.com/MikageSawatari/mxdownloader)
- [mXD Issues](https://github.com/MikageSawatari/mxdownloader/issues) — バグ報告・要望

---

## フィードバック

このマニュアルで分かりにくい箇所があれば Issue でお知らせください。
