# 詳細プロファイル: React Native + Expo

`framework_profiles.md` の「React Native」「Expo」項目を **FW 単体の視点** から深掘りした詳細プロファイル。
本件(Android 業務アプリ / HT / jQuery+Java チーム / Windows 開発)での評価を付記する。
Expo は RN 上の統合プラットフォームとして一体で扱う(2026 年時点で新規 RN プロジェクトはほぼ Expo ベース)。

- 情報は **2026 年 4 月時点**

---

## 0. 結論サマリ

| 項目 | 評価 |
|---|---|
| 総合推奨度(本件) | ★★★☆☆(jQuery+Java チームにはパラダイム転換大、Android 永続で優位性低下) |
| 技術リスク | 中(Meta 戦略依存 + サードパーティ依存) |
| 保守リスク | 中〜高(破壊的変更頻度中〜高、パッケージ互換性負担) |
| 学習曲線形状 | **初手中〜急(Hooks + モバイル境界)→ 中盤スロープ → 長期はパッケージ管理力が効く** |
| 立ち上げ期間(Java/jQueryから) | 4〜6 ヶ月 |
| Expo の有無 | **Expo 採用でほぼ全観点が劇的に改善**(セットアップ・iOS ビルド・OTA) |
| Windows で iOS ビルド | **◎(Expo EAS Build のみ唯一実用的に Mac 不要)** |
| AI/LLM 補完相性 | ◎(学習データ量最大) |

---

## 1. 基本情報

### 1.1 開発元・ガバナンス・Bus Factor

| 項目 | 内容 |
|---|---|
| React Native 開発元 | **Meta**(Facebook、旧 Facebook Open Source)+ **Microsoft**(Windows/macOS 拡張)+ **Callstack**(OSS サポート)+ **Expo**(エコシステム) |
| 発案 | 2015-03 |
| ガバナンス | Meta 主導 + コミュニティ寄与(React Native Open Source Program)|
| Expo 開発元 | **Expo, Inc.**(Palo Alto、資金調達済) |
| Bus Factor | **中〜高**(Meta + MS + Expo の複数社体制、Meta 単独リスクは軽減) |
| Expo Bus Factor | 中(単一企業だが資金調達企業で活発) |

**業務アプリ観点**: Meta + MS の協調体制で Google 単独の Flutter より多元的。Meta 社内では Messenger / Ads / Marketplace が RN で稼働、戦略継続性は高い。

### 1.2 ライセンス

| 対象 | ライセンス | 商用制約 |
|---|---|---|
| React Native | **MIT** | なし(旧 Meta 特許条項は 2017 に撤廃済) |
| Expo SDK(OSS) | MIT | なし |
| **EAS Build / Update / Submit** | Expo Inc. 有償プラン | Free / Production / Enterprise プラン |
| React | MIT | なし |
| Metro(bundler) | MIT | なし |
| Hermes(JS エンジン) | MIT | なし |

**業務アプリ観点**: Expo EAS の料金構造を採用前に確認。Free プランはビルド時間に上限。中規模商用はおおむね Production プラン推奨。

### 1.3 バージョン履歴(主要マイルストーン)

| 年月 | バージョン | 主な変更 |
|---|---|---|
| 2015-03 | v0.1 | Meta が OSS 公開 |
| 2018 | 0.59 | 安定化、Redux 全盛期 |
| 2019 | 0.60 | AndroidX 対応、自動リンク(autolinking) |
| 2020 | 0.62 | **Flipper 統合**(後に RN 0.74+ で非推奨化)|
| 2020 | 0.63 | Hermes Android デフォルト候補 |
| 2022-06 | 0.69 | React 18 対応、**Hermes デフォルト化** |
| 2023-01 | 0.71 | TypeScript デフォルト |
| 2023-10 | 0.73 | Android 14 対応、新アーキ安定寄り |
| 2024-04 | 0.74 | **Bridgeless Mode**、Yoga 3 |
| 2024-08 | 0.75 | 新アーキ強化 |
| 2024-11 | **0.76** | **新アーキ デフォルト化**(JSI/Fabric/TurboModules/Bridgeless) |
| 2025 | 0.77〜0.79 | 旧ブリッジ削除に向けた漸進 |
| **2026-04 時点** | 0.8x 系(推定) | 新アーキ完全移行、パフォーマンス成熟 |

### 1.4 Expo SDK 履歴

| 年月 | SDK | 主な変更 |
|---|---|---|
| 2015 | 1 | 初期 |
| 2020 | SDK 40 台 | Managed Workflow |
| 2021 | **EAS Build / Update GA** | クラウドビルド・OTA 公式化 |
| 2023 | SDK 49〜51 | Expo Router(File-based)|
| 2024 | SDK 52 | React Native 0.76 対応 |
| 2025 | SDK 53〜55(推定) | 新アーキ完全統合 |
| **2026-04 時点** | SDK 5x 系 | 四半期リリース継続 |

### 1.5 リリース・アップデートサイクル

| 対象 | 頻度 |
|---|---|
| React Native マイナー(0.7x) | **2〜3ヶ月** |
| React Native patch | 随時 |
| Expo SDK | **四半期**(3ヶ月) |
| Expo Go / Dev Client | 随時 |
| React | 不定期メジャー |

**業務アプリ観点**: **RN + Expo は四半期ごとの追従**を運用に織り込み必須。Flutter と同程度。

### 1.6 LTS・サポート期限・EOL(F5)

| 項目 | 内容 |
|---|---|
| 明示 LTS | **なし** |
| RN 古いマイナーサポート | 通常 **直近 6 マイナー程度**(約 1 年) |
| Expo SDK サポート | 直近 **3 SDK 程度** |
| EOL 通知 | GitHub / ブログ |
| 長期保守案件の留意 | 追従を怠ると **古いパッケージが新 RN で動かない**問題が頻発 |

### 1.7 破壊的変更頻度・後方互換ポリシー(F6)

| 局面 | 変更影響度 |
|---|---|
| JavaScript / React 本体 | 低(React 自体は後方互換重視) |
| React Native API | **中〜高**(内部実装変更がパッケージに波及) |
| 新アーキ(Fabric/TurboModules) | **大**(Legacy → Bridgeless への移行案件あり) |
| Expo SDK 更新 | 中(ライブラリ入替が各 SDK で必要) |
| ネイティブ依存パッケージ | **高**(Android SDK / iOS SDK 更新で互換性切れ頻出) |

**業務アプリ観点**: **サードパーティパッケージ依存が運用上の最大の摩擦点**。`react-native-*` のメンテ停止・移行先未決で動けなくなる事例がある。

### 1.8 脆弱性対応・CVE 体制(F7)

| 項目 | 内容 |
|---|---|
| RN 窓口 | Meta セキュリティチーム(GitHub Security Advisories) |
| Expo 窓口 | security@expo.dev |
| npm パッケージ | **npm audit / Dependabot / Snyk で自動検出** |
| 脆弱性リスク傾向 | **サードパーティ npm への依存が多く、履歴上 CVE が複雑化しやすい** |

### 1.9 商用サポート(F8)

| 手段 | 内容 |
|---|---|
| Meta 直接の商用サポート | **なし** |
| **Callstack**(OSS コンサル) | RN 中核コントリビュータ、商用サポート提供 |
| **Expo Enterprise プラン** | EAS 商用サポート・SLA・Private Runner |
| Infinite Red、Software Mansion | RN 専業コンサル |

---

## 2. フレームワーク特性

### 2.1 アーキテクチャ(新アーキ前提)

```
┌─────────────────────────────────────────────┐
│  React(宣言的 UI、Hooks、JSX)              │
├─────────────────────────────────────────────┤
│  React Native Framework(JS/TS)              │
│  - Components(View / Text / TextInput…)   │
│  - StyleSheet                               │
│  - Flexbox(Yoga 3)                         │
├─────────────────────────────────────────────┤
│  JSI(JavaScript Interface)                  │
│  - JS ↔ C++ 間の同期呼び出し                │
├─────────────────────────────────────────────┤
│  Fabric(新レンダラ)+ TurboModules           │
│  - 同期 UI 更新                             │
│  - ネイティブモジュールの遅延読み込み        │
├─────────────────────────────────────────────┤
│  Hermes(JS エンジン、デフォルト)            │
├─────────────────────────────────────────────┤
│  ネイティブ UI(Android View / iOS UIView)   │
└─────────────────────────────────────────────┘
```

### 2.2 新アーキテクチャ(2024 年デフォルト化)

| 要素 | 旧 | 新 | 効果 |
|---|---|---|---|
| **Bridge** | 非同期 JSON メッセージ | **JSI**(同期呼び出し) | レイテンシ大幅短縮 |
| **レンダラ** | Paper(非同期) | **Fabric**(同期) | UI 更新がフレーム内で完結 |
| **ネイティブモジュール** | 初期化時に全ロード | **TurboModules**(遅延) | 起動高速化 |
| **Codegen**(Pigeon 相当) | なし | **あり** | 型安全 Bridge コード生成 |
| **Bridgeless** | — | **あり(0.74+)** | Bridge 完全排除 |

**業務アプリ観点**: 新アーキで **旧バージョンの性能批判がほぼ解消**。ただし古いパッケージが新アーキ非対応な場合あり。

### 2.3 UI 描画方式

**OS 純正ネイティブ UI を呼出**(Flutter のような自前描画ではない)

| メリット | デメリット |
|---|---|
| iOS/Android ネイティブな外観 | プラットフォーム差が残る |
| OS 標準アクセシビリティ活用 | UI 完全一致は難しい |
| OS 新機能の追従が早い | 新 OS リリース時のネイティブ対応が必要 |

### 2.4 言語

| 言語 | 比率 |
|---|---|
| TypeScript | **事実上標準**(0.71 以降テンプレデフォルト) |
| JavaScript | 可、旧プロジェクト |
| ネイティブモジュール(Kotlin/Swift) | 必要時 |

### 2.5 並行・非同期モデル

| 機構 | 用途 |
|---|---|
| `Promise` / `async-await` | 標準 |
| React Hooks(`useEffect`、`useLayoutEffect`) | 副作用管理 |
| `react-query` / TanStack Query | サーバ状態管理 |
| Zustand / Redux Toolkit | クライアント状態管理 |
| `Worker`(react-native-workers) | 限定的 |

### 2.6 エラーハンドリング

| 機構 | 使い所 |
|---|---|
| `try/catch` + async/await | 主流 |
| React Error Boundaries | レンダリング時のエラー捕捉 |
| Sentry / Crashlytics | ランタイムクラッシュ |
| `LogBox`(開発時エラー UI) | 開発時 |

### 2.7 メタプログラミング・コード生成(H5)

| 技術 | 内容 |
|---|---|
| **Codegen**(新アーキ) | 型安全な Bridge コード自動生成 |
| Babel Plugins | コンパイル時変換 |
| Decorators(Legacy) | 一部ライブラリで利用 |

### 2.8 FFI・ネイティブ連携(H6)

| 連携手段 | 内容 |
|---|---|
| **Native Modules / TurboModules** | Kotlin / Swift でネイティブ機能実装、JS から呼出 |
| **Native UI Components** | カスタムネイティブ View を JSX で利用 |
| **JSI(C++ レベル)** | Hermes ランタイム直下、最速 |
| **Expo Config Plugins** | Expo でネイティブ設定を JS 側から宣言 |
| **Expo Modules API** | ネイティブモジュールを Expo 流に記述(Swift/Kotlin DSL) |

---

## 3. 開発環境・ツーリング

### 3.1 CLI・SDK

| コマンド | 用途 |
|---|---|
| `npx create-expo-app` / `npx react-native init`(旧) | プロジェクト生成 |
| `npx expo start` / `npx react-native start` | Metro + 開発サーバ |
| `npx expo run:android` / `:ios` | ローカルビルド実行 |
| `eas build` | **クラウドビルド**(Expo EAS) |
| `eas update` | **OTA 配信** |
| `eas submit` | ストア提出 |

### 3.2 IDE

| IDE | 評価 |
|---|---|
| **VS Code** | ◎ 業界標準 |
| WebStorm | ◎ |
| Android Studio | ネイティブビルド・Android 固有デバッグで併用 |
| Xcode | iOS ビルド時(Mac 必須、EAS 使用時は不要) |

### 3.3 LSP・補完品質(H8)

- TypeScript Language Server(tsserver)
- VS Code で強力な補完・リファクタ・定義移動
- Hooks の依存配列警告(ESLint Plugin)
- React Native 固有プロパティ補完(Android/iOS Style 差異)

### 3.4 ビルド・依存管理

| 項目 | 内容 |
|---|---|
| **Metro**(bundler) | RN 専用、Webpack 的役割 |
| **Hermes**(JS engine、デフォルト) | Meta 製、Android/iOS 両対応 |
| パッケージマネージャ | **npm / yarn / pnpm**(Expo 環境では npm か pnpm 推奨) |
| Lock file | `package-lock.json` / `yarn.lock` / `pnpm-lock.yaml` |
| iOS | CocoaPods(`Podfile` 経由)、Expo ではほぼ隠蔽 |
| Android | Gradle(設定ファイル `android/build.gradle`) |

### 3.5 Lint / Formatter

| ツール | 役割 |
|---|---|
| **ESLint + @react-native/eslint-config** | 公式 Lint |
| **Prettier** | 公式 Formatter |
| TypeScript strict | `tsc --noEmit` |

### 3.6 デバッガ・プロファイラ

| ツール | 用途 |
|---|---|
| **React Native DevTools**(Chrome DevTools 互換、公式) | JSI ブレークポイント、Elements、Network、Profiler |
| React Developer Tools | React コンポーネントツリー検査 |
| Flipper | **非推奨**(0.74 以降、Expo では非対応) |
| Android Studio / Xcode Debugger | ネイティブ層デバッグ |
| Sentry / Firebase Crashlytics | 本番クラッシュ |

**業務アプリ観点**: Flipper 依存の資料・チュートリアルが古いので、新規は RN DevTools を使うこと。

### 3.7 Hot Reload / Fast Refresh

| 機能 | 内容 |
|---|---|
| **Fast Refresh** | Flutter Hot Reload 相当、**状態保持**、高速 |
| Hooks 構造変更時 | 再マウント(状態リセット) |
| ネイティブコード変更時 | 再ビルド必要 |

### 3.8 コンパイル速度(H13)

| 局面 | 体感 |
|---|---|
| Fast Refresh | < 1 秒 |
| 初回 Gradle(Android) | 5〜15 分 |
| 初回 Xcode(iOS) | 5〜10 分 |
| 2 回目以降 | 数十秒〜数分 |
| **EAS Build**(クラウド) | 5〜20 分(Mac 不要) |

### 3.9 Windows 開発環境(本件最重要)

| 項目 | 内容 |
|---|---|
| Node.js(LTS 推奨) | ◎ |
| Android SDK | ◎ |
| iOS 開発 | **Mac 必須 or EAS Build(クラウド)** |
| **Expo EAS Build** | **Mac 不要で iOS ビルド可** |
| ディスク占有 | Expo Managed なら 5〜10 GB、Bare なら 20〜30 GB + Android SDK |
| **Expo Go**(開発者用実機アプリ) | 即座に Android/iOS 実機で確認可、Windows から iPhone 実機検証可 |

**Windows 開発での最大の利点**: **Expo EAS Build + Expo Go** で、**Mac を一切用意せずに iOS アプリを開発・ビルド・配信**可能。これは主要 FW の中で RN(Expo)のみが持つ実質的優位性。

---

## 4. パフォーマンス特性

### 4.1 起動時間(新アーキ後)

| 局面 | 体感 |
|---|---|
| Cold start | Hermes + TurboModules で大幅改善、Native には劣る程度 |
| Warm start | 即時 |
| Bridge 経由呼び出し | **排除済**(Bridgeless) |

### 4.2 スループット

| 指標 | 値 |
|---|---|
| UI | ネイティブと同等(OS UI を利用) |
| JS ↔ Native 呼び出し | **JSI で同期化、高速** |
| 複雑アニメーション | Reanimated 3 + Worklet で **UI スレッド上で実行可**、ネイティブ同等 |

### 4.3 メモリフットプリント

| 項目 | 値 |
|---|---|
| Hermes ランタイム | 数 MB |
| JS コード + React Tree | アプリ複雑度依存 |
| APK/IPA 寄与 | 5〜10 MB 基本 |

### 4.4 APK / アプリサイズ(H14)

| 項目 | サイズ |
|---|---|
| 最小 APK | **5〜10 MB**(Flutter より小さい場合多い) |
| Expo Managed 追加アセット | 数 MB |
| Hermes | JIT 比で ABI 縮小効果あり |

### 4.5 ベンチマーク参照

- React Native 新アーキ性能: https://reactnative.dev/architecture/landing-page

---

## 5. エコシステム・コミュニティ

### 5.1 パッケージ規模

| レジストリ | 規模 |
|---|---|
| **npm**(全体) | 数百万パッケージ(最大) |
| **react-native-\*** | 数万 |
| **@expo/\*** / **expo-\*** | 公式 SDK 100+ モジュール |
| React Native Directory | https://reactnative.directory/(FW 特化カタログ) |

### 5.2 GitHub 定量指標(H11、2026-04 時点目安)

| リポジトリ | Stars(概算) |
|---|---|
| `facebook/react-native` | **約 120k** |
| `expo/expo` | 約 34k |
| `software-mansion/react-native-reanimated` | 約 9k |
| `wix/Detox` | 約 11k |

### 5.3 年次調査(H11)

| 調査 | 位置 |
|---|---|
| Stack Overflow Developer Survey — Most Used(Frameworks) | **常時上位**(JS 系の強み) |
| Stack Overflow Most Admired | Flutter に 2020 年頃から抜かれたが依然 TOP 10 |
| GitHub Octoverse | 人気 FW 上位 |
| npm Download Statistics | **react-native 週次 100 万+** |

### 5.4 日本語リソース(H12)

| 種別 | 代表 |
|---|---|
| 国内カンファレンス | **React Native Meetup Tokyo**、**Reactな話**、**React Japan** |
| 書籍(日本語) | 『React Native アプリ開発入門』他、数は Flutter より少なめ |
| 日本語ブログ | Qiita / Zenn に多数 |
| Expo 日本語情報 | 増加中 |
| 国内採用 | メルカリ(部分)、LINE、DMM、ピクシブ 等 |

### 5.5 認定・トレーニング

- React Native School(非公式、英語)
- Maximilian Schwarzmüller Udemy コース
- Expo 公式 Tutorial
- 公式認定資格なし

### 5.6 コミュニティガバナンス

- React Native Open Source Program
- RFC プロセス: https://github.com/react-native-community/discussions-and-proposals

---

## 6. 業務アプリ視点(HT 特化)

### 6.1 HT スキャナ連携

| 機能 | 実装 |
|---|---|
| **DataWedge Keyboard Wedge** | `<TextInput>` にフォーカスがあれば受信、実装不要 |
| **DataWedge Intent API** | `react-native-datawedge-intents`(Darryn Campbell 氏、非公式) / 自作 Native Module |
| **Expo Managed Workflow** | **Intent 受信不可 → Dev Client / Prebuild 必須** |
| Zebra EMDK | 3rd party ラッパ、自作 Native Module |
| Honeywell / Datalogic | カスタム Native Module |

**業務アプリ観点の落とし穴**: **Expo Managed で HT を採用するのは事実上不可**。**Expo Dev Client**(カスタム開発ビルド)にすれば Intent 受信可。RN + Expo の選定時は Managed vs Dev Client を最初に決める。

### 6.2 MDM 連携

| 機能 | 代表ライブラリ |
|---|---|
| App Config | `react-native-managed-app-config` |
| Kiosk / Lock Task | カスタム Native Module |
| Device Owner | 同上 |

### 6.3 オフライン・同期

| 機能 | 代表ライブラリ |
|---|---|
| ローカル DB | **WatermelonDB**(大規模オフライン同期型、RN 特化)、SQLite(`expo-sqlite` / `react-native-sqlite-storage`) |
| KVS | **MMKV**(高速)、`@react-native-async-storage/async-storage` |
| バックグラウンドタスク | `expo-background-fetch` / `expo-task-manager`、`react-native-background-fetch` |
| 通信 | **fetch**(標準)、`axios`、`ky` |
| WebSocket | 標準、`socket.io-client` |
| gRPC | `@improbable-eng/grpc-web`(制約あり) |

### 6.4 セキュリティ

| 機能 | 代表ライブラリ |
|---|---|
| セキュアストレージ | **`expo-secure-store`**、`react-native-keychain` |
| 生体認証 | **`expo-local-authentication`** |
| 証明書ピン留め | `react-native-ssl-pinning` |
| 暗号化 | `crypto-js`、`expo-crypto` |

### 6.5 カメラ・スキャン

| 機能 | 代表ライブラリ |
|---|---|
| カメラ | **`expo-camera`** / **`react-native-vision-camera`**(高機能) |
| QR/バーコード | `expo-barcode-scanner`、Vision Camera + frame processors |
| OCR | Vision Camera + ML Kit frame processor |
| ML Kit | `@react-native-ml-kit/*`、`@infinitered/react-native-mlkit-*` |

### 6.6 プッシュ通知・クラッシュ収集

| 機能 | 代表ライブラリ |
|---|---|
| FCM/APNS | **`@react-native-firebase/messaging`**、**`expo-notifications`**(FCM 背後) |
| Sentry | **`@sentry/react-native`**(事実上標準) |
| Crashlytics | `@react-native-firebase/crashlytics` |

### 6.7 OTA(Over-The-Air)

| 機能 | 内容 |
|---|---|
| **EAS Update**(公式) | **Expo の強みの 1 つ**、本番で即時 JS/アセット更新 |
| **CodePush**(Microsoft) | App Center 終了(2025-03)で後継検討要 |
| 制約 | JS コードは OTA 可、ネイティブコード変更はストア審査 |

---

## 7. 主要採用事例(H9)

### 7.1 グローバル

| 企業 | 用途 |
|---|---|
| **Meta** | Facebook、Instagram、Messenger、Ads Manager、Marketplace |
| **Microsoft** | Office、Outlook、Teams、Xbox |
| **Shopify** | モバイルアプリ全面 |
| **Discord** | Android アプリ |
| **Walmart** | 米国アプリ |
| **Coinbase** | 暗号通貨アプリ |
| Tesla、Pinterest、Uber Eats、Airbnb(旧、現 Native 回帰)| — |
| **Bluesky**(SNS、Expo 採用) | — |
| **MrBeast**(Expo)| 公式アプリ |

### 7.2 国内

| 企業 | 用途 |
|---|---|
| メルカリ | 部分採用 |
| LINE | 一部アプリ |
| DMM、ピクシブ、マネーフォワード | モバイル |
| freee、Sansan | モバイル(部分) |

### 7.3 ドメイン別

| ドメイン | 採用実績 |
|---|---|
| SNS / コンシューマ | ◎(Meta、Discord、Bluesky) |
| Fintech | ◎(Coinbase、mercari pay 部分) |
| SaaS モバイル | ◎(Microsoft Office 系) |
| EC / 小売 | ◎(Walmart、Shopify) |
| HT 業務 | ○(実装あるが Native Kotlin 比では少) |
| 金融基幹 | ○(部分採用) |
| ゲーム | △(ゲーム本体は非、UI は可) |

---

## 8. AI/LLM 補完相性(H10)

| 観点 | 評価 |
|---|---|
| 学習データ量 | **◎ 最大**(JS/TS + React の膨大な学習資産) |
| 型情報活用 | ◎ TypeScript 強型付けで補完精度高 |
| JSX/Hooks 補完 | ◎ 構造が明確、Copilot が提案しやすい |
| GitHub Copilot | ◎ 最高水準 |
| ChatGPT / Claude | ◎ React/RN 質問への回答精度最高 |
| 誤補完 | 古いライブラリ(Redux Classic、Enzyme 等)の提案あり |

---

## 9. 学習パス(H18)

### 9.1 学習曲線の形状

```
難易度
  │                ____________ エキスパート(6〜12ヶ月)
  │               /             新アーキ、Reanimated、Native Module
  │          ____/
  │         /                    実務(3〜5ヶ月)
  │     ___/                     状態管理、Navigation、パッケージ選定
  │    /                         
  │   /                          基礎(2〜3ヶ月、jQuery から)
  │  /                           TS + React Hooks + RN 固有
  │ /                            
  └──────────────────────→ 時間
```

**形状の特徴**:

- **初手中〜急**(Hooks・関数型思考・モバイル境界で 3 壁)
- **中盤スロープ**(npm エコシステム選定の学習コスト)
- **長期はパッケージ追従力が効く**(新アーキ移行・SDK 更新)

### 9.2 推奨学習パス(jQuery/Java 経験者前提、4〜6 ヶ月)

| 週 | テーマ |
|---|---|
| 1〜2 | モダン JS(ES2020+)/ TypeScript 基礎 |
| 3〜4 | React 基礎(Hooks、関数コンポーネント、props、context) |
| 5〜6 | React Native 基礎(Core Components、StyleSheet、Flexbox) |
| 7〜8 | Expo セットアップ、Expo Router、Expo Go |
| 9〜10 | 状態管理(Zustand または Redux Toolkit)、TanStack Query |
| 11〜12 | ローカル DB(MMKV + SQLite)、通信、認証 |
| 13〜14 | テスト(Jest + RNTL、Detox/Maestro) |
| 15〜16 | HT 統合(DataWedge Dev Client)、MDM |
| 17〜20 | 本番開発着手 |

### 9.3 推奨教材

| カテゴリ | 教材 |
|---|---|
| 公式 | [React Native Docs](https://reactnative.dev/)、[Expo Docs](https://docs.expo.dev/) |
| コース | [React Native School](https://www.reactnativeschool.com/)、Udemy Maximilian Schwarzmüller |
| 書籍 | 『React Native アプリ開発入門』『Learning React Native』 |
| 動画 | React Native Meetup Tokyo、React Conf |
| Expo 公式 | Tutorial、Examples(GitHub) |

---

## 10. 落とし穴・バッドパターン

| # | 落とし穴 | 対策 |
|---|---|---|
| 1 | **Expo Managed で HT Intent 連携しようとして詰む** | 最初から **Dev Client / Bare Workflow** を選択 |
| 2 | `useEffect` 依存配列の誤りで無限ループ | ESLint `react-hooks/exhaustive-deps` を必ず有効 |
| 3 | 古いパッケージが新 RN で動かない | 定期的に依存更新、React Native Directory で採点確認 |
| 4 | Metro キャッシュ不整合で謎エラー | `npx react-native start --reset-cache` |
| 5 | 状態管理を途中で Redux → Zustand → Jotai と変える | 最初に決める、本件なら **Zustand が最軽量** |
| 6 | `FlatList` でパフォーマンス劣化 | `keyExtractor`、`getItemLayout`、`removeClippedSubviews` |
| 7 | Native Module が iOS/Android のどちらかだけ動く | 公式 Expo Modules API または Turbo Module で作成 |
| 8 | Navigator v4 と v5 の混在(古い記事に引きずられる)| **React Navigation v7** に統一、または Expo Router |
| 9 | 新アーキ未対応パッケージで Bridgeless 化できない | 代替パッケージを探す、依存関係を減らす |
| 10 | OTA 配信で規約違反 | App Store / Play の OTA 規約(変更はバグ修正に限る等)を遵守 |
| 11 | Hermes で動作しないライブラリ | **JSC(JavaScriptCore)フォールバック**可だが性能低下 |
| 12 | Flipper 依存の古いチュートリアル | React Native DevTools に読み替え |

---

## 11. 業務アプリでの適性評価

### 11.1 本件(Android HT + 20 画面 + Java/jQuery + Android 永続)

| 観点 | 評価 | コメント |
|---|---|---|
| 言語習熟難易度 | ★★☆☆☆ | TS + React + RN + モバイル境界で学習コスト最大 |
| 既存 jQuery 資産流用 | ★☆☆☆☆ | React は jQuery と思想が逆、資産流用不可 |
| HT スキャナ連携 | ★★★☆☆ | Dev Client 必須、Flutter 比で情報少 |
| 20 画面規模 | ★★★★☆ | 規模は問題なし |
| Android 永続 | ★★☆☆☆ | RN の最大利点(両 OS 対応)が活きない |
| Windows 開発 | ★★★★★ | Expo EAS で Mac 不要 |
| 長期保守 | ★★★☆☆ | 破壊的変更頻度高、追従負担 |
| 国内人材 | ★★★★☆ | React 人材は多いが RN 経験は減 |
| **総合** | ★★★☆☆ | **既存 React/JS 人材がない限り優先度低** |

### 11.2 業務アプリ一般

| ドメイン | 推奨度 |
|---|---|
| Android モバイル | ★★★☆☆(Kotlin / Flutter と競合) |
| Android + iOS | ★★★★☆ |
| Web + モバイル共有 | ★★★★☆(Expo Router で URL ベース可) |
| 高負荷 UI | ★★★☆☆(Reanimated 使いこなしが前提) |
| SaaS モバイル(React Web 資産あり) | ★★★★★ |
| コンシューマ向け(OTA 重視) | ★★★★★ |

---

## 12. ロードマップ(H15)

- Bridgeless の完全移行と旧ブリッジ削除
- **Expo SDK 連続四半期リリース**継続
- React Server Components 統合(実験的)
- AI アシスト(GitHub Copilot Workspace、Expo AI)
- パフォーマンス最適化(Yoga 3 以降)
- React Native for Web の成熟

公式: https://github.com/reactwg/react-native-new-architecture

---

## 13. 参照リンク

### 13.1 公式

| 対象 | URL |
|---|---|
| React Native 公式 | https://reactnative.dev/ |
| React Native リリース WG | https://github.com/reactwg/react-native-releases |
| 新アーキ | https://reactnative.dev/architecture/landing-page |
| Expo 公式 | https://expo.dev/ |
| Expo Docs | https://docs.expo.dev/ |
| Expo Router | https://docs.expo.dev/router/introduction/ |
| EAS Build | https://docs.expo.dev/build/introduction/ |
| EAS Update | https://docs.expo.dev/eas-update/introduction/ |
| Expo Pricing | https://expo.dev/pricing |

### 13.2 エコシステム

| 対象 | URL |
|---|---|
| React Native Directory | https://reactnative.directory/ |
| React Native Community | https://github.com/react-native-community |
| React Native Firebase | https://rnfirebase.io/ |
| Reanimated | https://docs.swmansion.com/react-native-reanimated/ |
| Vision Camera | https://react-native-vision-camera.com/ |
| React Navigation | https://reactnavigation.org/ |
| TanStack Query | https://tanstack.com/query |
| Zustand | https://github.com/pmndrs/zustand |

### 13.3 業務・HT 向け

| 対象 | URL |
|---|---|
| react-native-datawedge-intents | https://github.com/darryncampbell/react-native-datawedge-intents |
| WatermelonDB | https://watermelondb.dev/ |
| MMKV | https://github.com/mrousavy/react-native-mmkv |
| @sentry/react-native | https://docs.sentry.io/platforms/react-native/ |

### 13.4 開発環境・ツール

| 対象 | URL |
|---|---|
| Metro | https://metrobundler.dev/ |
| Hermes | https://hermesengine.dev/ |
| React Native DevTools | https://reactnative.dev/docs/react-native-devtools |
| Detox(E2E) | https://wix.github.io/Detox/ |
| Maestro(E2E) | https://maestro.mobile.dev/ |
| Codemagic | https://codemagic.io/react-native-ci-cd/ |

### 13.5 コミュニティ・学習

| 対象 | URL |
|---|---|
| React Conf | https://conf.react.dev/ |
| App.js Conf(Expo 主催) | https://appjs.co/ |
| React Native Radio(Podcast) | https://reactnativeradio.com/ |

### 13.6 セキュリティ

| 対象 | URL |
|---|---|
| RN Security Advisories | https://github.com/facebook/react-native/security/advisories |
| Expo Security | security@expo.dev |

---

## 14. 本ファイルの位置付け

- FW 詳細プロファイル(profile_flutter.md / profile_android_native.md と同粒度)
- RN と Expo は実運用で一体化するため統合して扱う
- 次は `profile_ionic_capacitor.md`、`profile_kmp.md` 予定

---

## 更新履歴

- **2026-04-23**: 初版作成。FW 詳細プロファイル(React Native + Expo)。
