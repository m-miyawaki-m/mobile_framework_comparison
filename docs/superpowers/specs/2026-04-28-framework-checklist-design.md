# モバイルフレームワーク選定 評価チェックシート 設計仕様

- **作成日**: 2026-04-28
- **対象成果物**: `docs/checks/` 配下のマークダウン群(汎用評価チェックシートのたたき台)
- **既存の関連資料**:
  - [`docs/framework_comparison/selection_criteria.md`](../../framework_comparison/selection_criteria.md) — 観点 A〜H の母集団
  - [`docs/framework_comparison/framework_profiles.md`](../../framework_comparison/framework_profiles.md) — FW/言語別の一次データ
  - [`docs/framework_comparison/framework_native_features.md`](../../framework_comparison/framework_native_features.md) — Part1 言語×プラットフォーム対応マトリクス、Part2 機能別ライブラリ表

---

## 1. 目的

モバイルアプリ向けフレームワーク・言語選定にあたり、**特定プロジェクトの事情に依存しない汎用評価軸** を整理した再利用可能なチェックシートを構築する。利用者は本シートに自プロジェクトの優先度(重み)を当てて読み替えることで、選定根拠を定量的に示せる。

### 設計原則

| 原則 | 内容 |
|---|---|
| 汎用性 | チームスキル・期間・既存資産・配布要件などの **プロジェクト固有事情を持ち込まない**(別軸として外出し) |
| 絶対評価 | 他FWとの相対比較ではなく、業界水準と照らした素の値で点数を付ける |
| アンカー方式 | 1〜5 各点に **言語化された判定基準** を付与し、誰が記入してもブレない |
| 再利用 | スコアは固定の事実として記載し、利用者は **重みのみ** を編集してプロジェクト適合度を計算 |
| 一次データ参照 | スコアの根拠は `framework_profiles.md` 等の既存一次データを引用 |

### 非目標

- プロジェクト固有事情(Java資産・Windows開発・Android永続 等)に基づく推奨は行わない。それは別途「プロジェクト適合性チェック」として作成する想定。
- 個別FW/言語の網羅は MVP の対象外。第一版は主要FW/言語のみ(後述)。

---

## 2. スコープ

### 評価対象

**評価は FW 単位で行う**。言語は各 FW の主言語として B 列に表記され、行としては独立しない。
評価対象は [`framework_profiles.md`](../../framework_comparison/framework_profiles.md) に記載の 13 フレームワーク:

| # | FW | 主言語(B列) | 備考 |
|---|---|---|---|
| 1 | Android ネイティブ(Jetpack Compose + Android SDK) | Kotlin | 業務アプリ第一推奨 |
| 2 | Flutter | Dart | |
| 3 | React Native | JavaScript / TypeScript | 素のRN |
| 4 | Expo(React Native + Expo SDK) | JavaScript / TypeScript | EAS Build含む |
| 5 | Ionic Framework | TypeScript(Angular/React/Vue) | UI層 |
| 6 | Capacitor | TypeScript | ネイティブブリッジ層 |
| 7 | Kotlin Multiplatform (KMP) | Kotlin | ロジック共通化 |
| 8 | Compose Multiplatform | Kotlin | UI共通化、KMP補完 |
| 9 | .NET MAUI | C# | |
| 10 | NativeScript | TypeScript / JavaScript | |
| 11 | Apache Cordova | JavaScript | 参考・縮小傾向 |
| 12 | Xamarin | C# | EOL(2024-05、参考のみ) |
| 13 | PWA | JavaScript / TypeScript | 参考、ブラウザ前提 |

**EOL/レガシー扱いの方針**: Cordova / Xamarin / PWA も評価対象に含める。低スコアが付くことで「なぜ採用すべきでないか」の根拠資料として機能する。

### 言語の扱い

- 言語は概要シートで FW 行の B 列(主言語)として表示される
- 言語自体の B/D/L スコアは付けない(同じ言語でも採用 FW で評価が変わるため)
- 言語固有の事実情報(対応領域・PWA対応・代表採用例 等)は `04_language_features.md` にカード形式で残す(FW 行から参照可能)

### 評価対象外

- Swift / SwiftUI(iOS 主軸プロジェクト向け、`framework_profiles.md` でも参考扱い)
- Unity / Unreal(ゲーム特化、業務アプリ用途外)

これらは将来の拡張ポイントとして 11 章に記載。

---

## 3. 成果物のファイル構成

```
docs/checks/
├── README.md                       使い方・スコアリング規約・プロジェクト固有事情との合成方法
├── 00_overview.md                  サマリ + 端末ターゲット表 + 3種★点
├── 01_business_suitability.md      業務適正度 B1〜B8(1〜5点 + アンカー)
├── 02_learning_difficulty.md       習得難易度 L1〜L8(1〜5点 + アンカー、5=易)
├── 03_developer_experience.md      開発体験・開発環境 D1〜D8(1〜5点 + アンカー)
└── 04_language_features.md         言語仕様(事実情報、スコアなし)
```

### 各ファイルの位置付け

| ファイル | 種別 | 目的 |
|---|---|---|
| `README.md` | ガイド | 評価ルール、スコア計算、利用ワークフロー |
| `00_overview.md` | サマリ | 1行/対象 で端末対応 + 3種★点を一覧 |
| `01〜03` | 詳細スコア | 8項目 × 1〜5点、根拠備考、合計・平均、★換算 |
| `04` | 事実 | 対応領域・代表採用例・言語特性(スコアなし) |

---

## 4. スコアリング規約(全シート共通)

### 4.1 スケールと方向

- **1〜5 点の整数評価**(0.5刻みは使わない)
- **5 = 最良**(習得難易度では 5 = 最も易しい)
- 全項目で「5 = 望ましい状態」になるよう項目名を整える(例: 「分散度」「予測可能性」「シンプルさ」「容易度」)

### 4.2 仮想ペルソナ

評価のブレを抑えるため、以下を共通の前提とする:

- **業務適正度・開発体験**: 「中規模(10〜50画面)の Android 業務アプリを 2〜3 年スパンで保守する開発組織」を仮定
- **習得難易度**: 「他言語経験はあるが当該言語は未経験の一般的なプログラマー」を仮定

これらに当てはまらないプロジェクト事情(チーム既存スキル等)は、利用者が **重み** を調整することで反映する設計。

### 4.3 アンカー方式

各項目には冒頭に「点数 → 状態の言語化(代表例つき)」を 5 行で記述する。

例:

```markdown
### B1: リリース・サポートの予測可能性

- 5: 明示LTSあり + 規則的リリース + 長期サポート保証(Java OpenJDK、Angular、.NET)
- 4: 明示LTSあり or 規則的リリース のどちらか(.NET MAUI、Kotlin)
- 3: 後方互換維持で事実上安定だが明示LTSなし(TypeScript、React)
- 2: LTSなし・リリース不規則で追従負荷あり(Flutter、React Native)
- 1: EOL or 縮小傾向(Xamarin、Cordova、PhoneGap)
```

### 4.4 スコア表のフォーマット

各スコア表(01〜03)は以下の列構成:

| 対象 | スコア | 重み | 備考(根拠) |
|---|---|---|---|

- **対象**: FW名 or 言語名
- **スコア**: 1〜5(整数)/ N/A
- **重み**: 利用者が調整(デフォルト全項目 1.0)
- **備考**: 点数を付けた根拠を 1〜2 行(`framework_profiles.md` 等への参照を含めても可)

### 4.5 集計と★換算

各シート末尾に集計行を設置:

```markdown
| 集計 | 単純平均 | 重み付き平均 | ★換算 |
|---|---|---|---|
| Kotlin | 4.25 | 4.50 | ★★★★★ |
```

**★換算ルール**:

| 平均値 | ★ |
|---|---|
| 4.5 以上 | ★★★★★(5) |
| 3.5〜4.49 | ★★★★(4) |
| 2.5〜3.49 | ★★★(3) |
| 1.5〜2.49 | ★★(2) |
| 1.5 未満 | ★(1) |

### 4.6 N/A の扱い

該当しない項目は **N/A** とし、平均計算の **分母から除外**(0点扱いではない)。

例: 言語単体評価で「ネイティブHWアクセス」を問う場合、当該言語を使う FW 経由の話なので N/A とする。

---

## 5. 概要シート(`00_overview.md`)

### 5.1 列定義

| 列 | 内容 | 値 |
|---|---|---|
| FW/言語名 | 評価対象 | 文字列 |
| 主言語 | FWの場合の主要言語 | 文字列 |
| 全活用ターゲット | 公式・第一級サポートで動作 | 下記の端末カテゴリから複数 |
| 限定利用ターゲット | 拡張ライブラリ経由で動作 | 同上 |
| 限定利用ライブラリ等 | 限定利用に必要な拡張・ラッパ | 文字列(例: Electron、Tauri、Capacitor、KMP) |
| 業務適正度 | `01_business_suitability.md` の総合★ | ★1〜★5 |
| 開発体験 | `03_developer_experience.md` の総合★ | ★1〜★5 |
| 習得難易度 | `02_learning_difficulty.md` の総合★ | ★1〜★5 |

### 5.2 端末カテゴリ(6種)

- **Android**(モバイル)
- **iOS**(モバイル)
- **Windows**(デスクトップ)
- **macOS**(デスクトップ)
- **Linux**(デスクトップ)
- **組込機器**(POS、車載、サイネージ、業務HT 等の物理機器)

**Web(ブラウザ)を含めない理由**: 概要シートは「物理端末/OS」での動作可否に焦点を絞る。Web対応・PWA対応は `04_language_features.md` に事実として記載。

### 5.3 記入例

| FW | 主言語 | 全活用ターゲット | 限定利用ターゲット | 限定利用ライブラリ等 | 業務適正度 | 開発体験 | 習得難易度 |
|---|---|---|---|---|---|---|---|
| Android ネイティブ | Kotlin | Android | — | — | ★★★★★ | ★★★★ | ★★★★ |
| Flutter | Dart | Android, iOS | Windows, macOS, Linux, 組込機器 | Flutter Desktop / Embedded | ★★★★ | ★★★★★ | ★★★ |
| React Native | JavaScript / TypeScript | Android, iOS | Windows, macOS | RN for Windows/macOS | ★★★ | ★★★★ | ★★★ |
| Expo | JavaScript / TypeScript | Android, iOS | — | — | ★★★ | ★★★★★ | ★★★★ |
| Ionic Framework | TypeScript | Android, iOS | — | (UI層、配布は Capacitor 等経由) | ★★★ | ★★★★ | ★★★ |
| Capacitor | TypeScript | Android, iOS | Windows, macOS, Linux | Capacitor Electron / Tauri |  ★★★★ | ★★★★ | ★★★ |
| KMP | Kotlin | Android, iOS | Windows, macOS, Linux | Compose Multiplatform | ★★★★ | ★★★ | ★★ |
| Compose Multiplatform | Kotlin | Android, iOS, Windows, macOS, Linux | — | — | ★★★ | ★★★ | ★★ |
| .NET MAUI | C# | Android, iOS, Windows, macOS | Linux | サードパーティ実装 | ★★★★ | ★★★★ | ★★★ |
| NativeScript | TypeScript / JavaScript | Android, iOS | — | — | ★★ | ★★ | ★★ |
| Apache Cordova | JavaScript | Android, iOS | — | — | ★★ | ★★ | ★★★ |
| Xamarin | C# | Android, iOS | — | — | ★(EOL) | — | — |
| PWA | JavaScript / TypeScript | (Web ブラウザ) | Android, iOS, Windows, macOS, Linux | OS の PWA 対応(機能制限あり) | ★★ | ★★★ | ★★★★ |

注: 上の★は記入例であり、確定値ではない。実装時に各シートで根拠付きで決定する。EOL扱いの Xamarin は B1 で 1 点となるため総合★も低くなり、本シートでは「採用すべきでない根拠資料」として残す。

---

## 6. 業務適正度シート(`01_business_suitability.md`)

### 6.1 評価項目(B1〜B8)

| # | 項目名 | 評価軸の要点 |
|---|---|---|
| B1 | リリース・サポートの予測可能性 | LTS有無・サポート期限・リリースサイクル規則性を統合 |
| B2 | 開発企業・コミュニティの分散度 | 後援者の多様性、Bus Factor の裏返し |
| B3 | ネイティブハードウェアアクセス | 業務HT周辺機器ベンダーSDK(Zebra/Honeywell/Datalogic)の公式言語前提度 |
| B4 | セキュリティ・脆弱性対応体制 | CVE 窓口、対応速度、OS暗号API/Keystore直結度 |
| B5 | 商用サポート可否 | SLA 付き商用契約の選択肢の有無・厚み |
| B6 | ライセンス安定性 | 商用条件変更の前例有無・ライセンス枯れ度 |
| B7 | 後方互換性(破壊的変更頻度の低さ) | メジャー間の互換維持ポリシー |
| B8 | エンタープライズ採用実績 | 業務基幹システムでの採用厚み |

### 6.2 各項目のアンカー

#### B1: リリース・サポートの予測可能性

- 5: 明示LTSあり + 規則的リリース + 長期サポート(Java OpenJDK、Angular、.NET)
- 4: 明示LTSあり or 規則的リリース のどちらか(Kotlin、.NET MAUI)
- 3: 後方互換維持で事実上安定、明示LTSなし(TypeScript、React)
- 2: LTSなし・リリース不規則で追従負荷あり(Flutter、React Native、Expo)
- 1: EOL or 縮小傾向(Xamarin、Cordova、PhoneGap)

#### B2: 開発企業・コミュニティの分散度

- 5: 多ベンダー体制 or 巨大独立コミュニティ(Java OpenJDK、React Native)
- 4: 主導企業 + 強力な複数企業/財団参加(.NET、TypeScript、React)
- 3: 単一企業主導だが財団化・企業規模大(Kotlin、Angular)
- 2: 単一企業の戦略依存(Flutter、Dart、Ionic)
- 1: 個人 or 小チーム依存(Vue、NativeScript)

#### B3: ネイティブハードウェアアクセス

- 5: 業務HT周辺機器ベンダーSDKが公式言語前提(Kotlin/Java + Android)
- 4: 主要機能はFW公式 or 高品質ラッパで網羅(Flutter)
- 3: コミュニティラッパ経由で実装可だが品質ばらつき(RN/Expo)
- 2: 拡張プラグイン経由で限定的(Ionic/Capacitor)
- 1: 原理的不可 or 重大制約(PWA)

#### B4: セキュリティ・脆弱性対応体制

- 5: 月次CVE窓口 + OS暗号API/Keystore直結 + 公式セキュリティチーム(Android/Jetpack、OpenJDK)
- 4: 公式セキュリティ窓口あり、対応速度高(Kotlin、Flutter、Angular)
- 3: GitHub Advisory ベース、対応速度中(React Native、TypeScript)
- 2: コミュニティ依存で対応速度ばらつき(Vue、サードパーティ多用FW)
- 1: 玉石混交の3rd party依存が中核(Capacitor 旧プラグイン、Cordova)

#### B5: 商用サポート可否

- 5: 複数ベンダーが SLA 付き商用サポート提供(Java: Oracle/Red Hat/Azul/AWS)
- 4: 単独ベンダーが商用サポート提供(.NET、Kotlin/IntelliJ、Ionic Enterprise)
- 3: 公式サポートはないがコンサル企業経由で実質可(React Native: Callstack 等)
- 2: 限定的・ベンダー間接(Flutter Partner 経由)
- 1: 商用サポートなし(コミュニティのみ)

#### B6: ライセンス安定性

- 5: Apache/MIT/BSD で枯れている(Kotlin、Flutter、TypeScript、.NET)
- 4: GPL系だが Classpath Exception 等で実質クリア(OpenJDK)
- 3: ベンダーライセンス併存だが実用上問題なし(Oracle JDK with NFTC)
- 2: 商用条件が複雑、規模で課金(Unreal Engine の売上ロイヤリティ)
- 1: 過去に一方的な商用条件変更の前例あり(Unity ランタイム料金騒動)

#### B7: 後方互換性(破壊的変更頻度の低さ)

- 5: 10年単位の互換維持文化(Java「Write Once, Run Forever」)
- 4: メジャーアップデートでも軽微な修正で済む(React、TypeScript)
- 3: 規則的だがマイグレーション必要(Angular、.NET)
- 2: メジャー毎に大規模Migration Guide が出る(Flutter、Kotlin 2.0)
- 1: メジャー間で事実上再書き(Vue 2→3、React Native 新アーキ移行)

#### B8: エンタープライズ採用実績

- 5: 業務基幹で広範採用、Fortune 500 多数(Java、Android Native)
- 4: エンタープライズ採用が広く認知(.NET、Kotlin、TypeScript)
- 3: 業務利用は伸びているが業界別偏り(Flutter、React Native)
- 2: 採用例はあるがニッチ(Ionic、KMP)
- 1: 業務利用ほぼ未検証 or 縮小(NativeScript、Cordova 新規)

---

## 7. 習得難易度シート(`02_learning_difficulty.md`)

### 7.1 評価項目(L1〜L8、5 = 最も易しい)

| # | 項目名 | 評価軸の要点 |
|---|---|---|
| L1 | 構文・記法のシンプルさ | 暗黙ルール・慣用句の少なさ |
| L2 | 型システムの取っつきやすさ | 型推論・null safety の自然さ、ジェネリクス強制度 |
| L3 | 並行・非同期モデルの理解負荷 | 同期的記述に近いほど易、複数モデル混在は難 |
| L4 | パラダイム混在度の少なさ | OOP/関数型/リアクティブの重なりが少ないほど易 |
| L5 | エコシステム・ツーリング学習量 | ビルドツール・依存管理・バンドラの重層度 |
| L6 | 公式ドキュメント・教材充実度 | 日本語含めた教材の量と質 |
| L7 | AI/LLM補完の精度 | 学習データ量と型情報の活用度 |
| L8 | トラブルシューティング容易度 | エラーメッセージ可読性、デバッガ統合度 |

### 7.2 各項目のアンカー(方向性のみ、詳細値は実装時に決定)

#### L1: 構文・記法のシンプルさ

- 5: 数日で読み書きできる、暗黙ルールが少ない(Java、C#、Kotlin)
- 4: 標準的な構文だが慣用句あり(Dart、TypeScript)
- 3: 言語機能多くスタイル分岐あり(Swift)
- 2: 暗黙ルール多数(JavaScript: this 束縛・プロトタイプ)
- 1: メタ構文・特殊記法多用(高度Scala、Rust マクロ等は対象外だが参考)

#### L2: 型システムの取っつきやすさ

- 5: 強い型推論 + null safety が自明(Kotlin、Dart 3.0+)
- 4: 静的型 + 型推論あり(TypeScript、C#、Swift)
- 3: 静的型だが冗長(Java の旧バージョン)
- 2: 動的型に後付け型(JavaScript + JSDoc)
- 1: 動的型のみ(素のJavaScript)

#### L3: 並行・非同期モデルの理解負荷

- 5: 同期的記述に近い(Kotlin coroutine + suspend、Dart async/await)
- 4: 単一モデルで習得可(JavaScript Promise/async)
- 3: 複数モデル併存だが選択明確(Java の Future/CompletableFuture/Reactive)
- 2: モデル選択でコード分岐(RxJava + coroutine 混在)
- 1: スレッド直接管理が必要(Java 古い書き方)

#### L4: パラダイム混在度の少なさ

- 5: 単一パラダイムで完結(Java OOP)
- 4: OOP + 関数型ハイブリッドだがOOP主導(Kotlin、C#)
- 3: 複数パラダイム選択可だがデフォルト明確(Swift、Dart)
- 2: 関数型・リアクティブ・OOP 混在(TypeScript + React + Hooks)
- 1: フレームワーク毎にパラダイムが変わる(Angular vs React vs Vue で要再学習)

#### L5: エコシステム・ツーリング学習量

- 5: 単一CLIで完結(Flutter `flutter` コマンド)
- 4: 標準ビルドツールで完結(Kotlin: Gradle、C#: dotnet CLI)
- 3: ビルドツール + 依存管理が分離(Java: Gradle/Maven)
- 2: バンドラ + パッケージマネージャ + ランタイム多重(RN: Metro + npm + Gradle + Xcode)
- 1: 複数バンドラ・複数ランタイム選択(Web 全般: Vite/Webpack/Rollup × npm/pnpm/yarn)

#### L6: 公式ドキュメント・教材充実度

- 5: 日本語含めて潤沢(Java、Kotlin、React、Vue、Angular、TypeScript)
- 4: 英語豊富 + 日本語コミュニティ活発(Flutter、Dart)
- 3: 英語豊富だが日本語限定的(C#、Swift)
- 2: 英語の散発記事中心(KMP、Compose Multiplatform)
- 1: 英語でも限定的(NativeScript、レガシFW)

#### L7: AI/LLM補完の精度

- 5: 型情報多く誤補完少(TypeScript、Kotlin)
- 4: 学習データ量が多く高精度(Java、JavaScript、C#)
- 3: 中程度(Dart、Swift)
- 2: 学習データ少なく型情報も限定的(KMP 新機能、Compose MP)
- 1: 動的すぎて補完信頼性低(素の JavaScript の動的構造)

#### L8: トラブルシューティング容易度

- 5: エラーメッセージ自明 + 統合デバッガ強力(Kotlin + Android Studio、C# + Visual Studio)
- 4: スタックトレース読みやすい(Java、Dart)
- 3: 標準的(TypeScript、Swift)
- 2: スタックトレース難読 or デバッガ制約あり(JavaScript の minified 環境)
- 1: 多層エラーで原因特定困難(RN: Metro + ネイティブ層 + Xcode/Gradle 多層)

---

## 8. 開発体験シート(`03_developer_experience.md`)

### 8.1 評価項目(D1〜D8、5 = 最も快適)

| # | 項目名 | 評価軸の要点 |
|---|---|---|
| D1 | IDE・ツーリング統合度 | 公式IDEの補完・リファクタ・Navigate-to-def |
| D2 | Hot Reload / フィードバックループ | 反映速度と state 保持 |
| D3 | 初期セットアップ・ビルド速度 | 初回構築の手間とディスク占有、CI ビルド時間 |
| D4 | デバッガ・プロファイラの品質 | 公式統合プロファイラの充実度 |
| D5 | テストフレームワーク成熟度 | Unit/UI/E2E の公式整備度 |
| D6 | CI/CD・配布の容易度 | 公式クラウドビルド・OTA配布の有無 |
| D7 | ライブラリ・パッケージエコシステム | 公式レジストリの厚み・品質 |
| D8 | 開発OSの柔軟性 | Win/Mac/Linux 全部で開発・ビルドできるか |

### 8.2 各項目のアンカー(方向性)

#### D1: IDE・ツーリング統合度

- 5: 公式IDEで補完/リファクタ/Navigate-to-def 完璧(Kotlin + Android Studio、Java + IntelliJ、C# + Visual Studio、TypeScript + VS Code)
- 4: 公式拡張で十分実用(Flutter + VS Code/AS、Dart)
- 3: 主要操作はカバー(React Native + VS Code)
- 2: エディタ任せで補完精度限定(NativeScript)
- 1: IDE 統合が弱い or なし

#### D2: Hot Reload / フィードバックループ

- 5: 1秒未満で反映 + state 保持(Flutter Hot Reload、Ionic Web HMR)
- 4: 高速だが state リセットあり(React Native Fast Refresh、Expo、.NET MAUI Hot Reload)
- 3: 中速、部分的反映(SwiftUI Preview)
- 2: Apply Changes 等の制約多い(Android Native Live Edit)
- 1: フルリビルド必須

#### D3: 初期セットアップ・ビルド速度

- 5: 単一CLIで数分・数GB(Expo、Ionic)
- 4: 公式IDE一発で完結、中程度(Android Native、Flutter Android のみ、.NET MAUI)
- 3: 標準的、複数ツール併用(React Native Android のみ)
- 2: 重い、複数IDE/SDK必要(Flutter Android+iOS、KMP)
- 1: 数十GB・初回数十分(KMP + Compose MP フル構成)

#### D4: デバッガ・プロファイラの品質

- 5: 公式統合プロファイラ充実(Android Studio Profiler、Flutter DevTools、Visual Studio Diagnostic Tools)
- 4: ブラウザ統合 or 専用ツールあり(React DevTools、RN DevTools)
- 3: 標準的(Chrome DevTools 経由)
- 2: 限定的(Ionic WebView 経由)
- 1: print デバッグ頼り

#### D5: テストフレームワーク成熟度

- 5: Unit/UI/E2E まで公式整備(Flutter: flutter_test/Widget Test/integration_test、Android: JUnit/Espresso/Compose Test)
- 4: 公式 + 事実上標準サードパーティ(React Native: Jest + RNTL + Detox、.NET MAUI: xUnit + Appium)
- 3: 標準的だがE2Eはサードパーティ依存(Ionic: Jest/Vitest + Cypress/Playwright)
- 2: 公式整備限定的(KMP: kotlin-test + プラットフォーム別)
- 1: コミュニティ寄せ集め

#### D6: CI/CD・配布の容易度

- 5: 公式クラウドビルド + OTA 配布(Expo + EAS Build/Update)
- 4: 公式CI連携 + 一般的サービス対応(Flutter + Codemagic、.NET MAUI + Azure DevOps)
- 3: 標準的なCIで構築可(Android Native + GitHub Actions/Bitrise)
- 2: 自前構築の比重高(KMP)
- 1: CI 構築が複雑(マルチプラットフォーム + マルチランタイム)

#### D7: ライブラリ・パッケージエコシステム

- 5: 公式レジストリ厚く品質管理あり(npm: 規模最大だが玉石混交、pub.dev: スコア付き)
- 4: 質は安定、量は中程度(Maven Central、NuGet)
- 3: 公式中心で品質高いが量限定(Kotlin、Dart)
- 2: 拡張前提でコミュニティ依存(Capacitor プラグイン)
- 1: エコシステム狭い(NativeScript)

#### D8: 開発OSの柔軟性

- 5: Win/Mac/Linux 全部で開発・ビルド可、Mac 不要(Expo + EAS Build で iOS も Mac 不要)
- 4: 全OS対応だが iOS は Mac 必要(Flutter、KMP、.NET MAUI、Ionic)
- 3: 主要OS対応(Android Native、React Native: Android のみ Win 可)
- 2: 特定OS縛り部分あり(.NET MAUI iOS は Mac 経由)
- 1: 特定OS必須(Swift on macOS のみ)

---

## 9. 言語仕様シート(`04_language_features.md`)

スコアリングはせず、**事実情報のみ**を言語ごとにカード形式で記述。

### 9.1 各カードの項目

| 項目 | 内容 |
|---|---|
| 対応プラットフォーム | モバイル(Android/iOS)/ Web / サーバ / デスクトップ / 組込 / ゲーム / CLI / データサイエンス の対応度(◎/○/△/×) |
| PWA対応 | 該当する場合のみ(Service Worker・manifest 対応度) |
| 代表的な採用例 | 企業・著名OSS、業界別 |
| 主要コンパイル方式 | AOT / JIT / インタプリタ / バイトコード |
| 主要パラダイム | OOP / 関数型 / 手続き型 / リアクティブ |
| 型システム特徴 | 静的/動的、null safety、型推論、ジェネリクス |
| メモリ管理 | GC / RC / ARC / Ownership |
| 標準ライブラリの範囲 | 言語単体での到達範囲 |
| 主要相互運用先 | FFI / JNI / Platform Channel など |
| 言語の設計哲学 | 安全性 / 表現力 / 簡潔さ |
| 関連参照 | `framework_profiles.md` 該当節へのリンク |

### 9.2 カード例(Kotlin)

```markdown
## Kotlin

| 項目 | 値 |
|---|---|
| 対応プラットフォーム | Android◎ / iOS○(KMP)/ Web△(Kotlin/JS)/ サーバ◎(Ktor/Spring)/ デスクトップ○(Compose Desktop)/ 組込△ / ゲーム△ / CLI○ / データ△ |
| PWA対応 | — |
| 代表的な採用例 | Google Android公式、Pinterest、Square、Trello、Netflix、JetBrains 製品群、Gradle |
| 主要コンパイル方式 | JVMバイトコード / Kotlin Native / Kotlin/JS / Kotlin/Wasm |
| 主要パラダイム | OOP + 関数型ハイブリッド |
| 型システム特徴 | 静的型 + null safety + 型推論 |
| メモリ管理 | JVM の場合 GC / Native は ARC 風 |
| 標準ライブラリの範囲 | コルーチン・コレクション・シリアライゼーション・標準IO |
| 主要相互運用先 | Java(完全相互運用)、JNI、Swift/Obj-C(KMP経由) |
| 言語の設計哲学 | Java互換性と簡潔さの両立 |
| 関連参照 | [`framework_profiles.md#kotlin`](../framework_comparison/framework_profiles.md) |
```

---

## 10. README.md(使い方ガイド)

### 10.1 構成

| セクション | 内容 |
|---|---|
| 目的 | 本シートが何で何でないか |
| 評価対象一覧 | MVP の対象 FW/言語 |
| スコアリング規約 | 4章の要約 |
| 利用ワークフロー | 後述 |
| プロジェクト固有事情との合成方法 | 後述 |
| 更新ポリシー | 採用時点で再確認すべき項目 |

### 10.2 利用ワークフロー(README に記載)

1. `00_overview.md` で候補を3〜5に絞る
2. `04_language_features.md` で対応プラットフォームと汎用性を確認(スキル投資の文脈)
3. `01〜03` の詳細スコアで根拠を確認
4. プロジェクトの優先度に応じて **重み列を編集**(コピーして自プロジェクトリポジトリへ持ち込む想定)
5. 重み付き平均で順位を再計算 → 上位を最終候補とする

### 10.3 プロジェクト固有事情との合成方法

本シートでは扱わないが、選定時には以下の **別軸チェック** を併用する想定:

| プロジェクト固有事情 | 合成方法 |
|---|---|
| チーム既存スキル(Java/JS/Kotlin 等) | L1〜L8 の重みを既存スキル親和度で調整 |
| 開発OS制約(Windows のみ等) | D8 の重みを上げる |
| 既存コード資産(Web/Android 等) | 流用可能な FW/言語の B8 採用実績やエコシステムを優先 |
| 配布・運用要件(Store/MDM/直配布) | B3、B4、B5 の重み調整 |
| 業界規制(金融/医療/公共) | B5(商用サポート)の重みを最大に |

これらのテンプレ列を README で例示する。

### 10.4 更新ポリシー

- 各 FW/言語の **採用時点で公式ソースで再確認**(LTS日付・EOL日付・ライセンス・サポート契約条件は変動)
- 一次データは `framework_profiles.md` を更新し、本チェックシートは派生として再生成
- 主要 FW のメジャーアップデート(LTS切替、ライセンス変更、EOL発表等)があった場合は当該項目を再評価

---

## 11. 拡張ポイント(将来の追加候補)

| 拡張 | 概要 |
|---|---|
| Swift / SwiftUI 評価 | iOS 主軸プロジェクト向けに対象拡大 |
| ゲーム系(Unity / Unreal) | 業務外用途として別カテゴリで評価 |
| プロジェクト適合性チェック | チームスキル・期間・既存資産を入力して B/D/L の重みを自動算出するテンプレ |
| 機能別ライブラリスコア | `framework_native_features.md` Part2 を 23 機能カテゴリ × FW でスコア化 |
| 重み付け感度分析 | 重みを変動させた場合の順位変化を可視化(Excel テンプレ等) |

---

## 12. 関連資料

- [`docs/framework_comparison/selection_criteria.md`](../../framework_comparison/selection_criteria.md) — A〜H 観点の母集団
- [`docs/framework_comparison/framework_profiles.md`](../../framework_comparison/framework_profiles.md) — FW/言語別ファクトシート
- [`docs/framework_comparison/framework_native_features.md`](../../framework_comparison/framework_native_features.md) — Part1 言語×プラットフォーム、Part2 機能×FW
- [`docs/android_business_app_framework_comparison.md`](../../android_business_app_framework_comparison.md) — Android 業務アプリ視点の総合比較
- [`docs/pwa_vs_native/pwa_vs_native_criteria.md`](../../pwa_vs_native/pwa_vs_native_criteria.md) — PWA vs ネイティブ判断基準

---

## 13. 更新履歴

- **2026-04-28**: 初版作成。5ファイル構成・スコアリング規約・各シート8項目を定義。
