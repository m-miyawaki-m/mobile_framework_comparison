# SQL Server Express + A5:SQL Mk-2 構築手順

VSCode 環境で簡易的に使える DB として **SQL Server Express** を立て、**A5:SQL Mk-2** から接続するまでの手順をまとめる。

## 方式選定の背景

A5:SQL Mk-2 が公式に対応している軽量 DB のうち、Windows ネイティブで完結し、外部ツール（A5）からの接続が容易なもの。

| | LocalDB | **Express（採用）** |
|---|---|---|
| インストールサイズ | 約 50MB | 約 300MB |
| TCP/IP | 既定で無効 | 設定で有効化可 |
| A5 接続 | 名前付きパイプ指定が必要・パイプ名が起動毎に変動 | `localhost\SQLEXPRESS` で直接接続できる |
| 用途 | アプリ組み込み用 | 小規模サーバー的に使える |

---

## 1. インストール

```powershell
winget install Microsoft.SQLServer.2022.Express
```

- インストール種別：**基本**
- インスタンス名：既定の `SQLEXPRESS` のまま
- インストール完了画面の **接続文字列 / インスタンス名** をメモ

代替：[公式ダウンロードページ](https://www.microsoft.com/ja-jp/sql-server/sql-server-downloads) からインストーラーを取得。

## 2. SQL Server Management Studio (SSMS) を導入（任意・推奨）

```powershell
winget install Microsoft.SQLServerManagementStudio
```

DB 作成・パスワード変更などを GUI で行える。

## 3. TCP/IP の有効化

1. スタートメニューから **「SQL Server 2022 構成マネージャー」** を起動
2. 左ペイン：**SQL Server ネットワークの構成 → SQLEXPRESS のプロトコル**
3. 右ペイン：**TCP/IP** を右クリック → **有効**
4. **TCP/IP** をダブルクリック → **IP アドレス** タブ
5. 一番下の **IPAll** で
   - `TCP 動的ポート` を **空欄**
   - `TCP ポート` を **`1433`**
6. 左ペイン：**SQL Server のサービス** → `SQL Server (SQLEXPRESS)` を右クリック → **再起動**

## 4. 混合モード認証の有効化

SSMS で Windows 認証で接続 → サーバー名を右クリック → **プロパティ → セキュリティ**

- **SQL Server 認証モードと Windows 認証モード** を選択 → OK
- サーバー名を右クリック → **再起動**

## 5. sa アカウントの有効化とパスワード設定

SSMS の新しいクエリで実行：

```sql
ALTER LOGIN sa ENABLE;
ALTER LOGIN sa WITH PASSWORD = 'YourStrong!Passw0rd';  -- 任意のものに変更
```

## 6. 作業用データベースの作成

```sql
CREATE DATABASE SampleDB;
```

## 7. ファイアウォール開放（同一 PC 内のみで使うなら不要）

管理者 PowerShell：

```powershell
New-NetFirewallRule -DisplayName "SQL Server 1433" -Direction Inbound -Protocol TCP -LocalPort 1433 -Action Allow
```

## 8. A5:SQL Mk-2 から接続

1. データベース追加 → **SQL Server (OLE DB)**
2. 入力項目：

| 項目 | 値 |
|---|---|
| サーバー名 | `localhost\SQLEXPRESS`（または `localhost,1433`） |
| 認証 | SQL Server 認証 |
| ユーザー ID | `sa` |
| パスワード | 手順 5 で設定したもの |
| データベース名 | `SampleDB` |

3. **テスト接続** → 成功で **OK**

---

## 接続トラブル時のチェック順

| 症状 | 確認ポイント |
|---|---|
| サーバーに接続できない | サービス `SQL Server (SQLEXPRESS)` が起動中か |
| ポート不明エラー | TCP/IP 有効化 & IPAll のポート 1433 設定済か / サービス再起動済か |
| ログイン失敗 | 混合モード認証か / `sa` が ENABLE か / パスワード合致 |
| 別 PC から繋がらない | ファイアウォールルール / `SQL Server Browser` サービス起動 |

## メモしておく値（A5 接続情報用）

- インスタンス名：`SQLEXPRESS`
- サーバー：`localhost\SQLEXPRESS` または `<PC 名>\SQLEXPRESS,1433`
- DB 名：`SampleDB`
- ユーザー：`sa`
- パスワード：（自分で決めたもの）
