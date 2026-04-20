# Third-party notices

mXD は下記のサードパーティソフトウェアを同梱または利用しています。本体 mXD 自身は MIT ライセンス([LICENSE](LICENSE))で配布されますが、同梱されるサードパーティコンポーネントはそれぞれのライセンスに従います。

## ExifTool (bundled)

- Author: Phil Harvey
- Homepage: https://exiftool.org/
- Bundled as: `tools/exiftool.exe` and `tools/exiftool_files/`
- License: GNU General Public License v1 or later **or** the "Artistic License" (dual-licensed, choose one)
- 同梱ファイル `tools/exiftool_files/LICENSE` に GPL v3 の全文が含まれています

mXD は ExifTool を**別プロセスとして外部呼び出し**しているだけで、mXD のコード自体とリンクしてはいません。したがって mXD のコードは MIT のままです。ExifTool バイナリを再配布する場合は GPL もしくは Artistic のいずれかのライセンス条項に従ってください。

ExifTool のソースコードは https://exiftool.org/ から入手できます。

## .NET 8 Desktop Runtime (bundled via self-contained publish)

- Author: Microsoft
- Bundled: self-contained `mXDownloader.exe` は .NET 8 Desktop Runtime のネイティブ依存関係を含みます
- License: MIT (https://github.com/dotnet/core/blob/main/LICENSE.TXT)

## SQLite (via Microsoft.Data.Sqlite)

- License: Public Domain (SQLite 本体) / Apache 2.0 (Microsoft.Data.Sqlite ラッパー)
- Homepage: https://www.sqlite.org/

## その他 NuGet パッケージ

- `Microsoft.Data.Sqlite` (Apache-2.0)
- `SQLitePCLRaw.bundle_e_sqlite3` (Apache-2.0)
- `System.Security.Cryptography.ProtectedData` (MIT)

---

Third-party ライセンス条項の不整合や誤記を見つけた場合は Issue でお知らせください。
