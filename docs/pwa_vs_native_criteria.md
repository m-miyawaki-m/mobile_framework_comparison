# PWA vs ネイティブアプリ 選定観点 詳細

`android_business_app_framework_comparison.md` 第 5 章「PWA vs ネイティブ」を拡張した詳細版。
選定時に考慮すべき観点を 15 カテゴリ・約 120 項目で網羅し、各項目について **PWA / ネイティブ / ハイブリッド(Ionic+Capacitor・TWA・Chrome Kiosk 等)** での対応度と選定影響度を整理する。

- 情報は **2026 年 4 月時点**(Android 業務アプリ・HT を主対象)
- 凡例(対応度): ◎=完全対応 / ○=標準的に対応 / △=制約あり・要自作 / ×=原理的に不可
- 凡例(選定影響度): **決定的**=1 項目で選定が固まりうる / **強い**=複数項目で大差 / 中=チューニングで緩和可 / 小=設計次第

---

## 0. 結論サマリ

| 観点カテゴリ | PWA が勝てる領域 | ネイティブ必須領域 |
|---|---|---|
| 配布・更新 | ★(URL アクセス、サーバ更新で即反映) | — |
| HW 機能 | — | ★(赤外線・シリアル・UWB・RFID・HT スキャナ Intent 等) |
| オフライン | — | ★(堅牢 SQLite + WorkManager) |
| BG 処理 | — | ★(長時間常駐・WorkManager) |
| セキュリティ | — | ★(ハードウェアキーストア等) |
| UX | — | ★(ネイティブルック) |
| 開発コスト | ★(Web 資産活用・1 コード) | — |
| MDM 運用 | △(Chrome キオスクは可) | ★(App Config・Device Owner) |

**決定的な足切り観点(どれか該当で PWA 即除外)**: IV3〜22 の大半、VI の長時間 BG、XV1〜3 以外の機器接続要件。

---

## 1. 判断の基本構造

PWA か ネイティブかを決める判断は、通常以下の順で決まる:

1. **絶対的な足切り**(物理機器連携・常駐処理・HW キーストア等) → 該当すれば **ネイティブ or ハイブリッド一択**
2. **運用要件**(MDM / Kiosk / 長期保守 / 端末寿命) → 企業運用で **ネイティブ優位**に傾く
3. **UX 要件**(ネイティブルック / ジェスチャー / ウィジェット) → ネイティブ優位
4. **既存資産 / チームスキル** → Web 資産がある場合 PWA/ハイブリッドが有利
5. **開発・保守コスト** → 小規模・短期なら PWA 有利
6. **ビジネス戦略**(ストア流入 / SEO 流入 / 広告費) → 用途次第

結果、以下のどちらかに落ち着くのが一般的:

- **純 PWA**(軽量・URL アクセス完結・既存 Web 資産活用)
- **ハイブリッド(Capacitor + Ionic 等)**(PWA の資産 + ネイティブ機能の両取り、本件の現実解の 1 つ)
- **純ネイティブ**(HT 業務 + 長期保守 + ハードウェア深連携)

---

## 2. 観点カテゴリ 概要表

| カテゴリ | 項目数 | 主要な差別化 |
|---|---|---|
| I. 配布・アクセス | 6 | ストア vs URL、発見性、インストール摩擦 |
| II. 更新・配信 | 7 | 即時反映 vs ストア審査 |
| III. 実行環境・性能 | 8 | ブラウザ vs OS ランタイム、非力 SoC での挙動 |
| IV. ハードウェア機能アクセス | **22**(拡充済) | **PWA vs ネイティブの最大の分水嶺** |
| V. ストレージ・オフライン | 7 | IndexedDB 上限 vs SQLite 堅牢性 |
| VI. バックグラウンド実行 | 6 | Service Worker vs WorkManager / Foreground Service |
| VII. 認証・セキュリティ | 9 | ハードウェアキーストア・Web 起因脆弱性 |
| VIII. UX・プラットフォーム統合 | 9 | ネイティブルック、ジェスチャー、ウィジェット |
| IX. 開発・保守 | 8 | 既存 Web 資産、LTS、破壊的変更 |
| X. エンタープライズ運用・MDM | 9 | App Config・Kiosk・Device Owner |
| XI. プラットフォームポリシー | 6 | ストア審査、ブラウザ API 制限 |
| XII. ビジネス・戦略 | 6 | 流入導線、課金手数料 |
| XIII. 法規制・コンプライアンス | 6 | GDPR・HIPAA・PCI・公共調達 |
| XIV. 地域・環境特性 | 4 | GMS 有無、通信環境、端末多様性 |
| XV. ハイブリッド/中間形態 | 5 | TWA・Capacitor・Chrome Kiosk |

計 **118 項目**(IV の拡充分含む)

---

## 3. 詳細(全 15 カテゴリ)

### I. 配布・アクセス観点

| # | 観点 | PWA | ネイティブ | ハイブリッド(Capacitor 等)| 選定影響度 |
|---|---|---|---|---|---|
| I1 | 配布チャネル | URL / A2HS | **Google Play / MDM / APK 直配布** | Play / MDM(APK 化) | 中 |
| I2 | インストール摩擦 | ◎ URL アクセスのみ | △ ストア経由またはサイドロード | 同ネイティブ | 中 |
| I3 | 発見性 | ◎ SEO / 検索流入 | ○ ストア検索・ASO | ○ | 中 |
| I4 | 初回ダウンロードサイズ | ◎ 必要最小のアセット | △ 数十 MB 一括 | 同ネイティブ | 小 |
| I5 | 初回起動時のデータ取得量 | △ ブラウザキャッシュに依存 | ◎ アセット同梱可 | ◎ | 小 |
| I6 | アイコン・ホーム画面配置 | ○ A2HS、**iOS では制約あり** | ◎ 標準 | ◎ | 小 |

### II. 更新・配信観点

| # | 観点 | PWA | ネイティブ | ハイブリッド | 影響度 |
|---|---|---|---|---|---|
| II1 | 更新配信の即時性 | **◎ サーバ更新で即反映** | △ ストア審査・MDM 配信遅延 | ○ Appflow Live Update / EAS Update | 強い |
| II2 | ストア審査の有無 | **×(不要)** | ◎ Play / App Store 審査 | 一部不要(Live Update) | 強い |
| II3 | 強制アップデート | △ キャッシュ制御依存 | ◎ In-App Update API | ○ | 中 |
| II4 | ロールバック容易性 | ◎ 前バージョンのアセット戻しで即対応 | △ ストア再提出 | ○ | 中 |
| II5 | 段階的ロールアウト | △ 自前実装 | ◎ Play Staged Rollout | ○ | 小 |
| II6 | バージョン混在の許容度 | △ キャッシュ次第で新旧混在 | ◎ バージョン管理容易 | ○ | 中 |
| II7 | 緊急パッチ展開速度 | **◎ 最速** | △ 審査数時間〜数日 | ○ Live Update | 強い |

### III. 実行環境・性能観点

| # | 観点 | PWA | ネイティブ | ハイブリッド | 影響度 |
|---|---|---|---|---|---|
| III1 | 実行基盤 | ブラウザ / WebView | OS ランタイム(ART) | WebView(ネイティブホスト) | — |
| III2 | Cold start 時間 | △ ブラウザ初期化コスト | ◎ 最速 | △ WebView 起動コスト | 中 |
| III3 | スループット | ○ ブラウザ JS エンジン依存 | ◎ ネイティブ最速 | ○ WebView JS | 中 |
| III4 | メモリフットプリント | △ ブラウザプロセス丸ごと | ◎ 最少 | △ WebView 分増 | 中 |
| III5 | バッテリー効率 | **△ 常時 JS 実行で消費大** | ◎ 最適化容易 | △ 同 PWA | 強い |
| III6 | 8 時間連続稼働 | △ Chromium メモリ事故リスク | ◎ | △ 要チューニング | **強い** |
| III7 | 非力 SoC(旧 HT)での動作 | △ WebView 初期レンダ遅延 | ◎ | △ | 強い |
| III8 | 高負荷 UI / 3D / 重アニメ | △ Canvas/WebGL だが制約 | ◎ Impeller / Reanimated | △ | 中 |

### IV. ハードウェア機能アクセス観点(**拡充済 22 項目**)

| # | 観点 | PWA | ネイティブ | ハイブリッド | 影響度 |
|---|---|---|---|---|---|
| IV1 | カメラ・マイク | ◎ getUserMedia / MediaRecorder(制約あり) | ◎ CameraX | ◎ @capacitor/camera | 中 |
| IV2 | 位置情報(GPS) | ◎ Geolocation API | ◎ Fused Location | ◎ @capacitor/geolocation | 小 |
| IV3 | Bluetooth Classic | **× 不可** | ◎ BluetoothSocket | ◎ Capacitor plugin | **決定的** |
| IV4 | BLE(Central) | △ **Web Bluetooth(Chromium Android のみ)** | ◎ | ◎ | **決定的**(iOS Safari 不可) |
| IV5 | NFC(NDEF 読取) | △ **Web NFC(Chromium Android のみ)** | ◎ | ◎ | **決定的**(iOS 不可、NDEF 限定) |
| IV6 | NFC Host Card Emulation | × | ◎ | ○ | 決定的 |
| IV7 | USB(Host / Accessory) | △ **Web USB(Chromium)**、制約多 | ◎ | ◎ | **決定的** |
| IV8 | **業務用スキャナ(DataWedge Wedge)** | ◎(キー入力として受信) | ◎ | ◎ | 小(どれでも可) |
| IV9 | **業務用スキャナ(DataWedge Intent)** | **×(Intent 受信不可)** | ◎ | ○ 自作プラグイン | **決定的** |
| IV10 | UHF RFID(産業用) | × | ◎ ベンダー SDK | △ カスタム | **決定的** |
| IV11 | **プリンタ(ZPL/ESC-POS、BT/USB)** | △ HTTP ネットワーク型のみ可 | ◎ 各 SDK | ○ | 強い |
| IV12 | センサー(加速度・ジャイロ・磁気・気圧) | ◎ Device Orientation / Motion | ◎ | ◎ | 小 |
| IV13 | 生体認証 | ○ **WebAuthn**(パスキー対応) | ◎ Biometric API | ◎ | 中 |
| IV14 | **赤外線(IrDA / IR Blaster)** | **× 不可** | ◎ ConsumerIrManager | ○ 自作プラグイン | **決定的** |
| IV15 | **シリアル通信(RS-232C / UART)** | × | ◎ USB Serial | ◎ | **決定的** |
| IV16 | Wi-Fi Direct / P2P | × | ◎ WifiP2pManager | ○ | 決定的 |
| IV17 | 画面キャスト(Miracast / Chromecast) | △ Presentation API(制約) | ◎ MediaRouter / Cast SDK | ○ | 中 |
| IV18 | UWB(超広帯域・屋内測位) | × | ○ Android 13+(限定 API) | △ | 決定的 |
| IV19 | IoT 近距離通信(ANT+ / Zigbee / Thread / Matter) | × | ◎(ドングル/SDK 併用) | ○ | **決定的** |
| IV20 | オーディオジャック経由周辺機器(旧決済リーダ等) | × | ◎ | ○ | 決定的 |
| IV21 | 外部ディスプレイ(HDMI / USB-C Alt / DP) | △ Presentation | ◎ DisplayManager | ○ | 中 |
| IV22 | **物理キー / ハードウェアトリガー** | △ Web のみ keydown、一部取りこぼし | ◎ KeyEvent | △ WebView 境界で取りこぼし | **強い**(HT) |
| IV23 | MSR(磁気ストライプ) | × | ◎ ベンダー SDK | ○ | 強い |

**この IV がほぼすべての選定判断を決める**。**IV3〜7, IV9〜11, IV14〜16, IV18〜20, IV23** のいずれか 1 つでも要件なら **PWA は原理的に不可**、ネイティブまたはハイブリッドの二択になる。

### V. ストレージ・オフライン観点

| # | 観点 | PWA | ネイティブ | ハイブリッド | 影響度 |
|---|---|---|---|---|---|
| V1 | 保存容量上限 | △ ブラウザ・OS の制約(数十 MB〜数 GB、保証なし) | ◎ 端末容量まで | ◎ | 強い |
| V2 | DB 選択肢 | IndexedDB / OPFS | **SQLite + Room / Drift / SQLDelight** | SQLite plugin / IndexedDB | 強い |
| V3 | キャッシュの永続性 | **△ OS がストレージ圧迫時に削除する可能性** | ◎ 自由に制御 | △ | **強い** |
| V4 | オフライン動作保証 | △ Service Worker 頼り | ◎ アプリ自体がオフライン前提で動く | ○ | 強い |
| V5 | 送信失敗時の自動再送キュー | △ Background Sync(Chromium のみ) | ◎ WorkManager 堅牢 | △ | **強い** |
| V6 | 断続的接続での業務継続 | △ | ◎ | ○ | 強い |
| V7 | 保存時暗号化 | △ WebCrypto 自力 | ◎ EncryptedSharedPreferences / SQLCipher | ○ | 中 |

### VI. バックグラウンド実行観点

| # | 観点 | PWA | ネイティブ | ハイブリッド | 影響度 |
|---|---|---|---|---|---|
| VI1 | バックグラウンド同期 | △ **Background Sync API**(Chromium 限定・短時間)| ◎ WorkManager | △ | **強い** |
| VI2 | 定期実行 | △ **Periodic Sync**(制約強、Chromium のみ)| ◎ WorkManager / AlarmManager | △ | 強い |
| VI3 | 常駐サービス(Foreground Service)| × | ◎(API 34+ は type 宣言必須)| △ 自作 plugin | **決定的** |
| VI4 | プッシュ通知 | ○ **Web Push**(iOS Safari 16.4+ で対応)| ◎ FCM / APNS | ◎ | 中 |
| VI5 | ローカル通知 | △ Notification API、OS 側の扱いに制限 | ◎ | ◎ | 中 |
| VI6 | WakeLock / CPU スリープ抑止 | △ Screen Wake Lock API のみ | ◎ PowerManager | ○ | 中 |

### VII. 認証・セキュリティ観点

| # | 観点 | PWA | ネイティブ | ハイブリッド | 影響度 |
|---|---|---|---|---|---|
| VII1 | 認証トークン保管 | Cookie / localStorage(XSS リスク) | **Android Keystore(TEE / StrongBox)** | 中間(プラグインで Keystore 可) | **強い** |
| VII2 | ハードウェアキーストア | × 利用不可 | ◎ | ○ | **決定的**(高セキュリティ要件) |
| VII3 | クライアント証明書認証 | ○ ブラウザの証明書ストア(制約)| ◎ KeyChain API + OkHttp | ◎ | 強い |
| VII4 | 証明書ピン留め | × | ◎ NetworkSecurityConfig / OkHttp | ◎ | 強い |
| VII5 | Root / 改ざん検出 | × | ◎ Play Integrity API | ○ | 強い |
| VII6 | アプリ署名・改ざん防止 | × | ◎ APK 署名、V2/V3 | ◎ | 中 |
| VII7 | XSS / CSRF 等 Web 固有脆弱性 | **リスクあり** | なし | リスクあり | 強い |
| VII8 | OS サンドボックス強度 | ブラウザ プロセス単位 | ◎ アプリ単位 UID | 中 | 中 |
| VII9 | CSP(Content Security Policy) | ◎ 標準 | — | ◎ WebView に適用 | 小 |

### VIII. UX・プラットフォーム統合観点

| # | 観点 | PWA | ネイティブ | ハイブリッド | 影響度 |
|---|---|---|---|---|---|
| VIII1 | ネイティブ UI ルック | △ Web UI、Ionic でネイティブ風 | ◎ | ○(Ionic で近似) | 強い |
| VIII2 | ジェスチャー・戻る操作 | △ ブラウザの戻る | ◎ OS ジェスチャー統合 | ○ | 中 |
| VIII3 | システム通知統合 | △ Notification API 基本のみ | ◎ Channel / インライン返信 | ○ | 中 |
| VIII4 | Share Sheet | ○ **Web Share API** | ◎ | ◎ | 小 |
| VIII5 | ディープリンク / Universal Links | ◎ URL そのもの | ◎ App Links / Universal Links | ◎ | 小 |
| VIII6 | ホーム画面ウィジェット | × | ◎ AppWidget / Glance | × | 中 |
| VIII7 | タスクスイッチ・マルチウィンドウ | △ ブラウザの扱い | ◎ 独立アプリ | ◎ | 中 |
| VIII8 | アクセシビリティ(TalkBack) | ◎ Web 標準 a11y | ◎ | ◎ | 小 |
| VIII9 | スプラッシュ・フルスクリーン | △ **iOS で制約**、Android は可 | ◎ | ◎ | 小 |

### IX. 開発・保守観点

| # | 観点 | PWA | ネイティブ | ハイブリッド | 影響度 |
|---|---|---|---|---|---|
| IX1 | 既存 Web 資産流用 | ◎ 100% | × | ◎ 90%+ | **強い** |
| IX2 | 必要スキル | Web(HTML/CSS/JS/TS) | **Kotlin + Android SDK + ART + Gradle** | Web + 一部ネイティブ | **強い** |
| IX3 | コードベース数(Android + iOS + Web)| **1**(Web のみ) | **2**(Android + iOS 別)| 1〜2 | 強い |
| IX4 | Hot Reload / Fast Refresh | ◎ Vite HMR 瞬時 | △ Compose Live Edit 制約 | ◎ Vite HMR | 中 |
| IX5 | テスト戦略 | Jest / Vitest / Playwright | JUnit + Espresso + Compose Test | Web テスト + 実機 | 中 |
| IX6 | 新 OS / 新ブラウザ対応工数 | △ ブラウザ多様性 | ◎ Android SDK 単一 | △ | 中 |
| IX7 | 長期保守性・LTS | ブラウザの後方互換性 | Google Play Target SDK 毎年引き上げ | 中間 | 中 |
| IX8 | 破壊的変更頻度 | 低(Web 標準は保守的) | 中(Android SDK ポリシー) | 中 | 中 |

### X. エンタープライズ運用・MDM 観点

| # | 観点 | PWA | ネイティブ | ハイブリッド | 影響度 |
|---|---|---|---|---|---|
| X1 | MDM 経由配布 | △ Chrome キオスクで URL 固定可 | ◎ APK 直配布 | ◎ | 強い |
| X2 | **App Config(Managed Configuration)動的注入** | × | ◎ RestrictionsManager | ○ プラグイン | **決定的**(必要時) |
| X3 | Kiosk / Lock Task | ◎ Chrome キオスク | ◎ startLockTask / Device Owner | ○ | 中 |
| X4 | Device Owner 連携 | × | ◎ DevicePolicyManager | ○ | 強い |
| X5 | リモートロック・ワイプ | × | ◎ MDM + DevicePolicyManager | ○ | 中 |
| X6 | 拠点別・ロール別設定配信 | △ サーバ側 API で | ◎ App Config | ○ | 中 |
| X7 | クラッシュレポート収集 | ○ Sentry(JS) | ◎ Firebase Crashlytics / Sentry | ◎ | 小 |
| X8 | アナリティクス・監査ログ | ◎ Web 標準 | ◎ | ◎ | 小 |
| X9 | 端末別バージョン固定 | △ | ◎ | ◎ | 中 |

### XI. プラットフォームポリシー・規約観点

| # | 観点 | PWA | ネイティブ | ハイブリッド | 影響度 |
|---|---|---|---|---|---|
| XI1 | **Google Play Target SDK 毎年引き上げ** | 無関係 | 対応必須 | 対応必須 | 中 |
| XI2 | App Store 審査(iOS) | 無関係 | 対応必須 | 対応必須 | 中 |
| XI3 | Chromium 系ブラウザ API 制限 | **強く影響**(Web NFC/BT/USB) | 無関係 | 無関係(ネイティブ層経由)| **強い** |
| XI4 | Safari / iOS WebKit の制約 | **強く影響** | 無関係 | WebView 側に波及 | 強い |
| XI5 | 広告・課金規約(ストア手数料 15〜30%)| なし | Google Play 15〜30% | 同ネイティブ | 中 |
| XI6 | コンテンツ規制 | ブラウザ(ほぼ自由)| ストア規約(検閲あり)| ストア規約 | 中 |

### XII. ビジネス・戦略観点

| # | 観点 | PWA | ネイティブ | ハイブリッド | 影響度 |
|---|---|---|---|---|---|
| XII1 | ユーザー獲得導線 | **◎ SEO / 検索流入 / URL 共有** | ○ 広告 / ASO | ◎(両取り) | 強い |
| XII2 | アプリ内課金・ストア手数料 | **◎ 手数料なし**(外部決済) | △ 15〜30% | 同ネイティブ | 中 |
| XII3 | ブランド統一(アイコン・スプラッシュ) | △ iOS 制約 | ◎ | ◎ | 小 |
| XII4 | SEO 流入重要度 | ◎ | × | ◎ | ビジネス次第 |
| XII5 | 初回体験のハードル | **◎ URL タップのみ** | △ インストール壁 | △ | 中 |
| XII6 | 継続利用率(Engagement) | △ 離脱しやすい | ◎ | ○ | 中 |

### XIII. 法規制・コンプライアンス観点

| # | 観点 | PWA | ネイティブ | ハイブリッド | 影響度 |
|---|---|---|---|---|---|
| XIII1 | 個人情報保護(GDPR / CCPA / 個人情報保護法)| ○ Web と同じ | ○ | ○ | 小(両方対応可) |
| XIII2 | 医療規制(HIPAA / PMDA / 薬機法)| △ 要件次第で困難 | ◎(ハードウェアキーストア等) | ○ | **強い** |
| XIII3 | 金融規制(PCI-DSS / FISC)| △ 決済画面の PCI 対応は可 | ◎ | ○ | 強い |
| XIII4 | 公共調達要件(アクセシビリティ・セキュリティ評価)| Web 標準で有利 | 要個別評価 | 中間 | 中 |
| XIII5 | 輸出規制(暗号ライブラリ)| Web 標準 Crypto 適用 | ◎ 別途申請要の場合あり | 同 | 小 |
| XIII6 | アクセシビリティ法令(JIS X 8341 / WCAG)| ◎ Web 標準 | ◎ a11y Service | ◎ | 小 |

### XIV. 地域・環境特性観点

| # | 観点 | PWA | ネイティブ | ハイブリッド | 影響度 |
|---|---|---|---|---|---|
| XIV1 | 中国(GMS 非搭載・ブラウザ規制)| △ ブラウザ規制次第 | ◎ ただし GMS 非依存設計要 | ○ | 強い |
| XIV2 | 通信環境(低速回線・間欠接続)| △ Service Worker 頼り | ◎ オフライン前提設計 | ○ | 強い |
| XIV3 | 端末の多様性(OS・ブラウザ版数)| △ ブラウザ版数差 | ◎ Android SDK 単一 | △ | 中 |
| XIV4 | 業務端末(HT のブラウザ更新不能・Android 古い世代)| **×**(旧 WebView 固定で CSS Grid 等未対応の場合あり)| ○ Min SDK を下げて対応 | △ | **強い** |

### XV. ハイブリッド / 中間形態の選択観点

| # | 観点 | 内容 | 本件での適性 |
|---|---|---|---|
| XV1 | **PWA + TWA**(Android アプリ化) | Chrome Custom Tabs ベースのアプリラッパ、ストア配信可 | △ HT 実績少、Intent 受信はラッパで拡張要 |
| XV2 | **Capacitor / Cordova ハイブリッド** | Web + ネイティブ機能の両取り、Android/iOS/PWA/Electron 同時展開 | ○(**本件の現実解の 1 つ**) |
| XV3 | **Chrome キオスク構成** | Chrome `--kiosk` で URL 固定、MDM で一括管理 | ○(Wedge モードで済むなら実用的) |
| XV4 | Ionic + PWA 同時展開 | 同一 Web コードを PWA・APK・iOS に分岐配信 | ◎ Web 資産最大活用 |
| XV5 | A2HS + ストアアプリの併走 | 高機能ユーザ向けはストア、軽量ユーザ向けは PWA | ビジネス戦略で有効 |

---

## 4. 足切り観点(この 1 項目で PWA 即除外)

以下 **22 項目**のどれか 1 つでも要件なら、**純 PWA は原理的に不可**。ネイティブまたはハイブリッド(ネイティブ層で実装)が必須。

| # | 項目 | 理由 |
|---|---|---|
| 1 | IV3 Bluetooth Classic | Web 未対応 |
| 2 | IV4 BLE(iOS 運用含む)| Safari 未対応 |
| 3 | IV5 NFC(iOS 運用含む)| Safari 未対応、Chromium は NDEF 限定 |
| 4 | IV6 NFC HCE | Web 未対応 |
| 5 | IV7 USB Host | Chromium 限定かつ制約多 |
| 6 | IV9 **DataWedge Intent API** | Broadcast Intent 受信不可 |
| 7 | IV10 UHF RFID | ベンダー SDK 必須 |
| 8 | IV11 プリンタ(BT/USB)| ネットワーク型以外不可 |
| 9 | IV14 **赤外線(IR Blaster)** | Web 未対応 |
| 10 | IV15 **シリアル通信(RS-232C)** | Web 未対応 |
| 11 | IV16 Wi-Fi Direct | Web 未対応 |
| 12 | IV18 UWB | Web 未対応 |
| 13 | IV19 ANT+ / Zigbee / Thread / Matter | Web 未対応 |
| 14 | IV20 オーディオジャック機器 | API なし |
| 15 | IV23 MSR(磁気カード) | ベンダー SDK |
| 16 | VI3 Foreground Service(長時間常駐)| Web 未対応 |
| 17 | VII2 **ハードウェアキーストア**(TEE) | Web 未対応 |
| 18 | VII4 証明書ピン留め | Web 未対応 |
| 19 | X2 **App Config 動的注入** | RestrictionsManager は OS API |
| 20 | X4 Device Owner 連携 | OS API |
| 21 | VIII6 ホーム画面ウィジェット | Web 未対応 |
| 22 | XIV4 旧 Android HT の WebView 固定 | CSS Grid / ES2020+ 未対応案件 |

## 4.5. ネイティブ or アプリ化が必須になる規制シナリオ

| 規制 | 要件 | 影響 |
|---|---|---|
| 医療(PMDA クラス II/III、HIPAA)| 監査ログ・暗号化・改ざん防止 | ネイティブ推奨 |
| 金融(FISC 安全対策基準) | ハードウェアキーストア、Root 検出 | ネイティブ推奨 |
| 政府・自衛 | アプリ署名、トラステッド配信 | ネイティブ必須 |
| 輸出規制品(RFID/無線)| 端末ごとの認証 | ネイティブ |

---

## 5. 決定フロー(フローチャート風)

```
START
  │
  ▼
Q1. 足切り観点(セクション 4)のうち、1 つでも該当するか?
  ├─ YES → 【ネイティブ or ハイブリッド】 一択
  │         ▼
  │      Q2. Web 資産を最大活用したいか?
  │       ├─ YES → 【ハイブリッド(Capacitor+Ionic+Vue/React/Angular)】
  │       └─ NO  → 【ネイティブ(Kotlin + Jetpack Compose)】
  │
  └─ NO  → Q3. 8h連続稼働 / 通信断跨ぎ / 物理キー多用 のどれかあるか?
            ├─ YES → 【ネイティブ or ハイブリッド】推奨
            └─ NO  → Q4. 既存 Web 資産 / 短期開発 / 軽量用途 か?
                     ├─ YES → 【PWA(+ Chrome キオスク)】
                     └─ NO  → Q5. iOS 展開 / ブランド統一 必要か?
                              ├─ YES → 【ネイティブ or Flutter】
                              └─ NO  → 【ハイブリッド】が現実解
```

---

## 6. 典型シナリオ別推奨

| シナリオ | 推奨 | 理由 |
|---|---|---|
| 社内業務(申請・承認・閲覧のみ)、既存 Web あり | **PWA + A2HS** | 最短・最軽量、Web 資産活用 |
| 社内業務 + 社外モバイル + 多機能 | **Ionic+Capacitor(ハイブリッド)** | Web+モバイル同時、MDM 配布可 |
| HT 業務(Wedge のみ、軽量) | **PWA + Chrome キオスク** | MDM で URL 固定、実装最短 |
| **HT 業務(Intent / プリンタ / BT / BG 常駐)** | **ネイティブ Kotlin** | Intent 受信・HW 統合・長時間常駐 |
| 営業支援・フィールド(オフライン・カメラ・位置)| Flutter or RN | 性能+オフライン+UI |
| コンシューマ向け SaaS・SEO 流入重視 | **PWA + ネイティブ並走** | SEO+ストアの両取り |
| 金融・医療モバイル(高セキュリティ) | **ネイティブ** | ハードウェアキーストア・規制対応 |
| 車載・産業機器 | Flutter Embedded / ネイティブ | 特殊機器 |

---

## 7. 本件(Android HT 業務アプリ + Java/jQuery チーム + Android 永続)への適用

### 7.1 足切り観点チェック結果

| 観点 | 本件での確認 | 判定 |
|---|---|---|
| IV9 DataWedge Intent | MultiBarcode / プロファイル切替要? | **YES なら PWA 不可** |
| IV11 プリンタ | ZPL BT プリンタ連携あり? | YES なら PWA 不可 |
| IV22 物理ファンクションキー | 多用する業務か? | YES なら PWA 不利 |
| V5 オフラインキュー再送 | 倉庫・店舗の通信断でも確実に再送要? | YES なら PWA 不利 |
| VI3 8h 常駐 | 1 シフト連続稼働? | YES なら PWA 不利 |
| VII2 ハードウェアキーストア | 機密データ扱う? | YES なら PWA 不可 |
| X2 MDM App Config 注入 | 拠点別設定の動的注入要? | YES なら PWA 不可 |

### 7.2 3 シナリオの現実解

| シナリオ | 推奨 | 想定工数 |
|---|---|---|
| 【A】DataWedge Wedge のみ・8h 常駐不要・MDM Config 不要 | **PWA + Chrome キオスク** | 最小 |
| 【B】Wedge 以外の機能要るが Web 資産活用したい | **Ionic + Capacitor + Vue** | 中 |
| 【C】本格 HT(Intent・常駐・プリンタ・MDM Config)| **ネイティブ Kotlin + Jetpack Compose** | 大 |

**本件の典型的な結論**:

- 一般に HT 業務アプリは 【C】ネイティブが最有力
- Web 資産活用重視なら 【B】Vue+Ionic+Capacitor
- 簡易照会系なら 【A】PWA + Chrome キオスク

---

## 8. 参照リンク

### 8.1 Web 標準・PWA

| 対象 | URL |
|---|---|
| Progressive Web Apps(MDN) | https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps |
| web.dev PWA | https://web.dev/explore/progressive-web-apps |
| Chrome Status(Platform API) | https://chromestatus.com/ |
| Service Worker | https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API |
| Web App Manifest | https://developer.mozilla.org/en-US/docs/Web/Manifest |
| Web Bluetooth | https://web.dev/articles/bluetooth |
| Web NFC | https://web.dev/articles/nfc |
| Web USB | https://developer.chrome.com/docs/capabilities/usb |
| Web Push(Notifications) | https://developer.mozilla.org/en-US/docs/Web/API/Push_API |
| Background Sync | https://web.dev/articles/background-sync |
| Periodic Sync | https://web.dev/articles/periodic-background-sync |
| Barcode Detection API | https://developer.chrome.com/docs/capabilities/web-apis/shape-detection |
| WebAuthn(パスキー) | https://www.w3.org/TR/webauthn-3/ |
| Web Share API | https://developer.mozilla.org/en-US/docs/Web/API/Web_Share_API |

### 8.2 Android ネイティブ(参考)

| 対象 | URL |
|---|---|
| Android Developers | https://developer.android.com/ |
| WorkManager | https://developer.android.com/topic/libraries/architecture/workmanager |
| Foreground Services | https://developer.android.com/develop/background-work/services/foreground-services |
| Managed Configurations | https://developer.android.com/work/managed-configurations |
| Biometric API | https://developer.android.com/training/sign-in/biometric-auth |
| Android Keystore | https://developer.android.com/privacy-and-security/keystore |
| Play Integrity API | https://developer.android.com/google/play/integrity |

### 8.3 ハイブリッド(Capacitor / TWA)

| 対象 | URL |
|---|---|
| Capacitor | https://capacitorjs.com/ |
| Ionic Framework | https://ionicframework.com/ |
| Trusted Web Activity(TWA) | https://developer.chrome.com/docs/android/trusted-web-activity |
| Digital Asset Links | https://developer.android.com/training/app-links/verify-android-applinks |
| Chrome kiosk | https://support.google.com/chrome/a/answer/9273974 |

### 8.4 HT 業務・Zebra

| 対象 | URL |
|---|---|
| Zebra DataWedge Keystroke Output | https://techdocs.zebra.com/datawedge/latest/guide/output/keystroke/ |
| Zebra DataWedge Intent Output | https://techdocs.zebra.com/datawedge/latest/guide/output/intent/ |
| Zebra EMDK for Android | https://techdocs.zebra.com/emdk-for-android/latest/guide/about/ |

### 8.5 規制・コンプライアンス

| 対象 | URL |
|---|---|
| WCAG 2.2 | https://www.w3.org/WAI/standards-guidelines/wcag/ |
| JIS X 8341-3(アクセシビリティ) | https://waic.jp/ |
| 個人情報保護法 | https://www.ppc.go.jp/ |
| FISC 安全対策基準 | https://www.fisc.or.jp/ |
| PCI-DSS | https://www.pcisecuritystandards.org/ |

---

## 9. 関連ドキュメント

- [`android_business_app_framework_comparison.md`](./android_business_app_framework_comparison.md) — 総合比較、第 5 章に本ドキュメントのサマリあり
- [`framework_native_features.md`](./framework_native_features.md) — 各 FW のネイティブ機能プラグイン対応表
- [`selection_criteria.md`](./selection_criteria.md) — 言語・FW 選定観点(A〜H)
- [`profile_android_native.md`](./profile_android_native.md) / [`profile_flutter.md`](./profile_flutter.md) / [`profile_react_native.md`](./profile_react_native.md) / [`profile_ionic_capacitor.md`](./profile_ionic_capacitor.md) / [`profile_kmp.md`](./profile_kmp.md) — 各 FW 詳細プロファイル

---

## 更新履歴

- **2026-04-24**: 初版作成。15 カテゴリ 118 項目(うち HW 機能アクセスを 22 項目まで拡充:赤外線・シリアル・UWB・Zigbee/Thread/Matter・オーディオジャック・外部ディスプレイ・物理キー等を追加)。
