# フレームワーク/言語別 プロファイル集

本ドキュメントは、`selection_criteria.md` で定義した観点(F. 保守・ガバナンス、G. 開発環境、他)について、**言語/フレームワーク単位で事実情報を収集**したファクトシートである。
比較・一覧表は本データから派生する二次物として位置付ける。

**姉妹ドキュメント**:

- [`framework_native_features.md`](./framework_native_features.md) — 言語の活用範囲(Android 外で何ができるか)と、カメラ/バーコード/NFC/BLE/GPS/生体認証/プッシュ/DB/MDM/印刷/ML 等 23 カテゴリ × 主要 FW の代表ライブラリ対応表
- [`profile_android_native.md`](./profile_android_native.md) — 本件第一推奨の Android ネイティブを深掘りした詳細版

- 情報は **2026年4月時点**。特にバージョン・EOL日付は公式ソースで採用時に再確認必須
- 対象: Android業務アプリで検討対象になり得る主要言語・FW

---

## 目次

### 言語
- [Kotlin](#kotlin)
- [Dart](#dart)
- [TypeScript](#typescript)
- [JavaScript(ECMAScript)](#javascriptecmascript)
- [Java](#java)
- [C#](#c)
- [Swift(参考)](#swift参考)

### フレームワーク
- [Android ネイティブ(Jetpack Compose + Android SDK)](#android-ネイティブjetpack-compose--android-sdk)
- [Flutter](#flutter)
- [React Native](#react-native)
- [Expo(React Native)](#exporeact-native)
- [Ionic Framework](#ionic-framework)
- [Capacitor](#capacitor)
- [Kotlin Multiplatform (KMP)](#kotlin-multiplatform-kmp)
- [Compose Multiplatform](#compose-multiplatform)
- [.NET MAUI](#net-maui)
- [NativeScript](#nativescript)
- [Apache Cordova(参考/レガシー)](#apache-cordova参考レガシー)
- [Xamarin(EOL)](#xamarineol)
- [PWA(参考)](#pwa参考)

### プロファイルのテンプレート項目

各プロファイルは以下の枠で統一している(該当なしの項目は `—`)。

- **基本情報**: 名称、開発元、ライセンス、初版、最新安定版、リリース頻度、LTS、EOL
- **言語・ランタイム**: 主言語/サブ言語、コンパイル方式
- **開発環境**: 推奨IDE、ビルド、依存管理、対応開発OS、iOSビルド要件、ディスク占有、セットアップ
- **開発体験**: ホットリロード、プロファイラ、Lint/Formatter
- **テスト**: Unit / UI / E2E
- **CI/CD・配信**: 代表CI、OTA、ストア
- **パッケージ管理・脆弱性**
- **業務アプリ適性・代表事例**
- **参照リンク**

---

## 言語

### Kotlin

> **詳細版**: [`profile_kotlin.md`](./profile_kotlin.md) — H1〜H18 観点反映(パフォーマンス・メモリ管理・エラーハンドリング・標準ライブラリ・メタプログラミング・FFI・ツーリング品質・AI補完相性・学習曲線形状 等)の言語単体詳細プロファイル

- **基本情報**
  - 開発元: JetBrains(+ Google が後援、Kotlin Foundation が管理)
  - ライセンス: Apache License 2.0
  - 初版: 2011 / 1.0 = 2016-02
  - 最新安定版(2026-04): 2.1 系(K2 コンパイラがデフォルト)
  - リリース頻度: 6ヶ月メジャー+随時パッチ
  - LTS: 明示 LTS なし(事実上の後方互換維持ポリシー)
  - EOL: なし
- **言語・ランタイム**
  - コンパイル: JVM バイトコード / Kotlin Native / Kotlin/JS / Kotlin/Wasm
  - Android では Java と完全相互運用
- **開発環境**
  - 推奨 IDE: Android Studio / IntelliJ IDEA(どちらもJetBrains)
  - ビルド: Gradle(Kotlin DSL 推奨)
  - 依存管理: Maven Central、Google Maven
  - 対応 OS: Windows / macOS / Linux
  - セットアップ: Android Studio インストールで完結
- **開発体験**
  - Lint: **ktlint** / **Detekt** / Android Lint
  - Formatter: `ktlint --format` / IntelliJ フォーマッタ
- **テスト**
  - Unit: JUnit、MockK、Kotest
  - UI(Android): Espresso、Compose Test
- **パッケージ管理・脆弱性**
  - Maven Central + Gradle Dependency Check、Dependabot
- **業務アプリ適性**
  - 強み: Java資産完全流用、Android公式推奨、Hilt(DI)、coroutine
  - 弱み: 言語単体では機能豊富、新機能追従に学習コスト
- **参照リンク**
  - 公式: https://kotlinlang.org/
  - リリース: https://kotlinlang.org/docs/releases.html
  - Kotlin Foundation: https://kotlinfoundation.org/
  - JetBrains 商用サポート: https://www.jetbrains.com/

---

### Dart

- **基本情報**
  - 開発元: Google
  - ライセンス: BSD-3-Clause
  - 初版: 2011 / 1.0 = 2013
  - 最新安定版(2026-04): Dart 3.x 系(Flutter と同期)
  - リリース頻度: 四半期マイナー+随時パッチ(Flutter と同期)
  - LTS: 明示 LTS なし
  - EOL: なし
- **言語・ランタイム**
  - コンパイル: AOT(本番)/ JIT(開発)
  - null safety: Dart 3 で強制
- **開発環境**
  - 推奨 IDE: Android Studio + Flutter/Dart plugin / VS Code + Dart extension
  - 依存管理: `pub` / pub.dev
  - 対応 OS: Windows / macOS / Linux
- **開発体験**
  - Lint: `dart analyze`、flutter_lints
  - Formatter: `dart format`
- **テスト**
  - Unit: `dart test`、flutter_test
- **パッケージ管理・脆弱性**
  - pub.dev、`dart pub outdated`、GitHub Dependabot 対応
- **業務アプリ適性**
  - 強み: 学習容易(Javaに近い)、Flutter と一体、型安全
  - 弱み: Flutter以外での採用が限定的、国内人材薄
- **参照リンク**
  - 公式: https://dart.dev/
  - パッケージ: https://pub.dev/

---

### TypeScript

- **基本情報**
  - 開発元: Microsoft
  - ライセンス: Apache License 2.0
  - 初版: 2012 / 1.0 = 2014
  - 最新安定版(2026-04): 5.x 系
  - リリース頻度: 3〜4ヶ月マイナー(x.y)、随時パッチ
  - LTS: 明示 LTS なし(後方互換性重視)
  - EOL: なし
- **言語・ランタイム**
  - JS へトランスパイル、ブラウザ/Node.js/Bun/Deno で実行
- **開発環境**
  - 推奨 IDE: VS Code(公式最優先) / WebStorm / IntelliJ
  - ビルド: tsc、esbuild、swc、Vite、webpack
  - 依存管理: npm / yarn / pnpm(本体は npm に含まれる)
- **開発体験**
  - Lint: ESLint + `@typescript-eslint`
  - Formatter: Prettier
- **テスト**
  - Unit: Jest、Vitest、node:test
- **パッケージ管理・脆弱性**
  - npm audit、pnpm audit、Snyk、Socket、GitHub Dependabot
- **業務アプリ適性**
  - 強み: 型安全、エコシステム最大、AI 補完相性良
  - 弱み: strict 運用は設計/規約整備が必要
- **参照リンク**
  - 公式: https://www.typescriptlang.org/
  - リリース: https://devblogs.microsoft.com/typescript/

---

### JavaScript(ECMAScript)

- **基本情報**
  - 標準化: Ecma International TC39
  - ライセンス: 標準仕様
  - 年次メジャー(毎年6月 ES2020/2021/…)
  - EOL: なし(標準仕様)
- **言語・ランタイム**
  - ブラウザ各社 / Node.js / Deno / Bun / Hermes(RN標準エンジン)
- **業務アプリ適性**
  - 強み: ブラウザ標準、最も普及
  - 弱み: 動的型付けで大規模保守は TS 推奨
- **参照リンク**
  - TC39: https://tc39.es/

---

### Java

- **基本情報**
  - 主導: Oracle + OpenJDK コミュニティ(IBM、Red Hat、Azul、Amazon、Microsoft 等)
  - ライセンス: OpenJDK = GPLv2 + Classpath Exception / Oracle JDK 17+ = Oracle NFTC
  - 初版: 1995 / 最新 LTS: Java 21(2023-09)、次期 LTS: Java 25(2025-09 予定)
  - リリース頻度: 6ヶ月メジャー、LTS は 2年ごと
  - LTS: あり。Java 8 / 11 / 17 / 21 / 25…(ベンダー別 6〜8年サポート)
- **言語・ランタイム**
  - JVM バイトコード、複数ディストリ(Temurin / Corretto / Zulu / Microsoft Build of OpenJDK 等)
- **開発環境**
  - 推奨 IDE: IntelliJ IDEA / Eclipse / VS Code + Java Extension Pack
  - ビルド: Gradle / Maven
  - 依存管理: Maven Central
- **業務アプリ適性(Android視点)**
  - Android では Kotlin に置き換わりつつあるが、旧モジュール・ライブラリは Java 直書きも併存可
- **参照リンク**
  - OpenJDK: https://openjdk.org/
  - Oracle ロードマップ: https://www.oracle.com/java/technologies/java-se-support-roadmap.html
  - Oracle NFTC: https://www.oracle.com/downloads/licenses/no-fee-license.html
  - Temurin: https://adoptium.net/temurin/releases/
  - Corretto: https://aws.amazon.com/corretto/
  - Azul: https://www.azul.com/downloads/

---

### C#

- **基本情報**
  - 開発元: Microsoft(+.NET Foundation)
  - ライセンス: MIT(ランタイム・標準ライブラリ)
  - 初版: 2000 / .NET Core 初版 = 2016
  - 最新安定版(2026-04): C# 13 系 / .NET 9(STS)、.NET 8(LTS)
  - リリース頻度: 年1メジャー(11月)
  - LTS: 偶数バージョンが 3 年 LTS、奇数は 18ヶ月 STS
- **業務アプリ適性(Android視点)**
  - .NET MAUI / 旧 Xamarin 経由、業務アプリ中 Microsoft 系社内標準の組織のみ
- **参照リンク**
  - .NET: https://dotnet.microsoft.com/
  - サポートポリシー: https://dotnet.microsoft.com/en-us/platform/support/policy/dotnet-core

---

### Swift(参考)

- **基本情報**
  - 開発元: Apple(+ Swift Community)
  - ライセンス: Apache License 2.0
  - 初版: 2014 / 最新: 6.x
  - リリース頻度: 年1メジャー(x.0)+ 年中に x.y 複数
- **本件スコープ**: iOS 専用のため Android 業務アプリでは非主軸。KMP で iOS 展開時のみ関連
- **参照リンク**
  - Swift 公式: https://www.swift.org/
  - Swift Evolution: https://www.swift.org/swift-evolution/

---

## フレームワーク

### Android ネイティブ(Jetpack Compose + Android SDK)

> **詳細版**: [`profile_android_native.md`](./profile_android_native.md) — バージョン履歴、JDK/Gradle要件、HT 連携(DataWedge/EMDK/MDM)、12 週学習ロードマップ、コスト見積もり、落とし穴、拡充参照リンクを含む深掘りプロファイル

- **基本情報**
  - 開発元: Google(Android オープンソースプロジェクト AOSP)
  - ライセンス: Apache License 2.0(AndroidX)
  - Jetpack Compose 1.0: 2021-07
  - 最新安定版(2026-04): Compose BOM 系統で月次更新
  - リリース頻度: AndroidX 月次〜四半期、Android OS 年1
  - LTS: Android OS はバージョン別、Google Play Target SDK は毎年引き上げ
- **言語**: Kotlin(推奨)/ Java(従来)
- **開発環境**
  - 推奨 IDE: **Android Studio**(IntelliJ IDEA Community ベース、無償)
  - ビルド: Gradle(Kotlin DSL / Groovy DSL)
  - 依存管理: Google Maven + Maven Central
  - 対応 OS: Win/Mac/Linux(Apple Silicon 対応)
  - ディスク占有: Android Studio + SDK + Emulator ≈ 15〜30 GB
- **開発体験**
  - ホットリロード: **Live Edit**(実験的)/ **Apply Changes**(中間)。Flutter/RN の Hot Reload には未達
  - プロファイラ: **Android Studio Profiler**(CPU/Memory/Network/Energy/Power Rail)
  - Lint/Formatter: Android Lint、ktlint、Detekt、`ktlint format`
- **テスト**
  - Unit: JUnit、MockK / Instrumented: AndroidX Test / UI: Espresso、**Compose Test**
  - E2E: Maestro、Appium
- **CI/CD・配信**
  - 代表 CI: GitHub Actions、Bitrise、Jenkins、Codemagic
  - 配信: Google Play / Firebase App Distribution / MDM / APK 直配布
  - OTA: 原則なし(Play Store In-App Update、Play Feature Delivery)
- **パッケージ管理・脆弱性**
  - Gradle Dependency Check、Dependabot、OWASP Dependency-Check
- **業務アプリ適性**
  - 強み: 最高性能、最新OS即時追従、HT業界標準、Zebra/Honeywell/Datalogic 各社SDKが直接使える
  - 弱み: Android 専用(iOS 別実装)、Compose 学習コスト
  - 代表事例: Google Pay、ほぼすべての Android 業務アプリ
- **参照リンク**
  - 公式: https://developer.android.com/
  - Jetpack Compose: https://developer.android.com/jetpack/compose
  - AndroidX リリース: https://developer.android.com/jetpack/androidx/versions
  - Google Play Target SDK 要件: https://support.google.com/googleplay/android-developer/answer/11926878
  - Android Security Bulletin: https://source.android.com/docs/security/bulletin

---

### Flutter

> **詳細版**: [`profile_flutter.md`](./profile_flutter.md) — アーキテクチャ(Engine + Framework + Impeller)・Widget ツリー・Dart 特性・Hot Reload・Windows 開発・HT連携・MDM・オフライン・主要採用事例・学習曲線・落とし穴・ロードマップ

- **基本情報**
  - 開発元: Google
  - ライセンス: BSD-3-Clause
  - 初版: 2017-05(1.0: 2018-12)
  - 最新安定版(2026-04): 3.x 系
  - リリース頻度: 四半期 stable、月次 beta、随時 patch
  - LTS: **明示LTSなし**(古バージョンのサポートは短期)
  - EOL: 明示なし、古バージョンは数ヶ月でツール非推奨
- **言語**: Dart
- **開発環境**
  - 推奨 IDE: Android Studio + Flutter plugin / VS Code + Flutter extension
  - ビルド: `flutter build`(内部で Gradle / Xcode)
  - 依存管理: pub / pub.dev
  - 対応 OS: Win/Mac/Linux
  - iOS ビルド: **Mac 必須**(Codemagic 等クラウドで代替可)
  - ディスク占有: 20〜80 GB(iOS を含むか次第)
- **開発体験**
  - ホットリロード: **Hot Reload**(状態保持、< 1秒)、Hot Restart、Hot Restart
  - プロファイラ: **Flutter DevTools**(ブラウザ統合)
  - Lint/Formatter: `flutter_lints`、`dart analyze`、`dart format`
- **テスト**
  - Unit: `flutter_test` / Widget Test: 同上 / E2E: `integration_test`、patrol、Maestro
- **CI/CD・配信**
  - 代表 CI: **Codemagic**(Flutter特化)、Bitrise、GitHub Actions
  - 配信: Play Store / App Store / Firebase App Distribution
  - OTA: サードパーティ(shorebird 等)、公式機構なし
- **パッケージ管理・脆弱性**
  - pub.dev、`dart pub outdated`、Dependabot
- **業務アプリ適性**
  - 強み: 高性能(AOT、Impeller)、UI完全一致、Hot Reload 最強、Zebra 公式 DataWedge デモあり、HT増加傾向
  - 弱み: Dart 人材薄、APK サイズ大(10〜15MB〜)、明示LTSなし
  - 代表事例: Google Pay、BMW My BMW、Alibaba Xianyu、Toyota車載
- **参照リンク**
  - 公式: https://flutter.dev/
  - リリース履歴: https://docs.flutter.dev/release/archive
  - Impeller: https://docs.flutter.dev/perf/impeller
  - GitHub: https://github.com/flutter/flutter

---

### React Native

> **詳細版**: [`profile_react_native.md`](./profile_react_native.md) — 新アーキ(JSI/Fabric/TurboModules/Bridgeless)・Hermes・Expo 含む・**Windows から iOS ビルド**・HT連携・OTA・主要採用事例

- **基本情報**
  - 開発元: Meta(+ Microsoft、Callstack、Expo、Infinite Red 等)
  - ライセンス: MIT
  - 初版: 2015-03
  - 最新安定版(2026-04): 0.7x〜0.8x 系
  - リリース頻度: 2〜3ヶ月マイナー、随時パッチ
  - LTS: **明示LTSなし**(直近数バージョンのみサポート)
  - EOL: 明示なし
- **言語**: JavaScript / TypeScript
- **開発環境**
  - 推奨 IDE: **VS Code**(主流)+ Android Studio / Xcode(ネイティブビルド)
  - ビルド: Metro bundler + Gradle + Xcode
  - 依存管理: npm / yarn / pnpm
  - 対応 OS: Win/Mac/Linux(iOS ビルドは Mac 必須)
  - ディスク占有: 15〜60 GB
- **開発体験**
  - ホットリロード: **Fast Refresh**(状態保持、高速)
  - プロファイラ: **React Native DevTools**(Chrome DevTools 互換)、React DevTools
    - 旧 Flipper はメンテ縮小(2024年以降)、新規は RN DevTools 推奨
  - Lint/Formatter: ESLint(@react-native/eslint-config)、Prettier
- **テスト**
  - Unit: **Jest** / UI: React Native Testing Library / E2E: **Detox**、Maestro、Appium
- **CI/CD・配信**
  - 代表 CI: Bitrise、GitHub Actions、**EAS Build**(Expo 利用時)、App Center(2025-03 サービス終了発表)
  - 配信: Play Store / App Store / MDM
  - OTA: **CodePush**(Microsoft、App Center 終了に伴い代替検討要)、**EAS Update**(Expo)
- **パッケージ管理・脆弱性**
  - npm audit、Dependabot、Snyk
- **業務アプリ適性**
  - 強み: JS/Web 資産流用、新アーキ(JSI/Fabric/TurboModules)、OSネイティブUI、大手採用多
  - 弱み: ネイティブ境界の知識要、破壊的変更頻度中〜高、プラグイン互換性
  - 代表事例: Facebook、Instagram、Office、Shopify、Discord、Walmart
- **参照リンク**
  - 公式: https://reactnative.dev/
  - リリース WG: https://github.com/reactwg/react-native-releases
  - 新アーキ: https://reactnative.dev/architecture/landing-page
  - CodePush / App Center 終了: https://learn.microsoft.com/en-us/appcenter/retirement

---

### Expo(React Native)

- **基本情報**
  - 開発元: Expo Inc.
  - ライセンス: MIT(SDK)、EAS は有料サービス
  - 初版: 2015
  - 最新 SDK(2026-04): SDK 5x 系(四半期ごと)
  - リリース頻度: 四半期 SDK、直近 3 SDK 程度をサポート
  - LTS: 明示なし
- **言語**: JavaScript / TypeScript
- **開発環境**
  - 推奨 IDE: VS Code
  - ビルド: **EAS Build(クラウド)** または ローカル `expo run`
  - 依存管理: npm / yarn / pnpm
  - 対応 OS: Win/Mac/Linux
  - **iOS ビルド: Mac 不要(EAS Build 経由)** — Windows 開発者の強い利点
  - ディスク占有: 5〜15 GB(Managed Workflow なら小さい)
- **開発体験**
  - ホットリロード: Fast Refresh
  - 実機確認: **Expo Go**(開発用アプリ、インストール即時確認)または **Dev Client**(カスタムビルド)
  - Lint/Formatter: ESLint、Prettier
- **テスト**
  - Jest、RNTL、Detox / Maestro
- **CI/CD・配信**
  - **EAS Build**(クラウド)、**EAS Update**(OTA 公式)、EAS Submit(ストア提出)
  - 料金: Free/Production/Enterprise プラン
- **パッケージ管理・脆弱性**
  - npm audit、Dependabot
- **業務アプリ適性**
  - 強み: 立ち上げ最速、Mac 不要で iOS 対応、OTA 標準、Expo Router で File-based ルーティング、2026年の RN 新規はほぼ Expo
  - 弱み: 細かいネイティブ制御は Dev Client / Prebuild、EAS 有料、超特殊 HW 連携は eject 相当の対応
  - 代表事例: MrBeast、BlueSky
- **参照リンク**
  - 公式: https://expo.dev/
  - ドキュメント: https://docs.expo.dev/
  - 価格: https://expo.dev/pricing
  - Expo Router: https://docs.expo.dev/router/introduction/

---

### Ionic Framework

> **詳細版**: [`profile_ionic_capacitor.md`](./profile_ionic_capacitor.md) — Stencil.js Web Components + Capacitor + Angular/React/Vue・WebView 性能制約・HT Wedge モード・Capacitor プラグイン・PWA 同時展開・本件第 2 候補

- **基本情報**
  - 開発元: Ionic(営利企業、資金調達済)
  - ライセンス: MIT(OSS コンポーネント)+ Ionic Enterprise(商用サポート)
  - 初版: 2013
  - 最新安定版(2026-04): 8.x 系
  - リリース頻度: 年1メジャー前後
  - LTS: 商用契約あり
- **言語**: JS / TS + HTML/CSS(+ React/Vue/Angular 任意)
- **開発環境**
  - 推奨 IDE: VS Code + Ionic extension
  - ビルド: npm ビルド + Capacitor(ネイティブ側)
  - 依存管理: npm / yarn / pnpm
  - 対応 OS: Win/Mac/Linux
- **開発体験**
  - ホットリロード: Vite/Webpack HMR、Live Reload、**ブラウザ実行で最速確認**
  - Lint/Formatter: ESLint、Stylelint、Prettier
  - デバッグ: Chrome DevTools(Android WebView)、Safari Web Inspector(iOS)
- **テスト**
  - Jest/Vitest、Karma、Cypress、Playwright、Appium
- **CI/CD・配信**
  - **Ionic Appflow**(公式、有料 Live Update 含む)、GitHub Actions
- **パッケージ管理・脆弱性**
  - npm audit、Dependabot、Snyk
- **業務アプリ適性**
  - 強み: Web 資産最大活用、PWA 同時展開、社内業務・情報系で立ち上げ最速
  - 弱み: WebView ベースで高負荷UI/3D/長時間稼働に弱い、BG 制限強、Capacitor プラグイン品質にばらつき
  - 代表事例: BBC Sounds、Burger King、T-Mobile、Diesel
- **参照リンク**
  - 公式: https://ionicframework.com/
  - Enterprise: https://ionic.io/pricing
  - GitHub: https://github.com/ionic-team/ionic-framework

---

### Capacitor

- **基本情報**
  - 開発元: Ionic(Cordova 後継)
  - ライセンス: MIT
  - 初版: 2019
  - 最新安定版(2026-04): 6.x〜7.x 系
  - リリース頻度: 半年〜年1
- **役割**: Web アプリをネイティブアプリとしてパッケージング、プラグインでネイティブAPI アクセス
- **開発環境**
  - CLI: `@capacitor/cli`
  - Android/iOS の公式 SDK が必要(Android Studio / Xcode)
- **業務アプリ適性**
  - Ionic と併用が主、単独で Web + Capacitor も可(Angular/React/Vue/Vanilla いずれも)
- **参照リンク**
  - 公式: https://capacitorjs.com/
  - プラグイン: https://capacitorjs.com/docs/plugins
  - GitHub: https://github.com/ionic-team/capacitor

---

### Kotlin Multiplatform (KMP)

> **詳細版**: [`profile_kmp.md`](./profile_kmp.md) — KMP Stable・Compose MP iOS Stable・expected/actual・Source Set・Kotlin/Native・SKIE・iOS 連携・Android 永続では不採用推奨の根拠

- **基本情報**
  - 開発元: JetBrains
  - ライセンス: Apache License 2.0
  - Stable 到達: 2023-11
  - 最新(2026-04): Kotlin 2.x と同期
  - リリース頻度: Kotlin に同期(6ヶ月)
  - LTS: 明示なし(JetBrains ビジネス契約で商用サポート可)
- **言語**: Kotlin(+ iOS UI 側は Swift/SwiftUI)
- **開発環境**
  - 推奨 IDE: Android Studio + KMP plugin / IntelliJ IDEA
  - ビルド: Gradle(Kotlin DSL)
  - 依存管理: Maven Central + Kotlin独自リポジトリ
  - iOS: Xcode 併用(Mac 必須)
  - ディスク占有: Android Studio + Xcode + KMP plugin = 50〜80 GB
- **開発体験**
  - ホットリロード: なし(共通部分)、Android 側 Apply Changes のみ
- **テスト**
  - kotlin-test、JUnit(Android)、XCTest(iOS)
- **CI/CD・配信**
  - GitHub Actions、Bitrise、Gradle Enterprise
- **業務アプリ適性**
  - 強み: ネイティブ性能・UX 維持、ロジック共通化、既存 Android ネイティブチームの iOS 拡張
  - 弱み: iOS UI は結局 Swift、UI 共通化は Compose MP に依存、Flutter/RN ほどの爆速開発ではない
  - 代表事例: Netflix、Cash App、McDonald's、Philips、Forbes、Careem
- **参照リンク**
  - 公式: https://kotlinlang.org/docs/multiplatform.html
  - Stable 到達アナウンス: https://blog.jetbrains.com/kotlin/2023/11/kotlin-multiplatform-stable/

---

### Compose Multiplatform

- **基本情報**
  - 開発元: JetBrains
  - ライセンス: Apache License 2.0
  - Desktop 1.0: 2021-12、iOS Stable: 2024-05
  - リリース頻度: Kotlin/KMP と同期
- **対応**: Android(Jetpack Compose)/ Desktop / iOS(Stable) / Web(実験的)
- **開発環境**
  - Android Studio + KMP plugin、IntelliJ IDEA、JetBrains Fleet
- **業務アプリ適性**
  - KMP の UI 層を共通化したい場合に採用。iOS は Stable だが成熟度は SwiftUI より浅い
- **参照リンク**
  - 公式: https://www.jetbrains.com/lp/compose-multiplatform/
  - リリースノート: https://github.com/JetBrains/compose-multiplatform/releases

---

### .NET MAUI

- **基本情報**
  - 開発元: Microsoft
  - ライセンス: MIT
  - 初版: .NET 6(2022)
  - 最新(2026-04): .NET 9(STS)/ .NET 8(LTS、2026-11 EOL)
  - リリース頻度: 年1(.NET と同期、11月)
  - LTS: 3 年(偶数バージョン)
- **言語**: C# / XAML
- **開発環境**
  - 推奨 IDE: Visual Studio 2022(Win)/ VS Code + C# Dev Kit
  - ビルド: MSBuild、`dotnet` CLI
  - 対応 OS: Win(主)、iOS ビルドはリモート Mac 必須
  - ディスク占有: 40〜60 GB
- **開発体験**
  - ホットリロード: **.NET Hot Reload** / **XAML Hot Reload**
  - テスト: xUnit、NUnit、MSTest
  - CI/CD: Azure DevOps、GitHub Actions、App Center(終了予定)
- **業務アプリ適性**
  - 強み: .NET 資産・Visual Studio 統合、C# は Java親和
  - 弱み: モバイルコミュニティ小、Android HT 実績限定的、Xamarin EOL 移行案件以外で新規採用は慎重
  - 代表事例: Microsoft 社内ツール系
- **参照リンク**
  - 公式: https://dotnet.microsoft.com/apps/maui
  - サポートポリシー: https://dotnet.microsoft.com/en-us/platform/support/policy/maui
  - GitHub: https://github.com/dotnet/maui

---

### NativeScript

- **基本情報**
  - 開発元: OpenJS Foundation(元 Progress Software)
  - ライセンス: Apache License 2.0
  - 初版: 2014
  - 最新(2026-04): 8.x 系
  - リリース頻度: 年1程度
  - モメンタム: **低下傾向**(2024年以降新規採用激減)
- **言語**: JS / TS(Angular / Vue / Svelte / Solid と統合可)
- **開発環境**
  - VS Code + NativeScript extension
  - CLI: `ns` / `tns`
- **業務アプリ適性**
  - △ 国内人材極少、大手採用少、参考記載
- **参照リンク**
  - 公式: https://nativescript.org/
  - OpenJS Foundation: https://openjsf.org/

---

### Apache Cordova(参考/レガシー)

- **基本情報**
  - 開発元: Apache Foundation
  - ライセンス: Apache License 2.0
  - 初版: 2011
  - **状態**: 活動縮小中。新規採用は **Capacitor** 推奨
- **参照リンク**
  - 公式: https://cordova.apache.org/

---

### Xamarin(EOL)

- **基本情報**
  - 開発元: Microsoft
  - ライセンス: MIT
  - **EOL: 2024-05-01**(サポート終了済)
  - 後継: **.NET MAUI**
- **参照リンク**
  - サポート終了: https://dotnet.microsoft.com/en-us/platform/support/policy/xamarin

---

### PWA(参考)

- **基本情報**
  - 標準化: W3C / WHATWG / Web Standards
  - ライセンス: 標準仕様
  - EOL: なし
- **構成要素**
  - Service Worker / Web App Manifest / IndexedDB / Web Push / Background Sync / Web NFC(Chromium Android)/ Web Bluetooth(Chromium)/ Barcode Detection API(Chromium)
- **開発環境**
  - 任意の Web スタック(React/Vue/Angular/Vanilla)
  - Chromeキオスク構成で Android HT 業務アプリとしても利用可
- **業務アプリ適性**
  - 強み: 既存 Web 資産そのまま、配布が URL+A2HS、サーバ更新で即反映
  - 弱み: Intent API 不可、BT/NFC/BG処理に制約、8h連続稼働でメモリ事故リスク
- **参照リンク**
  - MDN PWA: https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps
  - web.dev PWA: https://web.dev/explore/progressive-web-apps
  - Chrome Status: https://chromestatus.com/

---

## 本ファイルの用途

- **一次データ**: 比較・選定判断を下す際のソース・オブ・トゥルース
- **派生物**: `selection_criteria.md` の A〜G 観点別比較表、`android_business_app_framework_comparison.md` の総合評価はここから導出
- **更新方針**: 公式ソースの変更(特にバージョン・EOL・ライセンス)を四半期ごとに見直し

---

## 更新履歴

- **2026-04-23**: 初版作成。言語 7 + フレームワーク 13 のファクトシートを整備。
