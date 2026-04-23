# 詳細プロファイル: Ionic + Capacitor

`framework_profiles.md` の「Ionic Framework」「Capacitor」項目を **FW 単体の視点** から深掘りした詳細プロファイル。
本件(Android 業務アプリ / HT / jQuery+Java チーム / Windows 開発)での評価を付記する。
Ionic Framework(UI コンポーネント)と Capacitor(ネイティブランタイム)は実運用で常にセットのため統合して扱う。

- 情報は **2026 年 4 月時点**

---

## 0. 結論サマリ

| 項目 | 評価 |
|---|---|
| 総合推奨度(本件) | ★★★★☆(Vue 併用+Wedge モードなら本件第 2 候補) |
| 技術リスク | 低〜中(Ionic 社の存続性、WebView 性能制約) |
| 保守リスク | 中(Web エコシステムと Android/iOS SDK の両面を追う必要) |
| 学習曲線形状 | **初手楽(既存 Web スキル活用)→ 中盤フラット → 長期は WebView 境界制約で摩擦** |
| 立ち上げ期間(jQuery から) | **Vue 併用なら 2〜3 週、React 3〜5 ヶ月、Angular 3〜4 ヶ月** |
| **jQuery+Java チームとの相性** | **◎(最良、Vue なら最短ルート)** |
| Windows 開発 | ◎ Android 完全対応、iOS は Mac 必須 |
| AI/LLM 補完相性 | ◎ |
| 業務アプリ適性 | ◎(社内情報系・業務フォーム系で最強)/ ○(HT 用途では WebView 制約で一歩譲る) |

---

## 1. 基本情報

### 1.1 開発元・ガバナンス・Bus Factor

| 項目 | 内容 |
|---|---|
| **Ionic 開発元** | Ionic(旧 Drifty Co.、営利企業、資金調達済、CEO: Max Lynch) |
| 創業 | 2013 |
| Ionic Framework(OSS)| Ionic 社が主導、GitHub でオープン |
| **Capacitor 開発元** | Ionic 社(2018 年〜、Cordova の後継として開発) |
| ガバナンス | **単一企業主導**(Ionic 社) |
| Bus Factor | **中**(Ionic 社の企業存続性に依存、ただし Web 標準の上で動作するため FW 消失時の移行容易性は高い) |
| 商用モデル | Ionic Enterprise 契約(有償サポート + プレミアム機能) |

### 1.2 ライセンス

| 対象 | ライセンス | 商用制約 |
|---|---|---|
| Ionic Framework | **MIT** | なし |
| Capacitor | **MIT** | なし |
| Stencil.js(Ionic の内部実装)| MIT | なし |
| Ionic Appflow(CI/CD 商用)| 有償(Free/Starter/Basic/Enterprise) | プラン制限あり |
| Ionic Enterprise(商用ラッパ)| 有償 | Premier サポート、追加プラグイン |
| Capacitor プラグイン(@capacitor/\*)| MIT | なし |
| Capacitor Community / Capawesome | MIT(多い) | 各作者依存 |

### 1.3 バージョン履歴(主要マイルストーン)

| 年月 | バージョン | 主な変更 |
|---|---|---|
| 2013 | Ionic v1 | AngularJS + Cordova、モバイル Web UI の先駆 |
| 2016 | **Ionic 2** | Angular 2 対応、TypeScript 必須化 |
| 2018-04 | **Capacitor 1.0** | Cordova の後継として発表 |
| 2019-01 | **Ionic 4** | **Web Components 化**、Angular/React/Vue 対応に拡大 |
| 2020-04 | Capacitor 2.0 | Android 10 対応 |
| 2021-05 | Ionic 6 | Material Design 強化 |
| 2022-03 | Ionic 7 | UI コンポーネント刷新 |
| 2023-06 | Capacitor 5 | Android 13、iOS 16 対応 |
| 2024-04 | Capacitor 6 | Android 14、iOS 17 対応、新型 Plugins 仕組 |
| 2024 後半 | **Ionic 8** | Design Tokens、Accessibility 強化 |
| 2025 | Capacitor 7 | Android 15 対応 |
| **2026-04 時点** | Ionic 8.x、Capacitor 7.x 系(推定) | 継続的メンテ |

### 1.4 リリース・アップデートサイクル

| 対象 | 頻度 |
|---|---|
| Ionic Framework メジャー | **年 1 メジャー前後** |
| Ionic Framework マイナー/パッチ | 随時 |
| Capacitor メジャー | **半年〜年 1** |
| Capacitor マイナー/パッチ | 随時 |
| Capacitor プラグイン | 個別(公式は比較的安定) |

**業務アプリ観点**: Angular/React/Vue の更新サイクルが背景にあるので、それに追従する形。Ionic 本体の破壊的変更頻度は比較的少なめ。

### 1.5 LTS・サポート期限・EOL(F5)

| 項目 | 内容 |
|---|---|
| 明示 LTS | **Ionic Enterprise 契約で Extended LTS 提供** |
| Ionic Framework 通常サポート | 直近 1〜2 メジャー |
| Capacitor 通常サポート | 直近 1〜2 メジャー |
| EOL 通知 | 公式ブログ・リリースノート |

### 1.6 破壊的変更頻度・後方互換ポリシー(F6)

| 局面 | 変更影響度 |
|---|---|
| Ionic UI コンポーネント | 低〜中(命名変更や props 変更が各メジャーで発生) |
| Capacitor プラグイン API | 中(Android/iOS SDK 更新に連動) |
| Angular v14 → v17+ への追従 | 中(Angular 側の破壊的変更が流れる) |
| React / Vue | 低(比較的安定) |

### 1.7 脆弱性対応・CVE 体制(F7)

| 項目 | 内容 |
|---|---|
| Ionic 窓口 | security@ionic.io |
| Capacitor 窓口 | 同上 |
| 公開フロー | GitHub Security Advisories |
| 対応速度 | 迅速 |
| npm 依存脆弱性 | **npm audit / Dependabot で自動検出**、WebView ベースのため Web アプリ同様 |

### 1.8 商用サポート(F8)

| プラン | 内容 |
|---|---|
| **Ionic Enterprise** | Premier サポート、Identity Vault、Auth Connect、Offline Storage、AppFlow 連携 |
| **Ionic Appflow** | CI/CD、Live Update、ビルド配信 |
| SI パートナー | 国内外に存在 |
| JetBrains 商用 IDE | IntelliJ IDEA Ultimate(+ WebStorm)で Ionic/Angular/React/Vue サポート |

---

## 2. フレームワーク特性

### 2.1 アーキテクチャ

```
┌─────────────────────────────────────────────┐
│  アプリケーションコード                      │
│  (Angular / React / Vue / Vanilla TS)      │
├─────────────────────────────────────────────┤
│  Ionic Framework(UI コンポーネント)         │
│  - Web Components(Stencil.js 実装)          │
│  - Ion-Button, Ion-Card, Ion-Tabs...        │
├─────────────────────────────────────────────┤
│  Capacitor(ネイティブランタイム)            │
│  - Bridge(JS ↔ Native、非同期 IPC)         │
│  - Plugin API(@capacitor/camera 等)         │
├─────────────────────────────────────────────┤
│  WebView                                    │
│  - Android: Chrome-based System WebView     │
│  - iOS: WKWebView                           │
│  - Desktop: Electron(任意)                 │
├─────────────────────────────────────────────┤
│  ネイティブホスト(Android/iOS/Electron)     │
└─────────────────────────────────────────────┘
```

### 2.2 Ionic Framework(UI コンポーネント)

| 特性 | 内容 |
|---|---|
| 実装 | **Stencil.js**(Ionic 社製 Web Components コンパイラ) |
| 配布形態 | **Framework Agnostic な Web Components**、各 JS FW 用のラッパが @ionic/angular / @ionic/react / @ionic/vue で提供 |
| デザインシステム | Material(Android)/ iOS(Cupertino)両方のネイティブルック自動切替 |
| コンポーネント数 | 100+(Ion-Button、Ion-Card、Ion-Modal、Ion-Nav、Ion-Menu、Ion-Refresher、Ion-Toast 等) |
| アクセシビリティ | **◎(Web 標準 a11y がそのまま使える)** |
| カスタマイズ | CSS 変数、CSS Parts、Design Tokens |

### 2.3 Capacitor(ネイティブランタイム)

| 特性 | 内容 |
|---|---|
| 役割 | Web アプリをネイティブ APK/IPA にパッケージ化、ネイティブ API を JS から叩くブリッジ |
| Bridge | 非同期 JSON メッセージング |
| Plugin 実装 | JS + Android(Kotlin/Java)+ iOS(Swift/Objective-C) |
| 公式プラグイン | @capacitor/\*(Camera、Geolocation、Filesystem、Network、Preferences 等 25+) |
| コミュニティ | Capacitor Community、Capawesome、Ionic 外部プラグイン多数 |
| Cordova との関係 | Cordova プラグインも一部互換(徐々に減少) |
| Electron サポート | あり(@capacitor-community/electron) |

### 2.4 JS フレームワーク統合

| FW | 統合 | 特徴 |
|---|---|---|
| **Vue 3** | @ionic/vue | 本件推奨(jQuery 移行最スムーズ、テンプレート構文) |
| **React** | @ionic/react | Hooks 対応、React Navigation と Ionic Router 連携 |
| **Angular** | @ionic/angular | Ionic の歴史的本家、最も成熟、大規模業務向け |
| Vanilla TS / Stencil | 可 | 軽量プロジェクト向け |

### 2.5 Stencil.js との関係

Ionic コンポーネントは **Stencil.js**(Ionic 社製 Web Components コンパイラ)で実装されている。Stencil は React/Vue/Angular へのバインディングも自動生成するため、JS FW を問わず同じ UI が使える。

### 2.6 UI 描画方式

| 方式 | 内容 |
|---|---|
| WebView レンダリング | ブラウザエンジンが全画面を描画 |
| Platform 切替 | `mode="ios"` / `mode="md"` で自動、または明示指定 |
| カスタムテーマ | CSS 変数で簡単に統一/分離 |
| アニメーション | Ionic Animations(Web Animations API ベース) |

### 2.7 並行・非同期モデル

| 機構 | 用途 |
|---|---|
| `Promise` / `async-await` | 標準 |
| Observable / RxJS | Angular 併用時、HTTP レスポンスやイベントストリーム |
| Service Worker | PWA 対応、オフラインキャッシュ |
| Web Workers | CPU 重い処理のオフロード |

### 2.8 エラーハンドリング

| 機構 | 使い所 |
|---|---|
| `try/catch` | 主流 |
| Global Error Handler(Angular の ErrorHandler、React ErrorBoundary) | グローバル捕捉 |
| Sentry | 本番ランタイム |
| Capacitor Plugin Exception | Native 例外を `rejectedPromise` として受信 |

### 2.9 FFI・ネイティブ連携(H6)

| 手段 | 内容 |
|---|---|
| **Capacitor 公式プラグイン** | 基本機能、安定度高 |
| **Capacitor コミュニティプラグイン** | カメラ拡張、BLE、NFC、ZPL 印刷 等 |
| **カスタムプラグイン自作** | Android(Kotlin)/ iOS(Swift)で実装、Plugin API に沿う |
| **Cordova プラグイン** | 互換層で一部動作(徐々に非推奨) |
| **Web API 直接利用** | カメラ(getUserMedia)、位置情報(Geolocation API)、NFC(Web NFC、Chromium Android のみ) |

### 2.10 設計哲学(H17)

| 原則 | 意味 |
|---|---|
| **Web First** | Web 標準の上で動き、Web 資産を最大活用 |
| **Write Once, Run Anywhere**(Web/モバイル/PWA/Desktop) | 1 コードベースで広いターゲット |
| **Framework Agnostic** | Angular/React/Vue 自由選択 |
| **Developer Experience** | Web 開発者の既存知識をそのまま活用 |

---

## 3. 開発環境・ツーリング

### 3.1 CLI・SDK

| コマンド | 用途 |
|---|---|
| `npm init @ionic` / `ionic start` | プロジェクト生成 |
| `ionic serve` | ブラウザ上で開発 |
| `ionic capacitor add android / ios` | ネイティブプロジェクト追加 |
| `ionic capacitor run android` | 実機/エミュで実行 |
| `ionic build` | Web ビルド |
| `npx cap sync` | Web ↔ Native 同期 |
| `npx cap open android/ios` | Android Studio / Xcode 起動 |

### 3.2 IDE

| IDE | 評価 |
|---|---|
| **VS Code + Ionic extension** | ◎ 主流、軽量 |
| **WebStorm / IntelliJ IDEA Ultimate** | ◎ Angular 併用時は特に強力 |
| Android Studio | ネイティブビルド時に併用 |
| Xcode | iOS ビルド時(Mac) |

### 3.3 LSP・補完品質(H8)

- TypeScript Language Server
- Angular Language Service(Angular 併用時、テンプレート補完)
- Volar(Vue 併用時)
- VS Code / WebStorm で強力な補完・リファクタ

### 3.4 ビルド・依存管理

| 項目 | 内容 |
|---|---|
| Web ビルド | **Vite**(新規推奨) / Webpack(旧)/ Angular CLI(Angular 併用時) |
| ネイティブビルド | `@capacitor/cli` → Gradle(Android)/ CocoaPods + Xcode(iOS) |
| パッケージマネージャ | **npm / yarn / pnpm** |
| Lock file | 各 package-lock / yarn.lock / pnpm-lock |

### 3.5 Lint / Formatter

| ツール | 役割 |
|---|---|
| **ESLint** + @ionic/eslint-config | |
| **Prettier** | |
| Stylelint | CSS/SCSS |
| TypeScript strict | |
| Angular-specific ルール | @angular-eslint |
| Vue-specific ルール | eslint-plugin-vue |
| React-specific ルール | eslint-plugin-react |

### 3.6 デバッガ・プロファイラ

| ツール | 用途 |
|---|---|
| **Chrome DevTools**(Android WebView リモート接続) | **主力**、ブレークポイント、Network、Performance、Memory |
| **Safari Web Inspector**(iOS WebView) | 同上 |
| **Ionic Framework Inspector**(ブラウザ) | UI コンポーネント検査 |
| Sentry / Firebase Crashlytics | 本番 |
| Android Studio / Xcode | ネイティブ層デバッグ |

**スタックトレース可読性(H7)**: JS/TS スタックトレースは Chrome DevTools で読みやすい。ソースマップが生成されていれば TS ソースでブレーク可。

### 3.7 Hot Reload / Live Reload

| 機能 | 内容 |
|---|---|
| **`ionic serve`**(ブラウザ) | **最速 Hot Reload**、Vite HMR で即時反映 |
| **Ionic Live Reload**(実機) | `ionic capacitor run --livereload` で実機上の WebView に即時反映 |
| ネイティブコード変更時 | 再ビルド必要 |

**業務アプリ観点**: ブラウザでの開発フィードバックは Ionic の強み。画面開発の大半をブラウザで完結、仕上げを実機で確認する流れが効率的。

### 3.8 コンパイル速度(H13)

| 局面 | 体感 |
|---|---|
| Vite HMR(ブラウザ)| **瞬時**(数百ミリ秒) |
| 初回 Web ビルド | 数十秒 |
| 初回 Android ビルド(Gradle) | 数分 |
| 初回 iOS ビルド(Xcode) | 数分 |
| Capacitor sync | 数秒 |

### 3.9 Windows 開発環境

| 項目 | 内容 |
|---|---|
| Node.js + Ionic CLI + Capacitor CLI | ◎ 完全対応 |
| Android ビルド | ◎ 完全対応 |
| iOS ビルド | **Mac 必須**(Codemagic、GitHub Actions macOS runner、MacinCloud 等で代替) |
| Windows Desktop(Electron) | ◎ |
| PWA(ブラウザで動作確認)| ◎ |
| ディスク占有 | 5〜15 GB(Ionic + Capacitor + Android SDK) |

**本件強み**: Windows 上で業務アプリ開発の大半が完結。既存 Web 開発環境(Node.js、VS Code)をそのまま使える。

---

## 4. パフォーマンス特性

### 4.1 起動時間

| 局面 | 体感 |
|---|---|
| Cold start | **WebView 初期化コストあり**(数百ミリ秒〜1 秒台)、非力 HT では体感明確 |
| Warm start | 即時 |
| 画面遷移 | ページ切替は高速(Ionic のキャッシュ機構) |

### 4.2 スループット

| 指標 | 値 |
|---|---|
| UI レンダリング | ブラウザエンジン依存、Chrome ベースで一定水準 |
| JS ↔ Native 呼び出し | **Bridge 経由の非同期 IPC**(RN 新アーキ比で遅い) |
| 重いアニメ/3D | WebGL 等で可能だが、Flutter/ネイティブ比で劣る |
| フォーム操作 | ◎(Web と同じで快適) |

### 4.3 メモリフットプリント

| 項目 | 値 |
|---|---|
| WebView 自体 | 数十 MB〜(Android の System WebView に依存) |
| JS ヒープ | アプリ複雑度依存 |
| ネイティブ側 | Capacitor ホストのみ数 MB |

### 4.4 APK / アプリサイズ(H14)

| 項目 | サイズ |
|---|---|
| Capacitor ホスト APK | **数 MB**(最小) |
| Web アセット | 数 MB(バンドルサイズ依存) |
| 典型的アプリ | **5〜15 MB**(Flutter より小さい) |

### 4.5 バッテリー・連続稼働

| 観点 | 評価 |
|---|---|
| バッテリー効率 | **△**(WebView 常時 JS 実行でネイティブ比で消費大) |
| 8 時間連続稼働 | 要チューニング、Flutter/ネイティブ比で不利 |
| バックグラウンド | **WebView 制約で限定的**(Service Worker は Web 同等) |

---

## 5. エコシステム・コミュニティ

### 5.1 パッケージ規模

| レジストリ | 規模 |
|---|---|
| **npm**(全体) | 数百万(最大) |
| **@ionic/\***(公式) | 10+ パッケージ |
| **@capacitor/\***(公式) | 25+ プラグイン |
| **Capacitor Community** | 50+ プラグイン |
| **Capawesome** | 企業向けプラグイン集 |

### 5.2 GitHub 定量指標(H11、2026-04 時点目安)

| リポジトリ | Stars(概算) |
|---|---|
| `ionic-team/ionic-framework` | **約 50k** |
| `ionic-team/capacitor` | 約 12k |
| `ionic-team/stencil` | 約 13k |

### 5.3 年次調査

- Stack Overflow Developer Survey: Web 系 FW の一部として言及、モバイル FW 単独カテゴリでは Flutter/RN に次ぐ位置
- JS 人材市場: React/Angular/Vue の汎用スキルが使えるため、採用容易

### 5.4 日本語リソース(H12)

| 種別 | 代表 |
|---|---|
| 書籍(日本語) | 『Ionic ではじめるスマホアプリ開発』他(やや古め)、技術評論社系 |
| 日本語ブログ | Qiita / Zenn に豊富(とくに Angular+Ionic の業務系記事) |
| 国内コミュニティ | Ionic Japan User Group(connpass)、各地 Angular/React/Vue Meetup |
| 採用事例 | 社内業務アプリ・情報系で多数、BtoB の小〜中規模モバイルで採用増 |

### 5.5 認定・トレーニング

- **Ionic Academy**(公式、英語)
- Ionic 公式ドキュメント・チュートリアル
- Udemy に多数のコース

---

## 6. 業務アプリ視点(HT 特化)

### 6.1 HT スキャナ連携(最重要観点)

| 機能 | 実装 |
|---|---|
| **DataWedge Keyboard Wedge** | `<input>` にフォーカスがあれば受信、**実装不要**、**Ionic の得意分野** |
| **DataWedge Intent API** | **Capacitor Intent プラグイン自作**(Android Native Module 追加)、またはコミュニティプラグイン探索 |
| Zebra EMDK | 非推奨(WebView 経由での低レベル制御は現実的でない) |
| Honeywell / Datalogic | カスタム Capacitor プラグイン自作 |
| 物理ファンクションキー | **WebView 境界で取りこぼしあり**、Native 層で受信→WebView に通知する実装が必要 |

**業務アプリ観点**: **Keyboard Wedge モードで済む業務**(スキャン=単純テキスト入力)なら Ionic は最短実装。詳細制御が必要な HT 案件では Flutter/ネイティブに劣後。

### 6.2 MDM 連携

| 機能 | 代表実装 |
|---|---|
| **App Config**(Managed Configuration) | **`@capawesome-team/capacitor-managed-configurations`** |
| Kiosk / Lock Task | カスタム Capacitor プラグイン(Native 側で実装) |
| Device Owner | 同上 |
| App Identity(MDM ID 取得)| Capawesome 等のプラグイン |

### 6.3 オフライン・同期

| 機能 | 代表ライブラリ |
|---|---|
| SQLite | **`@capacitor-community/sqlite`**、SQLCipher オプションあり |
| KVS | **`@capacitor/preferences`** |
| IndexedDB | Web 標準、PWA と共通 |
| バックグラウンド同期 | **`@capacitor/background-task`**(短時間のみ)、`@capawesome/capacitor-background-task` |
| 通信 | fetch / axios / `@capacitor/http`(プラットフォーム固有ヘッダ回避) |
| Service Worker | PWA と共通、Offline Cache 可 |

**業務アプリ観点**: **堅牢なオフラインキュー + 長時間バックグラウンド同期が要件なら Ionic は不利**。Flutter / ネイティブの WorkManager 直結が圧倒的に強い。

### 6.4 セキュリティ

| 機能 | 代表ライブラリ |
|---|---|
| セキュアストレージ | **`@ionic-enterprise/identity-vault`**(商用)/ `@capacitor-community/secure-storage` |
| 生体認証 | `@capacitor-community/biometric-auth`、`@aparajita/capacitor-biometric-auth` |
| 証明書ピン留め | カスタムプラグイン、または `@capacitor/http` の設定 |
| 暗号化 | WebCrypto API(Web 標準)、SQLCipher(DB 暗号化) |

### 6.5 カメラ・スキャン

| 機能 | 代表ライブラリ |
|---|---|
| カメラ | **`@capacitor/camera`** |
| QR/バーコード | **`@capacitor-mlkit/barcode-scanning`**(ML Kit ラッパ) |
| OCR | `@capacitor-mlkit/text-recognition` |
| Web NFC(Chromium Android) | Web 標準 API、限定的 |

### 6.6 プッシュ通知・クラッシュ収集

| 機能 | 代表ライブラリ |
|---|---|
| FCM/APNS | **`@capacitor/push-notifications`**(公式) |
| ローカル通知 | `@capacitor/local-notifications` |
| Sentry | **`@sentry/capacitor`** |
| Crashlytics | `@capacitor-community/firebase-crashlytics` |

### 6.7 OTA(Live Update)

| 機能 | 内容 |
|---|---|
| **Ionic Appflow Live Updates**(商用) | ストア審査なしに Web アセット即時更新 |
| 自作 OTA | Web アセットをサーバから配信 → Capacitor で切替 |
| 制約 | ネイティブコード変更はストア審査必要 |

**業務アプリ観点**: **Ionic の最大の強みの 1 つ**。社内配信・緊急修正展開で優位。

### 6.8 PWA 同時展開(他 FW に真似できない価値)

| 観点 | 内容 |
|---|---|
| PWA 化 | **1 コードベースで PWA・Android APK・iOS IPA を同時展開可** |
| A2HS(Add to Home Screen)| Web 標準対応 |
| Service Worker キャッシュ | Web 標準、Workbox 推奨 |
| Chrome キオスク構成 | PWA を Chrome キオスクで業務端末に固定展開可 |

---

## 7. 主要採用事例(H9)

### 7.1 グローバル

| 企業 | 用途 |
|---|---|
| **BBC Sounds** | 音声ストリーミング |
| **Burger King** | 注文アプリ |
| **T-Mobile** | 顧客アプリ |
| **Southwest Airlines** | 社内業務 |
| **MarketWatch** | 金融情報 |
| **Diesel** | EC |
| **Untappd** | SNS |
| **Nationwide**(米保険)| モバイル |
| **Instant Pot** | 家電連携 |

### 7.2 国内

| 企業 | 用途 |
|---|---|
| 社内業務アプリ、情報系 BtoB モバイル | 多数(特に Angular+Ionic 採用が歴史的に多い) |
| SIer 受託案件 | 多数 |

### 7.3 ドメイン別

| ドメイン | 採用実績 |
|---|---|
| **社内業務アプリ(申請/承認/閲覧)** | ★★★★★(既存 Web 画面流用、PWA 併用) |
| **社内情報系 BtoB モバイル** | ★★★★★ |
| 金融(顧客アプリ) | ★★★★☆(Wedge 入力中心なら◎) |
| 小売・EC | ★★★★☆ |
| 医療(院内系) | ★★★★☆ |
| **HT 業務**(Wedge モード)| ★★★★☆(Intent 不要なら◎) |
| **HT 業務**(Intent/高機能)| ★★★☆☆(Native 実装負担大) |
| 高負荷 UI / 3D / ゲーム | ★★☆☆☆(WebView 性能制約) |
| 長時間稼働バックグラウンド | ★★☆☆☆(Service Worker 制約) |

---

## 8. AI/LLM 補完相性(H10)

| 観点 | 評価 |
|---|---|
| 学習データ量 | **◎ 最大**(JS/TS + Angular/React/Vue + Web 全般の膨大な学習資産) |
| 型情報活用 | ◎ TypeScript strict で補完精度高 |
| Ionic コンポーネント補完 | ○ Web Components で属性補完は十分 |
| GitHub Copilot | ◎ 最高水準(Web FW 共通の精度) |
| ChatGPT / Claude | ◎ Angular/React/Vue 質問への回答精度最高 |
| Capacitor 特有コード | ○ 公式ドキュメント量に依存 |

---

## 9. 学習パス(H18)

### 9.1 学習曲線の形状

```
難易度
  │                __________  エキスパート(6ヶ月〜)
  │               /             Capacitor カスタムプラグイン、Appflow、大規模構成
  │          ____/
  │         /                    実務(1〜2ヶ月)
  │     ___/                     Ionic コンポーネント、ルーティング、状態管理
  │    /                         
  │   /                          基礎(2〜3週、jQuery から Vue 経由)
  │  /                           TS + Vue + Ionic UI
  │ /                            
  └──────────────────────→ 時間
```

**形状の特徴**:

- **初手楽**(Web スキル直接活用、jQuery+Java チームに最も優しい)
- **中盤フラット**(モダン Web と同じパターン)
- **長期はネイティブ境界の壁**(Capacitor プラグイン自作、Android/iOS 知識要)

### 9.2 本件推奨学習パス(jQuery+Java から、Vue+Ionic、2〜3 週)

| 週 | テーマ |
|---|---|
| 1 | TypeScript 基礎(Java からの差分、interface、generics) + モダン JS(ES2020+) |
| 2 | Vue 3 基礎(リアクティブ、SFC、Composition API、Directives)+ Vue Router + Pinia |
| 3 | Ionic コンポーネント(Ion-\*)、Capacitor セットアップ、API 連携、実機ビルド |

### 9.3 React+Ionic / Angular+Ionic の場合

| FW | 立ち上げ |
|---|---|
| React+Ionic | **3〜5 ヶ月**(Hooks/関数型のパラダイム転換大) |
| Angular+Ionic | **3〜4 ヶ月**(DI/RxJS/Reactive Forms の学習コスト) |

### 9.4 推奨教材

| カテゴリ | 教材 |
|---|---|
| 公式 | [Ionic Framework Docs](https://ionicframework.com/docs)、[Capacitor Docs](https://capacitorjs.com/docs) |
| 公式チュートリアル | Ionic Academy(英語、有償あり) |
| Vue 併用 | [Vue 3 公式ガイド](https://ja.vuejs.org/)、Pinia、Vue Router |
| 書籍(日本語) | 『Ionic ではじめる〜』他(やや古め、公式最新を併用) |
| 動画 | Simon Grimm Academy、Ionic 公式 YouTube |

---

## 10. 落とし穴・バッドパターン

| # | 落とし穴 | 対策 |
|---|---|---|
| 1 | WebView の性能を過小評価 | 初期に性能要件を確認、高負荷 UI は別 FW を検討 |
| 2 | Cordova プラグインと Capacitor プラグインの混在 | Capacitor に統一、Cordova 互換層は最小限に |
| 3 | ネイティブ機能が必要になり Ionic から離脱 | 最初に必要機能のプラグイン有無を確認 |
| 4 | Angular Reactive Forms を 20 画面で過剰採用 | 小規模では Template-driven Forms で十分 |
| 5 | Capacitor プラグインの品質ばらつき | **公式(@capacitor/\*)を優先、コミュニティは GitHub 活動度を確認** |
| 6 | iOS の WKWebView で一部 API 不動作 | Capacitor 公式の互換情報を確認、代替 API 検討 |
| 7 | Service Worker と Capacitor のキャッシュ競合 | Service Worker は Web モード(PWA)時のみ有効化 |
| 8 | Live Reload 実機が繋がらない | 同一 Wi-Fi、ファイアウォール、IP 明示 |
| 9 | バッテリー消費が大きい指摘 | `requestIdleCallback`、アニメ最適化、不要な再レンダリング削減 |
| 10 | 物理キーの取りこぼし | Native Module を作成、Broadcast Intent で WebView に通知 |
| 11 | Angular v14 → v17+ の更新追従不足 | 四半期ごとに `ng update` チェック |
| 12 | Ionic Enterprise 機能を OSS で使おうとする | 業務要件で必要なら Enterprise 契約を前提に |

---

## 11. 業務アプリでの適性評価

### 11.1 本件(Android HT + 20 画面 + jQuery+Java + Android 永続)

| 観点 | 評価 | コメント |
|---|---|---|
| 言語習熟難易度(Vue 併用)| ★★★★★ | jQuery → Vue テンプレート移行最短 |
| 既存 jQuery 資産流用 | ★★★★☆ | HTML/CSS はそのまま、JS ロジックは段階移植可 |
| HT Wedge モード対応 | ★★★★★ | 実装不要 |
| HT Intent API 対応 | ★★★☆☆ | カスタムプラグイン自作要 |
| 20 画面規模 | ★★★★☆ | Vue のスイートスポット |
| Android 永続 | ★★★★☆ | iOS 共通化の利点は無関係(元々 Web+モバイル共通) |
| Windows 開発 | ★★★★★ | 完全対応 |
| 長時間稼働 | ★★★☆☆ | WebView 制約あり、8h 稼働はチューニング必要 |
| BG バッテリー | ★★★☆☆ | 同上 |
| 長期保守 | ★★★★☆ | Web エコの健全性に依存 |
| 国内人材 | ★★★★★ | Web 系人材豊富 |
| **総合** | **★★★★☆** | **Wedge 中心+中負荷 UI なら本件第 2 候補** |

### 11.2 業務アプリ一般

| ドメイン | 推奨度 |
|---|---|
| 社内業務・情報系モバイル | ★★★★★(最強) |
| 社内基幹の大規模業務 | ★★★★☆(Angular+Ionic) |
| 既存 Web アプリのモバイル化 | ★★★★★(最短) |
| PWA + モバイル同時展開 | ★★★★★(他 FW では困難) |
| HT 業務(Wedge 限定) | ★★★★☆ |
| HT 業務(Intent 必要) | ★★★☆☆ |
| 高負荷 UI / 3D / 重アニメ | ★★☆☆☆ |
| 長時間 BG 稼働 | ★★☆☆☆ |

---

## 12. ロードマップ(H15)

| 中期テーマ | 内容 |
|---|---|
| **Ionic 9 / Capacitor 8**(想定) | Android 16+ / iOS 18+ 対応 |
| Ionic Portals | マイクロフロントエンド(ネイティブアプリに Ionic 部分を埋込) |
| Ionic Appflow 機能拡充 | Live Update の高度化 |
| Capacitor 新プラグイン API | 生産性向上 |
| AI 統合 | Ionic Academy AI アシスト |
| Web Components 標準追従 | Shadow DOM、CSS @layer 等 |

公式: https://ionicframework.com/blog

---

## 13. 参照リンク

### 13.1 公式

| 対象 | URL |
|---|---|
| Ionic Framework 公式 | https://ionicframework.com/ |
| Ionic Docs | https://ionicframework.com/docs |
| Capacitor 公式 | https://capacitorjs.com/ |
| Capacitor Docs | https://capacitorjs.com/docs |
| Ionic ブログ | https://ionic.io/blog |
| Ionic Enterprise / Appflow 価格 | https://ionic.io/pricing |
| Stencil.js(Ionic 内部実装)| https://stenciljs.com/ |

### 13.2 エコシステム・プラグイン

| 対象 | URL |
|---|---|
| Capacitor 公式プラグイン | https://capacitorjs.com/docs/plugins |
| Capacitor Community | https://github.com/capacitor-community |
| **Capawesome**(企業向け) | https://capawesome.io/ |
| @capacitor-mlkit/\* | https://github.com/capawesome-team/capacitor-mlkit |

### 13.3 業務・HT 向け

| 対象 | URL |
|---|---|
| Capacitor Managed Configurations | https://github.com/capawesome-team/capacitor-managed-configurations |
| @capacitor-community/sqlite | https://github.com/capacitor-community/sqlite |
| @capacitor-community/firebase-crashlytics | https://github.com/capacitor-community/firebase-crashlytics |
| @sentry/capacitor | https://docs.sentry.io/platforms/javascript/guides/capacitor/ |

### 13.4 開発環境・ツール

| 対象 | URL |
|---|---|
| VS Code Ionic Extension | https://marketplace.visualstudio.com/items?itemName=ionic.ionic |
| Ionic CLI | https://ionicframework.com/docs/cli |
| Capacitor CLI | https://capacitorjs.com/docs/cli |
| Ionic Appflow | https://ionic.io/appflow |

### 13.5 コミュニティ・学習

| 対象 | URL |
|---|---|
| **Ionic Academy**(公式有償コース)| https://ionicacademy.com/ |
| Ionic Blog | https://ionic.io/blog |
| Ionic Forum | https://forum.ionicframework.com/ |
| Ionic Japan User Group(connpass)| https://ionic-jp.connpass.com/ |
| Ionitron(Ionic YouTube)| 公式 YouTube |

### 13.6 セキュリティ

| 対象 | URL |
|---|---|
| Ionic Security | security@ionic.io |
| GitHub Security Advisories(Ionic) | https://github.com/ionic-team/ionic-framework/security/advisories |
| GitHub Security Advisories(Capacitor) | https://github.com/ionic-team/capacitor/security/advisories |

---

## 14. 本ファイルの位置付け

- FW 詳細プロファイル(profile_flutter.md / profile_react_native.md / profile_android_native.md と同粒度)
- 本件第 2 候補(Vue+Ionic+Capacitor の組合せ)の詳細根拠
- 次は `profile_kmp.md` 予定

---

## 更新履歴

- **2026-04-23**: 初版作成。FW 詳細プロファイル(Ionic + Capacitor)。
