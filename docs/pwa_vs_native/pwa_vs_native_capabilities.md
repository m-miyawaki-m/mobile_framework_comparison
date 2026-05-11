# PWA / ハイブリッド / ネイティブ できる・できない 早見表

要件リストを照合して **足切り判断する** ためのページ。本文は機能カテゴリ別の ◎/△/× マトリクスのみ、根拠は他資料に委譲。

- 概観 + 選定ガイド: [`pwa_vs_native_overview.md`](./pwa_vs_native_overview.md)
- 詳細根拠(118 項目): [`pwa_vs_native_criteria.md`](./pwa_vs_native_criteria.md)
- 「なぜできないか」の原理: [`pwa_native_layer_boundary.md`](./pwa_native_layer_boundary.md)
- 情報基準日: 2026 年 5 月時点

凡例:
- **◎** 完全対応・公式 / 業界標準
- **○** 標準的に対応(若干の制約あり、または FW 経由)
- **△** 制約あり(プラットフォーム限定 / 機能限定 / 要自作)
- **×** 原理的に不可

---

## A. 配布・更新

| 機能 | PWA | ハイブリッド | ネイティブ |
|---|---|---|---|
| URL アクセス(SEO 流入)| **◎** | △ TWA で擬似的 | △ Deep Link |
| ホーム画面追加(A2HS)| ○(iOS で制約)| **◎ APK アイコン** | **◎** |
| Store 配布 | × | **◎ Play / App Store** | **◎ 同上** |
| MDM 配信(APK 直)| × | **◎** | **◎** |
| OTA 即時更新(Web 資産)| **◎ サーバ更新で即時** | ○ Live Update / Appflow | × Store 審査 |
| OTA ネイティブコード | — | × Store 審査必要 | × 同上 |
| Store 審査回避 | **◎ 不要** | △ 一部 Live Update 可 | × |
| 強制アップデート | △ キャッシュ制御 | ◎ In-App Update API | **◎** |
| 段階的ロールアウト | △ 自前 | ○ Plugin / Appflow | **◎ Play Staged Rollout** |

---

## B. HW 機能 — カメラ・センサ系

| 機能 | PWA | ハイブリッド | ネイティブ |
|---|---|---|---|
| カメラ撮影 | ◎ `getUserMedia` | **◎ `@capacitor/camera`** | **◎ CameraX** |
| マイク | ◎ | ◎ | ◎ |
| GPS / 位置情報 | ◎ Geolocation | ◎ | ◎ Fused Location |
| 加速度・ジャイロ | ◎ DeviceMotion | ◎ | ◎ SensorManager |
| 気圧・近接・光 | △(一部のみ Web 標準) | ◎ Plugin | ◎ |
| 生体認証(指紋・顔)| ○ WebAuthn(パスキー)| ◎ Plugin | ◎ Biometric API |
| 振動 | ◎ Vibration API | ◎ | ◎ Vibrator |

---

## C. HW 機能 — 通信系(BT / NFC / USB / Serial)

| 機能 | PWA | ハイブリッド | ネイティブ |
|---|---|---|---|
| BLE(汎用 GATT)| △ Chromium 限定、Safari 不可 | ◎ `@capacitor-community/bluetooth-le` | ◎ BluetoothLeScanner |
| **BT Classic SPP** | **×** | ◎ Plugin | ◎ BluetoothSocket |
| BLE Beacon 検知 | △ Chromium 部分対応 | ◎ Plugin | ◎ |
| **NFC NDEF 読み取り** | △ Chromium Android のみ | ◎ Plugin | ◎ android.nfc |
| **NFC HCE**(カードエミュ)| **×** | △ Plugin あるが品質ばらつき | ◎ HostApduService |
| Web USB(汎用)| △ Chromium 限定 | ◎ Plugin | ◎ UsbManager |
| Web Serial | △ Chrome デスクトップのみ | ◎ Plugin | ◎ USB Serial |
| **赤外線(IR Blaster)** | **×** | ○ カスタム Plugin | ◎ ConsumerIrManager |
| **Wi-Fi Direct** | **×** | ○ Plugin | ◎ |
| **UWB**(超広帯域)| **×** | △ カスタム Plugin | ◎ Android 13+ API |
| Zigbee / Thread / Matter | × | ○ ドングル + Plugin | ◎ ドングル + SDK |

---

## D. HW 機能 — 業務端末(HT スキャナ・プリンタ・RFID)

「業務 HT 系」と「外付け端末 SDK 連携」の早見。

| 機能 | PWA | ハイブリッド | ネイティブ |
|---|---|---|---|
| **DataWedge Keyboard Wedge**(`<input>` 受信)| **◎** | **◎** | **◎** |
| **DataWedge Intent API** | **×**(BroadcastReceiver 不可)| ◎ カスタム Plugin | ◎ BroadcastReceiver |
| Zebra EMDK(低レベル制御)| × | △ Plugin 自作大変 | ◎ |
| **ベンダー AAR / SDK**(各社 HT)| **×** | ◎ カスタム Plugin 化 | ◎ 直組込 |
| **UHF RFID**(外付け or HT 内蔵)| **×** | ◎ Plugin 化 | ◎ |
| プリンタ(Bluetooth / USB)| **×** | ◎ ベンダー Plugin | ◎ |
| プリンタ(ネットワーク)| ◎ HTTP / IPP | ◎ | ◎ |
| **物理ファンクションキー**(F1〜F12 等)| △ `keydown` のみ、取りこぼし | ○ カスタム Plugin | ◎ KeyEvent |
| HT 専用キーマッピング | × | ○ Plugin | ◎ |
| MSR(磁気カードリーダ)| × | ◎ Plugin | ◎ |

---

## E. ストレージ・オフライン

| 機能 | PWA | ハイブリッド | ネイティブ |
|---|---|---|---|
| LocalStorage / SessionStorage | ◎ | ◎ | — |
| IndexedDB | ◎(容量保証なし)| ◎ | — |
| **SQLite**(堅牢)| **×** | ◎ `@capacitor-community/sqlite` | ◎ Room |
| OPFS(ファイル直)| ○ Chromium 限定 | ◎ Filesystem Plugin | ◎ |
| Cache API(Service Worker)| ◎ | ◎(PWA モード時)| — |
| 暗号化ストレージ | △ WebCrypto 自前 | ◎ Secure Storage Plugin | ◎ Encrypted Prefs / Datastore |
| **HW Keystore(TEE / StrongBox)** | **×** | ◎ Plugin | ◎ Android Keystore |
| 容量上限(ブラウザ管理)| △ ブラウザが削除する可能性 | ◎ アプリ専用領域 | ◎ |

---

## F. 認証・セキュリティ

| 機能 | PWA | ハイブリッド | ネイティブ |
|---|---|---|---|
| OAuth / OIDC | ◎ | ◎ | ◎ |
| パスキー(WebAuthn)| ◎ | ◎ | ◎ |
| 生体認証(ローカル)| ○ WebAuthn | ◎ Biometric Plugin | ◎ Biometric API |
| **証明書ピン留め**(TLS Pinning)| **×**(ブラウザに不可)| ◎ Plugin / `@capacitor/http` | ◎ OkHttp / NetworkSecurityConfig |
| Play Integrity / DeviceCheck | × | ◎ Plugin | ◎ |
| ルート検出 | × | △ Plugin | ◎ |
| 画面録画防止 / スクショ防止 | × | ◎ Plugin(FLAG_SECURE)| ◎ FLAG_SECURE |

---

## G. バックグラウンド実行

| 機能 | PWA | ハイブリッド | ネイティブ |
|---|---|---|---|
| Service Worker(短時間 BG)| ◎(短時間)| ◎(PWA モード)| — |
| **Foreground Service**(8h 連続稼働)| **×** | ◎ Plugin | ◎ |
| **WorkManager**(堅牢 BG ジョブ)| **×** | ◎ Plugin | ◎ |
| Background Sync | △ Chromium 制約多 | ◎ Plugin | ◎ |
| プッシュ通知(FCM/APNS)| ◎(iOS 16.4+)| ◎ | ◎ |
| ローカル通知(時刻指定)| △ | ◎ | ◎ |
| Alarm Manager | × | ○ Plugin | ◎ |
| バッテリー効率 | △ WebView 常時 JS | △ 同上 | ◎ Doze と整合 |

---

## H. UX・プラットフォーム統合

| 機能 | PWA | ハイブリッド | ネイティブ |
|---|---|---|---|
| ネイティブルック | △ Ionic で近似 | ○ Ionic Web Components | **◎ ネイティブ標準** |
| スワイプ・ジェスチャー | △ Web ジェスチャー | ○ Ionic Gesture | **◎ OS 標準** |
| Pull-to-Refresh | ◎ Ionic | ◎ | ◎ |
| Safe Area / Notch 対応 | ○ env() | ◎ Ionic | ◎ |
| 暗色モード | ◎ prefers-color-scheme | ◎ | ◎ |
| **ホーム画面ウィジェット** | **×** | × | ◎(Glance / WidgetKit)|
| ショートカット(Long Press)| × | ○ Plugin | ◎ |
| 通知から起動 | ◎ Web Push | ◎ | ◎ |
| App-to-App 共有(Share Sheet)| ◎ Web Share | ◎ | ◎ |
| Deep Link / Universal Link | ◎ URL | ◎ Plugin | ◎ |

---

## I. エンタープライズ運用・MDM

| 機能 | PWA | ハイブリッド | ネイティブ |
|---|---|---|---|
| **MDM App Config 注入** | **×** | ◎ `@capawesome-team/capacitor-managed-configurations` | ◎ RestrictionsManager |
| **Device Owner 連携** | **×** | ○ カスタム Plugin | ◎ DevicePolicyManager |
| **Kiosk / Lock Task** | △ Chrome キオスク Only | ○ Plugin | ◎ startLockTask() |
| プロビジョニング(BYOD / COBO)| × | ○ | ◎ |
| ワークプロファイル(For Work)| △ ブラウザ単位 | ○ | ◎ |
| App Identity(MDM 配布識別)| × | ◎ Plugin | ◎ |
| リモートワイプ | × | ○ MDM 経由 | ◎ |

---

## J. ハードウェア構成 / ハードウェア依存

| 機能 | PWA | ハイブリッド | ネイティブ |
|---|---|---|---|
| カメラ複数台(背面・前面切替)| △ | ◎ | ◎ |
| カメラパラメータ(露出・WB)| △ Image Capture API 部分対応 | ◎ Plugin | ◎ Camera2 |
| カメラセンサ直アクセス(RAW)| × | ○ | ◎ |
| GPU シェーダ(WebGL/WebGPU)| ◎ | ◎(WebView)| ◎ Vulkan / OpenGL ES |
| Native OpenGL / Vulkan | × | × | ◎ |

---

## K. 開発・保守特性

| 観点 | PWA | ハイブリッド | ネイティブ |
|---|---|---|---|
| **既存 Web 資産流用** | ◎ | ◎ | × |
| **iOS 対応時 Mac 必須** | × 不要(ブラウザだけ)| ◎(Capacitor の iOS ビルド)| ◎ |
| Hot Reload | ◎ Vite HMR | ◎ Live Reload | △ Live Edit 制約多 |
| デバッガ | ◎ Chrome DevTools | ◎ DevTools(WebView 経由)+ Android Studio | ◎ Android Studio |
| LTS 整合(端末寿命 5〜7 年)| △ ブラウザ依存 | △ Capacitor 追従要 | ◎ Android LTS / iOS LTS |
| 商用 SLA | × | △ Ionic Enterprise | ○ ベンダー個別 |
| 国内人材プール | ◎(Web 系最大)| ◎ | ○(Kotlin / Swift)|

---

## L. PWA 足切り早見(1 つでも該当 → PWA 不可)

PWA でできない代表項目を 1 表に集約。**要件にこのいずれかが含まれた瞬間に PWA は除外候補**。

| カテゴリ | 機能 |
|---|---|
| 通信系 | BT Classic、NFC HCE、Wi-Fi Direct、UWB、Zigbee/Thread |
| HT・業務端末 | ベンダー AAR / SDK 全般、UHF RFID、DataWedge Intent、プリンタ(BT/USB)、IR Blaster、シリアル直叩き |
| ストレージ | SQLite 級堅牢 DB、HW Keystore(TEE / StrongBox)、容量上限保証 |
| BG 実行 | Foreground Service 常駐(8h)、WorkManager、Alarm Manager |
| セキュリティ | TLS 証明書ピン留め、Play Integrity、ルート検出、画面録画防止 |
| エンタープライズ | MDM App Config 注入、Device Owner、Kiosk(Chrome 以外)、ワークプロファイル、リモートワイプ |
| UX | ホーム画面ウィジェット |

→ 要件分析の最初のフェーズで上記を確認することで、無駄な PWA 検討を避けられる。

---

## M. ハイブリッド の主な制約(◎ が多いが弱点あり)

| 弱点 | 内容 | 緩和策 |
|---|---|---|
| WebView 性能上限 | 60fps 厳しい局面、複雑アニメで遅延 | 重い処理は Web Worker / Native Plugin |
| Bridge オーバーヘッド | 高頻度(< 10ms)呼出には不向き | Native 側で集約してから通知 |
| BG 長時間処理 | 純粋な WebView は弱い | Foreground Service Plugin で補強 |
| バッテリー消費 | WebView 常時 JS 実行で大 | requestIdleCallback / 不要再レンダ削減 |
| Plugin 品質ばらつき | 公式以外は当たり外れ | 公式 `@capacitor/*` + Capawesome を優先 |
| ストア審査 | Live Update でも審査範囲外あり | Web 資産のみの修正は OTA、ネイティブは Store |
| iOS WebView 制約 | WKWebView は一部 API 不動作 | Capacitor 公式互換情報を確認 |

---

## N. ネイティブ の制約(機能は ◎ だがコスト面)

| 弱点 | 内容 | 緩和策 |
|---|---|---|
| OS 別実装 | Android + iOS = 2 倍の作業 | KMP / Compose Multiplatform / Flutter で共通化 |
| Store 審査 | リリースサイクル数日〜週単位 | In-App Update API で強制プロモート |
| OTA に弱い | AOT 済 DEX のため Web 的な即時配信不可 | 設定値だけ Remote Config |
| Web 資産流用 | × | API 共通化のみ |
| 国内 Kotlin 人材は Web より薄い | チーム編成しにくい場合あり | Java 経験者を再教育 |

---

## O. 用語注記

- **「ハイブリッド」**: 本書では主に **Capacitor**(Ionic 系)を想定。React Native / Flutter / TWA / Chrome キオスク等の派生も広義のハイブリッドだが、能力プロファイルは Capacitor と概ね類似(個別差は [`pwa_vs_native_criteria.md`](./pwa_vs_native_criteria.md) XV 章参照)。
- **「ベンダー AAR / SDK」**: 業務端末ベンダー(Zebra / Honeywell / Datalogic / 各社 RFID リーダ製造元等)が配布する Android 用 SDK。Java/Kotlin の AAR 形式が一般的。ブラウザでは原理的にロードできない([`pwa_native_layer_boundary.md`](./pwa_native_layer_boundary.md))。

---

## 更新履歴

- **2026-05-11**: 初版作成。`pwa_vs_native_criteria.md` から能力比較部分を抽出・整理。
