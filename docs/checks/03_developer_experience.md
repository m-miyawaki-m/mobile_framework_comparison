# 開発体験・開発環境(D1〜D8)

各 FW の日常的な開発フィードバックループの快適度を 8 項目で評価する。
評価ルール詳細は [`README.md`](./README.md) 参照。

- スコア: 1〜5(5 = 最も快適)
- 重み: デフォルト 1.0(利用者が調整可)
- 仮想ペルソナ: 中規模 Android 業務アプリを 2〜3 年スパンで保守する開発組織

| 項目 | 内容 |
|---|---|
| D1 | IDE・ツーリング統合度 |
| D2 | Hot Reload / フィードバックループ |
| D3 | 初期セットアップ・ビルド速度 |
| D4 | デバッガ・プロファイラの品質 |
| D5 | テストフレームワーク成熟度 |
| D6 | CI/CD・配布の容易度 |
| D7 | ライブラリ・パッケージエコシステム |
| D8 | 開発OSの柔軟性 |

---

## D1: IDE・ツーリング統合度

### アンカー

- **5**: 公式 IDE で補完 / リファクタ / Navigate-to-def 完璧(Kotlin + Android Studio、C# + Visual Studio)
- **4**: 公式拡張で十分実用(Flutter + VS Code / AS、Dart)
- **3**: 主要操作はカバー(React Native + VS Code)
- **2**: エディタ任せで補完精度限定(NativeScript)
- **1**: IDE 統合が弱い or なし

### スコア表

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native | 5 | 1.0 | Android Studio + Kotlin プラグイン公式統合 |
| Flutter | 4 | 1.0 | Android Studio / VS Code 公式拡張、Flutter Inspector 統合 |
| React Native | 3 | 1.0 | VS Code + RN Tools、Metro 統合あるが補完精度は型次第 |
| Expo | 3 | 1.0 | VS Code + Expo Tools、TS 採用前提なら 4 |
| Ionic Framework | 4 | 1.0 | VS Code + Ionic 公式拡張、TS / Angular サポート充実 |
| Capacitor | 4 | 1.0 | VS Code 統合、Capacitor CLI 連携 |
| KMP | 4 | 1.0 | Android Studio + KMP plugin、iOS UI は Xcode 併用 |
| Compose Multiplatform | 3 | 1.0 | Android Studio + Fleet、まだ進化中 |
| .NET MAUI | 5 | 1.0 | Visual Studio 2022 + C# Dev Kit 完璧 |
| NativeScript | 2 | 1.0 | VS Code 拡張あるが補完限定 |
| Apache Cordova | 3 | 1.0 | エディタ非依存、Web 開発標準 |
| Xamarin | 4 | 1.0 | Visual Studio 完璧だが EOL |
| PWA | 4 | 1.0 | VS Code + 各種 Web 拡張、ブラウザ DevTools 統合 |

---

## D2: Hot Reload / フィードバックループ

### アンカー

- **5**: 1 秒未満で反映 + state 保持(Flutter Hot Reload、Ionic Web HMR)
- **4**: 高速だが state リセットあり(React Native Fast Refresh、Expo、.NET MAUI Hot Reload)
- **3**: 中速、部分的反映(SwiftUI Preview)
- **2**: Apply Changes 等の制約多い(Android Native Live Edit)
- **1**: フルリビルド必須

### スコア表

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native | 2 | 1.0 | Live Edit(実験的)+ Apply Changes、完全な Hot Reload 相当未達 |
| Flutter | 5 | 1.0 | Hot Reload 最速、state 保持 |
| React Native | 4 | 1.0 | Fast Refresh、Hooks の構造変更時は再マウント |
| Expo | 4 | 1.0 | Fast Refresh + Expo Go 実機テスト最速 |
| Ionic Framework | 5 | 1.0 | Vite / Webpack HMR + Live Reload、Web 感覚 |
| Capacitor | 4 | 1.0 | Web 側 HMR、ネイティブは再ビルド |
| KMP | 2 | 1.0 | Android 側 Apply Changes、iOS 側は再ビルド |
| Compose Multiplatform | 3 | 1.0 | Compose Hot Reload(改善中) |
| .NET MAUI | 4 | 1.0 | .NET Hot Reload + XAML Hot Reload |
| NativeScript | 4 | 1.0 | LiveSync で実機反映 |
| Apache Cordova | 4 | 1.0 | Web 開発と同等、ネイティブは再ビルド |
| Xamarin | 3 | 1.0 | XAML Hot Reload + .NET Hot Reload(EOL) |
| PWA | 5 | 1.0 | Web 開発の HMR 最速、ブラウザリロード即時 |

---

## D3: 初期セットアップ・ビルド速度

### アンカー

- **5**: 単一CLIで数分・数GB(Expo、Ionic)
- **4**: 公式 IDE 一発で完結、中程度(Android Native、Flutter Android のみ)
- **3**: 標準的、複数ツール併用(React Native Android のみ)
- **2**: 重い、複数 IDE / SDK 必要(Flutter Android+iOS、KMP)
- **1**: 数十GB・初回数十分(KMP + Compose MP フル構成)

### スコア表

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native | 4 | 1.0 | Android Studio 一発、15〜30 GB、初回数分 |
| Flutter | 3 | 1.0 | Android のみなら 4、+iOS で 50〜80 GB |
| React Native | 3 | 1.0 | Node + Android SDK + Metro、初回 5〜15 分 |
| Expo | 5 | 1.0 | Node + Expo CLI、5〜10 GB、EAS でクラウドビルド |
| Ionic Framework | 5 | 1.0 | npm + Ionic CLI、5〜15 GB |
| Capacitor | 5 | 1.0 | Ionic と同等、Web ビルド主体 |
| KMP | 2 | 1.0 | Android Studio + Xcode + KMP plugin、50〜80 GB |
| Compose Multiplatform | 1 | 1.0 | KMP + Compose MP フル構成、80 GB+ |
| .NET MAUI | 3 | 1.0 | Visual Studio + Workloads、40〜60 GB、初回 5〜10 分 |
| NativeScript | 3 | 1.0 | NS CLI + Android SDK |
| Apache Cordova | 4 | 1.0 | 軽量、Web 開発感覚 |
| Xamarin | 2 | 1.0 | Visual Studio + Xamarin Workloads、重め |
| PWA | 5 | 1.0 | Node + 任意バンドラ、最軽量 |

---

## D4: デバッガ・プロファイラの品質

### アンカー

- **5**: 公式統合プロファイラ充実(Android Studio Profiler、Flutter DevTools、Visual Studio Diagnostic Tools)
- **4**: ブラウザ統合 or 専用ツールあり(React DevTools、RN DevTools)
- **3**: 標準的(Chrome DevTools 経由)
- **2**: 限定的(Ionic WebView 経由)
- **1**: print デバッグ頼り

### スコア表

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native | 5 | 1.0 | Android Studio Profiler(CPU / Memory / Network / Energy / Power Rail) |
| Flutter | 5 | 1.0 | Flutter DevTools(Performance / Memory / Network / Widget Inspector / CPU Sampler) |
| React Native | 4 | 1.0 | RN DevTools(新アーキ標準)、React DevTools |
| Expo | 4 | 1.0 | RN 同等 + Expo DevTools |
| Ionic Framework | 3 | 1.0 | Chrome DevTools / Safari Web Inspector |
| Capacitor | 3 | 1.0 | Ionic 同等 |
| KMP | 4 | 1.0 | Android Studio Profiler(Android 側)+ Xcode Instruments(iOS 側) |
| Compose Multiplatform | 4 | 1.0 | KMP 同等 |
| .NET MAUI | 5 | 1.0 | Visual Studio Diagnostic Tools、App Center Analytics |
| NativeScript | 3 | 1.0 | Chrome DevTools 経由 |
| Apache Cordova | 3 | 1.0 | Chrome DevTools / Safari Inspector |
| Xamarin | 5 | 1.0 | Visual Studio Diagnostic Tools(EOL) |
| PWA | 5 | 1.0 | Chrome DevTools / Lighthouse / Performance Observer |

---

## D5: テストフレームワーク成熟度

### アンカー

- **5**: Unit / UI / E2E まで公式整備(Flutter: flutter_test / Widget Test / integration_test、Android: JUnit / Espresso / Compose Test)
- **4**: 公式 + 事実上標準サードパーティ(React Native: Jest + RNTL + Detox、.NET MAUI: xUnit + Appium)
- **3**: 標準的だが E2E はサードパーティ依存(Ionic: Jest / Vitest + Cypress / Playwright)
- **2**: 公式整備限定的(KMP: kotlin-test + プラットフォーム別)
- **1**: コミュニティ寄せ集め

### スコア表

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native | 5 | 1.0 | JUnit / MockK / Espresso / Compose Test |
| Flutter | 5 | 1.0 | flutter_test / Widget Test / integration_test |
| React Native | 4 | 1.0 | Jest + RNTL + Detox / Maestro |
| Expo | 4 | 1.0 | RN 同等、EAS で配布テストも |
| Ionic Framework | 3 | 1.0 | Jest / Vitest + Cypress / Playwright / Appium |
| Capacitor | 3 | 1.0 | Ionic 同等 |
| KMP | 2 | 1.0 | kotlin-test / JUnit / XCTest、Platform 別 |
| Compose Multiplatform | 2 | 1.0 | KMP 同等 |
| .NET MAUI | 4 | 1.0 | xUnit / NUnit / MSTest + MAUI UI Tests + Appium |
| NativeScript | 2 | 1.0 | Jasmine / Mocha + Appium |
| Apache Cordova | 2 | 1.0 | Jasmine + Appium |
| Xamarin | 4 | 1.0 | xUnit + Xamarin UITest |
| PWA | 4 | 1.0 | Jest / Vitest + Cypress / Playwright + Lighthouse |

---

## D6: CI/CD・配布の容易度

### アンカー

- **5**: 公式クラウドビルド + OTA 配布(Expo + EAS Build / Update)
- **4**: 公式 CI 連携 + 一般的サービス対応(Flutter + Codemagic、.NET MAUI + Azure DevOps)
- **3**: 標準的な CI で構築可(Android Native + GitHub Actions / Bitrise)
- **2**: 自前構築の比重高(KMP)
- **1**: CI 構築が複雑(マルチプラットフォーム + マルチランタイム)

### スコア表

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native | 3 | 1.0 | GitHub Actions / Bitrise / Firebase App Distribution |
| Flutter | 4 | 1.0 | Codemagic 公式推奨、Firebase Test Lab 連携 |
| React Native | 3 | 1.0 | Bitrise / GitHub Actions、Mac runner 必要 |
| Expo | 5 | 1.0 | EAS Build + EAS Update(OTA)で全部完結、Mac 不要 |
| Ionic Framework | 4 | 1.0 | Ionic Appflow + Live Update(Enterprise) |
| Capacitor | 4 | 1.0 | Ionic Appflow / Live Update |
| KMP | 2 | 1.0 | 自前構築主、GitHub Actions / Bitrise の Mac runner 必要 |
| Compose Multiplatform | 2 | 1.0 | KMP 同等 |
| .NET MAUI | 4 | 1.0 | Azure DevOps Pipelines / GitHub Actions / App Center 後継 |
| NativeScript | 2 | 1.0 | NS CLI + 自前 CI |
| Apache Cordova | 2 | 1.0 | 自前構築、PhoneGap Build は EOL |
| Xamarin | 3 | 1.0 | App Center(廃止予定)、Azure DevOps |
| PWA | 5 | 1.0 | 静的ホスティング + Service Worker 配布、CI 標準 |

---

## D7: ライブラリ・パッケージエコシステム

### アンカー

- **5**: 公式レジストリ厚く品質管理あり(npm: 規模最大だが玉石混交、pub.dev: スコア付き)
- **4**: 質は安定、量は中程度(Maven Central、NuGet)
- **3**: 公式中心で品質高いが量限定(Kotlin、Dart)
- **2**: 拡張前提でコミュニティ依存(Capacitor プラグイン)
- **1**: エコシステム狭い(NativeScript)

### スコア表

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native | 4 | 1.0 | Maven Central + Google Maven、品質安定 |
| Flutter | 5 | 1.0 | pub.dev はスコア付き品質管理あり |
| React Native | 5 | 1.0 | npm 最大規模、ただし玉石混交 |
| Expo | 5 | 1.0 | Expo SDK + npm |
| Ionic Framework | 4 | 1.0 | npm + Ionic 公式 + Capacitor プラグイン |
| Capacitor | 2 | 1.0 | Capacitor プラグインはコミュニティ依存、品質ばらつき |
| KMP | 3 | 1.0 | Maven Central + KMP 互換ライブラリ、量限定 |
| Compose Multiplatform | 3 | 1.0 | KMP 同等 |
| .NET MAUI | 4 | 1.0 | NuGet、品質安定、量も十分 |
| NativeScript | 1 | 1.0 | エコシステム狭い |
| Apache Cordova | 2 | 1.0 | プラグインは縮小傾向 |
| Xamarin | 3 | 1.0 | NuGet + Xamarin.Forms ライブラリ(EOL のため新規少) |
| PWA | 5 | 1.0 | npm 全部利用可 |

---

## D8: 開発OSの柔軟性

### アンカー

- **5**: Win / Mac / Linux 全部で開発・ビルド可、Mac 不要(Expo + EAS Build で iOS も Mac 不要)
- **4**: 全 OS 対応だが iOS は Mac 必要(Flutter、KMP、.NET MAUI、Ionic)
- **3**: 主要 OS 対応(Android Native、React Native: Android のみ Win 可)
- **2**: 特定 OS 縛り部分あり(.NET MAUI iOS は Mac 経由)
- **1**: 特定 OS 必須(Swift on macOS のみ)

### スコア表

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native | 3 | 1.0 | Win / Mac / Linux で開発可、Android 専用なら 5 だが iOS 出力不可で 3 |
| Flutter | 4 | 1.0 | Win / Mac / Linux 開発可、iOS ビルドは Mac 必須 |
| React Native | 3 | 1.0 | Android は全 OS、iOS は Mac 必須 |
| Expo | 5 | 1.0 | EAS Build で iOS も Mac 不要、Windows 開発時の最大の利点 |
| Ionic Framework | 4 | 1.0 | Web 開発感覚、iOS ビルドのみ Mac 必須 |
| Capacitor | 4 | 1.0 | Ionic 同等 |
| KMP | 4 | 1.0 | Win / Mac / Linux 開発可、iOS は Mac + Xcode 必須 |
| Compose Multiplatform | 4 | 1.0 | KMP 同等 |
| .NET MAUI | 4 | 1.0 | Win / Mac で開発可、iOS は Mac 経由 |
| NativeScript | 3 | 1.0 | Win / Mac / Linux 対応、iOS は Mac 必須 |
| Apache Cordova | 3 | 1.0 | 同上 |
| Xamarin | 2 | 1.0 | Visual Studio Win 中心、iOS は Mac リモート(EOL) |
| PWA | 5 | 1.0 | OS 選ばず、ブラウザがあれば動く |

---

## 集計

| FW | D1 | D2 | D3 | D4 | D5 | D6 | D7 | D8 | 単純平均 | 重み付き平均 | ★換算 |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Android Native | 5 | 2 | 4 | 5 | 5 | 3 | 4 | 3 | 3.88 | 3.88 | ★★★★ |
| Flutter | 4 | 5 | 3 | 5 | 5 | 4 | 5 | 4 | 4.38 | 4.38 | ★★★★ |
| React Native | 3 | 4 | 3 | 4 | 4 | 3 | 5 | 3 | 3.63 | 3.63 | ★★★★ |
| Expo | 3 | 4 | 5 | 4 | 4 | 5 | 5 | 5 | 4.38 | 4.38 | ★★★★ |
| Ionic Framework | 4 | 5 | 5 | 3 | 3 | 4 | 4 | 4 | 4.00 | 4.00 | ★★★★ |
| Capacitor | 4 | 4 | 5 | 3 | 3 | 4 | 2 | 4 | 3.63 | 3.63 | ★★★★ |
| KMP | 4 | 2 | 2 | 4 | 2 | 2 | 3 | 4 | 2.88 | 2.88 | ★★★ |
| Compose Multiplatform | 3 | 3 | 1 | 4 | 2 | 2 | 3 | 4 | 2.75 | 2.75 | ★★★ |
| .NET MAUI | 5 | 4 | 3 | 5 | 4 | 4 | 4 | 4 | 4.13 | 4.13 | ★★★★ |
| NativeScript | 2 | 4 | 3 | 3 | 2 | 2 | 1 | 3 | 2.50 | 2.50 | ★★★ |
| Apache Cordova | 3 | 4 | 4 | 3 | 2 | 2 | 2 | 3 | 2.88 | 2.88 | ★★★ |
| Xamarin | 4 | 3 | 2 | 5 | 4 | 3 | 3 | 2 | 3.25 | 3.25 | ★★★ |
| PWA | 4 | 5 | 5 | 5 | 4 | 5 | 5 | 5 | 4.75 | 4.75 | ★★★★★ |

> 注: 各セルのスコアは上記アンカーに基づく初期値(2026-04-28 時点)。

---

## 更新履歴

- **2026-04-28**: 初版作成。D1〜D8 のアンカー定義と 13 FW の初期スコアを記載。

