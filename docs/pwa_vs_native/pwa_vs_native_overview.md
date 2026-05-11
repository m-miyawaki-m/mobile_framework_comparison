# PWA / ハイブリッド / ネイティブ 概観 + 選定ガイド

3 構成のどれを採用するかを **5 分で判断する** ためのページ。詳細根拠は同ディレクトリの他資料を参照。

- 詳細レファレンス(118 項目): [`pwa_vs_native_criteria.md`](./pwa_vs_native_criteria.md)
- 機能 ◎/△/× 早見: [`pwa_vs_native_capabilities.md`](./pwa_vs_native_capabilities.md)
- レイヤ境界の原理: [`pwa_native_layer_boundary.md`](./pwa_native_layer_boundary.md)
- 構造図: [`architecture_pwa.puml`](./architecture_pwa.puml) / [`architecture_hybrid.puml`](./architecture_hybrid.puml) / [`architecture_native.puml`](./architecture_native.puml)
- 情報基準日: 2026 年 5 月時点

---

## 1. 3 構成の概観

| 構成 | 配信形態 | 実行ランタイム | 開発スタック | 代表技術 |
|---|---|---|---|---|
| **PWA**(純 PWA)| HTTPS URL アクセス + A2HS | ブラウザ(V8 + Blink/WebKit) | HTML / CSS / TS / Web API | React / Vue / Angular / Service Worker |
| **ハイブリッド**(Capacitor 等)| Play / MDM / APK 直配布 | APK ホスト(ART)+ WebView | 同上 + ネイティブ Plugin | Ionic + Capacitor(+ Vue / React / Angular)/ React Native / Flutter Web 等 |
| **ネイティブ** | 同上 | ART / iOS Runtime | OS 固有言語 | Kotlin + Jetpack Compose / Swift + SwiftUI / KMP / .NET MAUI |

---

## 2. 観点別 比較(早見)

| 観点 | PWA | ハイブリッド | ネイティブ |
|---|---|---|---|
| 配布の手軽さ | ◎ URL のみ | △ Store / MDM | △ Store / MDM |
| 即時更新(OTA) | **◎ サーバ更新で即時** | ○ Live Update(Web 資産のみ) | × Store 審査 |
| HW 機能アクセス | × ベンダー SDK 不可 | **◎ Plugin で全機能** | **◎ 全機能直接** |
| 既存 Web 資産流用 | ◎ | ◎ | × |
| 開発コスト(初期) | **◎ 1 コードベース** | ◎ 1 コードベース(+ Plugin)| △ OS 別実装(Android/iOS 2 倍)|
| UX(ネイティブ感) | △ ブラウザ範囲 | ○ WebView + Ionic で近似 | **◎ ネイティブルック** |
| 性能(60fps / GPU / 重 UI)| △ WebView 上限 | △ WebView 上限 | **◎ OS ネイティブ** |
| オフライン堅牢性 | △ IndexedDB 限界 | ◎ SQLite + Workbox | **◎ Room + WorkManager** |
| バックグラウンド常駐 | × | ○ Foreground Service Plugin | **◎ Foreground Service** |
| MDM 統合 | × Chrome Kiosk のみ | **◎ App Config / Device Owner** | **◎ 同上** |
| 商用 SLA | × | △ Ionic Enterprise | ○ ベンダー個別契約 |
| 長期保守(端末寿命 5〜7 年)| △ ブラウザ依存 | △ Capacitor 更新追従 | **◎ OS LTS と整合** |
| Store 審査 | **× 不要** | △ Store 経由(ハイブリッド配信時)| △ Store 経由 |
| 国内人材 | ◎(Web 系最大) | ◎ | ○(Android Kotlin 多)|

---

## 3. 用途別 推奨

| 用途 | 第一推奨 | 第二推奨 | 補足 |
|---|---|---|---|
| 社内情報系(申請/承認/閲覧)| **PWA** | ハイブリッド | ブラウザで十分なら PWA、HW 連携あればハイブリッド |
| 既存 Web アプリのモバイル化 | **PWA** | ハイブリッド | URL 配信が圧倒的に楽 |
| 顧客向け B2C(EC / SNS 等)| **ハイブリッド** | ネイティブ | UX 重視ならネイティブ、コスト重視ならハイブリッド |
| **HT 業務(Wedge モードで足りる)** | **ハイブリッド**(Capacitor)| ネイティブ | スキャン=単純テキスト入力なら Ionic 最短 |
| **HT 業務(Intent API / SDK 必須)** | **ハイブリッド** or **ネイティブ** | — | カスタム Plugin 自作 / 直組込 |
| **外付け SDK 端末(BT スキャナ・RFID リーダ等)** | **ハイブリッド**(Capacitor + カスタム Plugin)or **ネイティブ** | — | PWA 不可([`pwa_native_layer_boundary.md`](./pwa_native_layer_boundary.md) 参照)|
| 金融・公共・医療(規制系)| **ネイティブ + 商用サポート契約** | ハイブリッド(Ionic Enterprise) | HW Keystore / SLA / 証明書ピン留め |
| 高負荷 UI / 3D / ゲーム | **ネイティブ**(Compose / SwiftUI / Unity)| — | WebView では性能不足 |
| 長時間稼働(8h+ BG)| **ネイティブ** | ハイブリッド(注意) | Foreground Service / WorkManager |
| 1 画面〜数画面の軽量ツール | **PWA** | — | 過剰投資回避 |

---

## 4. 決定フロー(yes/no 分岐)

```
Q1. ベンダー SDK / Foreground Service / Keystore / MDM App Config が要件?
 ├─ Yes → Q2(PWA は除外)
 └─ No  → Q3

Q2. 商用 SLA / 長期保守 5〜7 年 / 高負荷 UI / Android 永続 のいずれか必須?
 ├─ Yes → ★ ネイティブ
 └─ No  → ★ ハイブリッド(Capacitor)

Q3. UX「ネイティブルック」「ジェスチャー」「ウィジェット」が必須?
 ├─ Yes → Q2 と同じ判断(ネイティブ / ハイブリッド)
 └─ No  → Q4

Q4. オフライン堅牢性(SQLite 級)/ 8h 連続稼働 / 重い BG 処理 が要件?
 ├─ Yes → ★ ハイブリッド(BG 限定なら)or ネイティブ
 └─ No  → ★ PWA
```

### 4.1 補足: 各分岐の典型例

| 分岐 | 該当しうる業務例 |
|---|---|
| Q1 = Yes | HT/RFID/プリンタ業務、銀行・医療など HW Keystore 要件、長時間 BG 同期 |
| Q2 = Yes | 預金系金融、医療機器連携、Android 業務 HT 5 年保守、ゲーム |
| Q3 = Yes | コンシューマ向け SNS、ニュースアプリ、写真編集アプリ |
| Q4 = Yes | 配送ルートアプリ(GPS 連続記録)、現場点検アプリ(オフライン優先)|
| Q4 = No(= PWA OK)| 社内申請、勤怠、ダッシュボード、社内 Wiki、社内チャットの軽量版 |

---

## 5. 中間形態(ハイブリッドのバリエーション)

「ハイブリッド」と一括りにしているが、実は複数の中間形態がある:

| 形態 | 概要 | 採用判断 |
|---|---|---|
| **Capacitor**(Ionic 系)| Web アプリを APK 化、Plugin 経由で Native | 本リポジトリの基本案 |
| **React Native** | JS ↔ Native Bridge、UI は JSI/Fabric で Native ビュー | UI ネイティブ感重視 |
| **Flutter** | Dart + Skia 独自レンダラ | デザイン統一・高描画性能 |
| **TWA**(Trusted Web Activity)| APK 殻 + Chrome Custom Tabs で PWA をロード | ストア掲載が欲しいだけの PWA |
| **Chrome キオスク**(PWA + Kiosk OS)| Chrome をキオスクモードで起動、PWA を全画面 | 据置端末で PWA を業務化 |
| **Capacitor + Live Update**(Ionic Appflow) | Capacitor APK の Web 資産だけ OTA | 即時更新が要件のハイブリッド |

詳細は [`pwa_vs_native_criteria.md`](./pwa_vs_native_criteria.md) XV 章。

---

## 6. このページの次に読むもの

- **「××機能は PWA でできる?」を確認したい** → [`pwa_vs_native_capabilities.md`](./pwa_vs_native_capabilities.md)
- **詳細根拠で 1 項目ずつ評価したい** → [`pwa_vs_native_criteria.md`](./pwa_vs_native_criteria.md)
- **PWA で SDK 連携できないのはなぜ?** → [`pwa_native_layer_boundary.md`](./pwa_native_layer_boundary.md)
- **PWA で Ionic 採用するならどの JS FW?** → [`../checks/05_js_framework_for_ionic.md`](../checks/05_js_framework_for_ionic.md)
- **モバイル FW 13 種から選びたい** → [`../checks/00_overview.md`](../checks/00_overview.md)

---

## 更新履歴

- **2026-05-11**: 初版作成。冗長な `pwa_vs_native_criteria.md` の概要版として整備。
