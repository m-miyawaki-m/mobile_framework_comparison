# Framework Selection Checklist Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** `docs/checks/` 配下に 6 ファイルのモバイル FW 選定評価チェックシート(マークダウン)を作成する。

**Architecture:** 5 つのシート(概要/業務適正度/習得難易度/開発体験/言語仕様)+ README で構成。各スコアシートは 8 項目 × 13 FW を 1〜5 点アンカー方式で評価、N/A は分母調整、重み列で利用者が後から調整可能。スコアの一次データは既存の `docs/framework_comparison/framework_profiles.md` 等から派生。

**Tech Stack:** Markdown(GitHub Flavored)、Git。生成ツールやテストフレームワークなし、ドキュメンテーションのみ。

**Spec:** [`docs/superpowers/specs/2026-04-28-framework-checklist-design.md`](../specs/2026-04-28-framework-checklist-design.md)

**Source data:**
- `docs/framework_comparison/framework_profiles.md` — FW/言語別ファクトシート
- `docs/framework_comparison/selection_criteria.md` — F1〜F10/G1〜G13 観点母集団
- `docs/framework_comparison/framework_native_features.md` — Part1 言語×プラットフォーム、Part2 機能×FW
- `docs/android_business_app_framework_comparison.md` — 業務観点比較
- `docs/pwa_vs_native/pwa_vs_native_criteria.md` — PWA 評価データ

**Evaluation targets (13 FWs):**

1. Android Native(Jetpack Compose + Android SDK)/ Kotlin
2. Flutter / Dart
3. React Native / JavaScript / TypeScript
4. Expo(RN + Expo SDK)/ JavaScript / TypeScript
5. Ionic Framework / TypeScript
6. Capacitor / TypeScript
7. KMP(Kotlin Multiplatform)/ Kotlin
8. Compose Multiplatform / Kotlin
9. .NET MAUI / C#
10. NativeScript / TypeScript / JavaScript
11. Apache Cordova / JavaScript
12. Xamarin(EOL)/ C#
13. PWA / JavaScript / TypeScript

---

## File Structure

```
docs/checks/
├── README.md                       Task 1 で作成
├── 00_overview.md                  Task 9 で作成(他シート完成後)
├── 01_business_suitability.md      Task 3 + 4 で作成(8 項目 + 集計)
├── 02_learning_difficulty.md       Task 5 + 6 で作成
├── 03_developer_experience.md      Task 7 + 8 で作成
└── 04_language_features.md         Task 2 で作成
```

各スコアシート(01/02/03)の内部構造:

```markdown
# [Sheet Title]

## 1. 評価ルール(README参照)
## 2. 各項目アンカーとスコア表
   ### B1: [item name]
       - アンカー(5段階)
       - スコア表(13 FW × [score, 重み, 備考])
   ### B2: ... (同様に B8 まで)
## 3. 集計(全 FW の単純平均/重み付き平均/★換算)
```

---

## 共通ルール

- **コミットフォーマット**: `docs(checks): <概要>` を Conventional Commit の subject にする
- **ファイル末尾改行**: すべてのMDファイルは末尾に改行を入れる
- **重みのデフォルト**: 全項目で `1.0`(N/A 項目は重み欄に「—」)
- **N/A 表記**: 該当しない項目は `N/A` とし、平均計算の分母から除外
- **★換算**: 4.5+ = ★★★★★ / 3.5+ = ★★★★ / 2.5+ = ★★★ / 1.5+ = ★★ / <1.5 = ★
- **検証コマンド**: 各タスクの最後に `git diff --stat HEAD~1` で1コミットの変更ファイル数とサイズを確認

---

### Task 1: README.md

**Files:**
- Create: `docs/checks/README.md`

- [ ] **Step 1: ディレクトリ作成**

```bash
mkdir -p docs/checks
ls docs/checks
```

Expected output: 空ディレクトリ(エラーが出ないこと)

- [ ] **Step 2: README.md を作成**

`docs/checks/README.md` に以下の内容を書き込む:

````markdown
# モバイルフレームワーク選定 評価チェックシート

本ディレクトリは、モバイルアプリ向けフレームワーク選定にあたり**プロジェクト固有事情に依存しない汎用評価軸**で各 FW を採点する再利用可能なチェックシート群である。

- 設計仕様: [`docs/superpowers/specs/2026-04-28-framework-checklist-design.md`](../superpowers/specs/2026-04-28-framework-checklist-design.md)
- 一次データ: [`docs/framework_comparison/framework_profiles.md`](../framework_comparison/framework_profiles.md)
- 情報基準日: 2026 年 4 月時点

---

## 評価対象(13 フレームワーク)

| # | FW | 主言語 | 備考 |
|---|---|---|---|
| 1 | Android ネイティブ(Jetpack Compose + Android SDK) | Kotlin | 業務アプリ第一推奨 |
| 2 | Flutter | Dart | |
| 3 | React Native | JavaScript / TypeScript | 素のRN |
| 4 | Expo(React Native + Expo SDK) | JavaScript / TypeScript | EAS Build含む |
| 5 | Ionic Framework | TypeScript | UI層 |
| 6 | Capacitor | TypeScript | ネイティブブリッジ層 |
| 7 | Kotlin Multiplatform (KMP) | Kotlin | ロジック共通化 |
| 8 | Compose Multiplatform | Kotlin | UI共通化 |
| 9 | .NET MAUI | C# | |
| 10 | NativeScript | TypeScript / JavaScript | |
| 11 | Apache Cordova | JavaScript | 縮小傾向 |
| 12 | Xamarin | C# | EOL(2024-05) |
| 13 | PWA | JavaScript / TypeScript | ブラウザ前提 |

---

## ファイル構成

| ファイル | 内容 |
|---|---|
| [`00_overview.md`](./00_overview.md) | サマリ + 端末ターゲット表 + 3種★点 |
| [`01_business_suitability.md`](./01_business_suitability.md) | 業務適正度 8 項目(B1〜B8) |
| [`02_learning_difficulty.md`](./02_learning_difficulty.md) | 習得難易度 8 項目(L1〜L8、5=易) |
| [`03_developer_experience.md`](./03_developer_experience.md) | 開発体験・開発環境 8 項目(D1〜D8) |
| [`04_language_features.md`](./04_language_features.md) | 言語仕様(事実情報、スコアなし) |

---

## スコアリング規約

### 1. スケール

- **1〜5 点の整数評価**(0.5 刻み不可)
- **5 = 望ましい状態**(習得難易度では 5 = 最も易しい)
- 各項目は冒頭に「点数 → 状態の言語化(代表例つき)」を 5 行で記述する **アンカー方式**

### 2. 仮想ペルソナ

評価のブレを抑える前提条件:

- **業務適正度・開発体験**: 中規模(10〜50画面)の Android 業務アプリを 2〜3 年スパンで保守する開発組織
- **習得難易度**: 他言語経験はあるが当該言語未経験の一般プログラマー

### 3. 集計

- **単純平均**: 各項目スコアの算術平均
- **重み付き平均**: Σ(スコア × 重み)÷ Σ(重み)
- **★換算**: 平均値を 5 段階おすすめ度に変換
  - 4.5 以上 = ★★★★★(5)
  - 3.5〜4.49 = ★★★★(4)
  - 2.5〜3.49 = ★★★(3)
  - 1.5〜2.49 = ★★(2)
  - 1.5 未満 = ★(1)

### 4. N/A の扱い

該当しない項目は **N/A** とし、平均計算の **分母から除外**(0 点扱いではない)。重み欄も `—`。

### 5. 重み列

各項目の重みはデフォルト `1.0`。利用者がプロジェクト優先度に応じて 0.0〜5.0 で調整する想定。本シート自体は重みを変更しない(原本として固定)。

---

## 利用ワークフロー

1. [`00_overview.md`](./00_overview.md) の端末対応 + 3種★ で候補を 3〜5 に絞り込む
2. [`04_language_features.md`](./04_language_features.md) で対応プラットフォーム(Web/サーバ/組込/ゲーム/CLI/データ)とスキル投資の汎用性を確認
3. [`01〜03`](./01_business_suitability.md) の詳細スコア表で根拠を確認(備考列に一次データ参照)
4. 自プロジェクト用にコピーして **重み列を編集**
5. 重み付き平均で順位を再計算 → 上位を最終候補とする

---

## プロジェクト固有事情との合成方法

本シートでは扱わない以下の **プロジェクト固有事情** は別軸チェックとして併用:

| 固有事情 | 合成方法 |
|---|---|
| チーム既存スキル(Java / JS / Kotlin 等) | L1〜L8 の重みを既存スキル親和度で調整 |
| 開発OS制約(Windows のみ等) | D8 の重みを上げる |
| 既存コード資産(Web / Android 等) | 流用可能な FW の B8 採用実績を優先 |
| 配布・運用要件(Store / MDM / 直配布) | B3 / B4 / B5 の重み調整 |
| 業界規制(金融 / 医療 / 公共) | B5(商用サポート)の重みを最大に |

---

## 更新ポリシー

- 各 FW の **採用時点で公式ソースで再確認**(LTS 日付、EOL 日付、ライセンス、サポート契約条件は変動)
- 一次データは [`framework_profiles.md`](../framework_comparison/framework_profiles.md) を更新し、本チェックシートは派生として再生成
- メジャーアップデート(LTS 切替、ライセンス変更、EOL 発表等)があった場合は当該項目を再評価

---

## 更新履歴

- **2026-04-28**: 初版作成
````

- [ ] **Step 3: ファイル内容を確認**

```bash
wc -l docs/checks/README.md
head -10 docs/checks/README.md
```

Expected: 100+ 行、1 行目が `# モバイルフレームワーク選定 評価チェックシート` で始まる

- [ ] **Step 4: コミット**

```bash
git add docs/checks/README.md
git commit -m "$(cat <<'EOF'
docs(checks): add README with scoring rules and target list

13 FWs, 1-5 anchor scoring across 4 dimensions, weight-adjustable per
project. Links to spec, primary data sources, and per-sheet workflow.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)"
```

Expected: コミット成功、`1 file changed`

---

### Task 2: 04_language_features.md(言語仕様カード)

**Files:**
- Create: `docs/checks/04_language_features.md`

評価対象 13 FW で使用される 6 言語のカードを作成: Kotlin / Dart / TypeScript / JavaScript / Java / C#

各カードは以下の項目で構成:
1. 対応プラットフォーム(モバイル / Web / サーバ / デスクトップ / 組込 / ゲーム / CLI / データサイエンス)
2. PWA対応(該当時のみ)
3. 代表的な採用例
4. 主要コンパイル方式
5. 主要パラダイム
6. 型システム特徴
7. メモリ管理
8. 標準ライブラリの範囲
9. 主要相互運用先
10. 言語の設計哲学
11. 関連参照

データソース: `framework_profiles.md` の「言語」節 + `framework_native_features.md` Part 1.1 マトリクス

- [ ] **Step 1: ファイル作成 + ヘッダ + Kotlin カード**

`docs/checks/04_language_features.md` を新規作成し、以下を書く:

````markdown
# 言語仕様(事実情報)

本シートは評価対象 13 FW で使用される言語の **事実情報のみ** を記載する(スコアなし)。
[`framework_profiles.md`](../framework_comparison/framework_profiles.md) と [`framework_native_features.md`](../framework_comparison/framework_native_features.md) Part 1.1 を一次データとする。

評価(スコア)は [`01_business_suitability.md`](./01_business_suitability.md) / [`02_learning_difficulty.md`](./02_learning_difficulty.md) / [`03_developer_experience.md`](./03_developer_experience.md) で **FW 単位** に行う。本シートは FW 行から主言語(B 列)経由で参照される補助資料。

凡例:
- 対応プラットフォーム: ◎=主要用途 / ○=実用可 / △=可能だが非主流 / ×=非対応

---

## Kotlin

| 項目 | 値 |
|---|---|
| 対応プラットフォーム | Android◎ / iOS○(KMP)/ Web△(Kotlin/JS)/ サーバ◎(Ktor / Spring)/ デスクトップ○(Compose Desktop)/ 組込△(Kotlin Native)/ ゲーム△(libGDX)/ CLI○(Clikt)/ データ△(KotlinDL) |
| PWA対応 | — |
| 代表的な採用例 | Google Android 公式推奨、Pinterest、Square、Trello、Netflix、JetBrains 製品群、Gradle |
| 主要コンパイル方式 | JVM バイトコード / Kotlin Native / Kotlin/JS / Kotlin/Wasm |
| 主要パラダイム | OOP + 関数型ハイブリッド |
| 型システム特徴 | 静的型 + null safety + 型推論 |
| メモリ管理 | JVM の場合 GC / Native は ARC 風 |
| 標準ライブラリの範囲 | コルーチン / コレクション / シリアライゼーション / 標準IO |
| 主要相互運用先 | Java(完全相互運用)、JNI、Swift / Obj-C(KMP 経由) |
| 言語の設計哲学 | Java 互換性と簡潔さの両立 |
| 関連参照 | [`framework_profiles.md#kotlin`](../framework_comparison/framework_profiles.md) |

---
````

- [ ] **Step 2: Dart カードを追加**

ファイル末尾に以下を追記:

````markdown
## Dart

| 項目 | 値 |
|---|---|
| 対応プラットフォーム | Android◎(Flutter)/ iOS◎(Flutter)/ Web○(Flutter Web)/ サーバ△(Dart Frog / dart:io)/ デスクトップ○(Flutter Desktop)/ 組込○(Flutter Embedded)/ ゲーム× / CLI○(pub bin)/ データ× |
| PWA対応 | △(Flutter Web で PWA 化可能、SEO 制限あり) |
| 代表的な採用例 | Flutter 全般、Google Pay、Alibaba、BMW、Toyota 車載 |
| 主要コンパイル方式 | AOT(モバイル/デスクトップ)/ JIT(開発時 Hot Reload)/ JS トランスパイル(Web) |
| 主要パラダイム | OOP + 関数型(Stream / Future) |
| 型システム特徴 | 静的型 + null safety(3.0+ 強制)+ 型推論 |
| メモリ管理 | GC |
| 標準ライブラリの範囲 | dart:io / dart:async / dart:html / Future / Stream |
| 主要相互運用先 | Platform Channel(Java/Kotlin/Swift/Obj-C)、FFI(C) |
| 言語の設計哲学 | UI 開発に最適化、シンプルな構文と高速起動 |
| 関連参照 | [`framework_profiles.md#dart`](../framework_comparison/framework_profiles.md) |

---
````

- [ ] **Step 3: TypeScript カードを追加**

ファイル末尾に以下を追記:

````markdown
## TypeScript

| 項目 | 値 |
|---|---|
| 対応プラットフォーム | Android◎(RN / Ionic)/ iOS◎(RN / Ionic)/ Web◎ / サーバ◎(Node.js / Deno / Bun / NestJS)/ デスクトップ◎(Electron / Tauri)/ 組込○(Espruino / Johnny-Five)/ ゲーム○(Phaser / PixiJS / Babylon)/ CLI◎ / データ○(TensorFlow.js) |
| PWA対応 | ◎(Web 標準 + Service Worker / Workbox など最も成熟) |
| 代表的な採用例 | VS Code、Slack、Discord、GitHub、Microsoft 365、Airbnb、Stripe、Asana |
| 主要コンパイル方式 | JS トランスパイル(tsc / Babel / esbuild / SWC) |
| 主要パラダイム | OOP + 関数型 + リアクティブ(フレームワーク次第) |
| 型システム特徴 | 静的型(漸進的)+ 型推論 + structural typing + ジェネリクス |
| メモリ管理 | GC(JS ランタイム依存) |
| 標準ライブラリの範囲 | JS 標準 + 型定義パッケージ群(@types/*) |
| 主要相互運用先 | JavaScript(完全相互運用)、WebAssembly、Native Module(RN / Capacitor) |
| 言語の設計哲学 | JavaScript への漸進的な型付け、互換性重視 |
| 関連参照 | [`framework_profiles.md#typescript`](../framework_comparison/framework_profiles.md) |

---
````

- [ ] **Step 4: JavaScript カードを追加**

ファイル末尾に以下を追記:

````markdown
## JavaScript(ECMAScript)

| 項目 | 値 |
|---|---|
| 対応プラットフォーム | Android○(RN / Ionic)/ iOS○(RN / Ionic)/ Web◎ / サーバ◎(Node.js)/ デスクトップ◎(Electron)/ 組込○ / ゲーム○ / CLI◎ / データ○ |
| PWA対応 | ◎(Web プラットフォーム標準) |
| 代表的な採用例 | あらゆる Web サイト、Cordova / Capacitor 旧コード資産、Node.js サーバ全般 |
| 主要コンパイル方式 | インタプリタ(JIT 最適化)、トランスパイラ経由(Babel) |
| 主要パラダイム | マルチパラダイム(関数型 + プロトタイプ OOP + 手続き型) |
| 型システム特徴 | 動的型(オプションで JSDoc 型注釈) |
| メモリ管理 | GC |
| 標準ライブラリの範囲 | ECMAScript Built-in / Web API / Node.js Core API |
| 主要相互運用先 | TypeScript(完全互換)、WebAssembly、Native Module |
| 言語の設計哲学 | 柔軟性最優先、後方互換維持 |
| 関連参照 | [`framework_profiles.md#javascriptecmascript`](../framework_comparison/framework_profiles.md) |

---
````

- [ ] **Step 5: Java カードを追加**

ファイル末尾に以下を追記:

````markdown
## Java

| 項目 | 値 |
|---|---|
| 対応プラットフォーム | Android◎(レガシ + Kotlin 併用)/ iOS× / Web△(GWT 旧)/ サーバ◎(Spring / Jakarta EE)/ デスクトップ○(JavaFX / Swing)/ 組込○ / ゲーム△(jMonkeyEngine)/ CLI○ / データ○(JVM ML) |
| PWA対応 | — |
| 代表的な採用例 | Android 旧アプリ、Spring 系企業システム、Apache Hadoop、Eclipse、IntelliJ プラグイン |
| 主要コンパイル方式 | JVM バイトコード(JIT) |
| 主要パラダイム | OOP + (Java 8+)関数型要素 |
| 型システム特徴 | 静的型 + ジェネリクス(消去型)/ null 非保証 |
| メモリ管理 | GC(複数コレクタ選択可: G1 / ZGC / Shenandoah) |
| 標準ライブラリの範囲 | java.* / javax.* — 最も豊富な標準ライブラリの一つ |
| 主要相互運用先 | Kotlin / Scala / Groovy(JVM 言語間)、JNI(C/C++) |
| 言語の設計哲学 | Write Once, Run Anywhere、後方互換最優先 |
| 関連参照 | [`framework_profiles.md#java`](../framework_comparison/framework_profiles.md) |

---
````

- [ ] **Step 6: C# カードを追加**

ファイル末尾に以下を追記:

````markdown
## C#(.NET)

| 項目 | 値 |
|---|---|
| 対応プラットフォーム | Android○(MAUI / Xamarin)/ iOS○(MAUI)/ Web○(Blazor)/ サーバ◎(ASP.NET Core)/ デスクトップ◎(WPF / WinUI / MAUI / Avalonia)/ 組込○ / ゲーム◎(Unity)/ CLI○ / データ○(ML.NET) |
| PWA対応 | ○(Blazor WebAssembly で PWA 化可能) |
| 代表的な採用例 | Microsoft 全製品群、Stack Overflow、Unity ゲーム、社内基幹系 .NET アプリ |
| 主要コンパイル方式 | IL バイトコード + JIT / AOT(NativeAOT) |
| 主要パラダイム | OOP + 関数型(LINQ / pattern matching) |
| 型システム特徴 | 静的型 + 型推論 + null safety(nullable reference types)+ ジェネリクス(具現化型) |
| メモリ管理 | GC |
| 標準ライブラリの範囲 | BCL(Base Class Library)+ .NET Standard / .NET Runtime |
| 主要相互運用先 | F# / VB.NET(.NET 言語間)、P/Invoke(C/C++)、COM Interop |
| 言語の設計哲学 | 生産性重視、Microsoft エコシステム統合 |
| 関連参照 | [`framework_profiles.md#c`](../framework_comparison/framework_profiles.md) |

---

## 更新履歴

- **2026-04-28**: 初版作成。Kotlin / Dart / TypeScript / JavaScript / Java / C# の 6 言語カードを記載。
````

- [ ] **Step 7: ファイル内容を確認**

```bash
wc -l docs/checks/04_language_features.md
grep -c "^##" docs/checks/04_language_features.md
```

Expected: 130+ 行、`##` 行が 7 件以上(言語 6 + 更新履歴 1)

- [ ] **Step 8: コミット**

```bash
git add docs/checks/04_language_features.md
git commit -m "$(cat <<'EOF'
docs(checks): add 04_language_features with 6 language cards

Facts-only cards for Kotlin/Dart/TypeScript/JavaScript/Java/C# with
platform support, adoption examples, compile model, paradigm, type
system, memory management, interop, and design philosophy.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)"
```

---

### Task 3: 01_business_suitability.md(B1〜B4 + ヘッダ)

**Files:**
- Create: `docs/checks/01_business_suitability.md`

業務適正度 8 項目のうち B1〜B4 を記載する。各項目はアンカー(5段階)+ スコア表(13 FW × [score, 重み, 備考])で構成。

スコア決定の根拠は `framework_profiles.md` および `selection_criteria.md` F3〜F8 を参照。

- [ ] **Step 1: ヘッダと共通ルール参照を作成**

`docs/checks/01_business_suitability.md` を新規作成し、以下を書く:

````markdown
# 業務適正度(B1〜B8)

業務アプリ用途で各 FW がどれだけ適しているかを 8 項目で評価する。
評価ルール詳細は [`README.md`](./README.md) 参照。

- スコア: 1〜5(5 = 望ましい)
- 重み: デフォルト 1.0(利用者が調整可)
- 仮想ペルソナ: 中規模 Android 業務アプリを 2〜3 年スパンで保守する開発組織

| 項目 | 内容 |
|---|---|
| B1 | リリース・サポートの予測可能性 |
| B2 | 開発企業・コミュニティの分散度 |
| B3 | ネイティブハードウェアアクセス |
| B4 | セキュリティ・脆弱性対応体制 |
| B5 | 商用サポート可否 |
| B6 | ライセンス安定性 |
| B7 | 後方互換性(破壊的変更頻度の低さ) |
| B8 | エンタープライズ採用実績 |

---

## B1: リリース・サポートの予測可能性

### アンカー

- **5**: 明示 LTS あり + 規則的リリース + 長期サポート(Java OpenJDK、Angular、.NET)
- **4**: 明示 LTS あり or 規則的リリース のどちらか(.NET MAUI、Kotlin)
- **3**: 後方互換維持で事実上安定だが明示 LTS なし(TypeScript、React)
- **2**: LTS なし・リリース不規則で追従負荷あり(Flutter、React Native、Expo)
- **1**: EOL or 縮小傾向(Xamarin、Cordova、PhoneGap)

### スコア表

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native | 4 | 1.0 | Android SDK は毎年更新、Jetpack 月次、Google Play Target API 引き上げ規則的だが明示 LTS なし |
| Flutter | 2 | 1.0 | 四半期 stable・月次 beta だが明示 LTS なし、追従負荷あり |
| React Native | 2 | 1.0 | 2〜3ヶ月で新マイナー、直近数バージョンのみサポート |
| Expo | 2 | 1.0 | 四半期 SDK、直近 3 SDK 程度サポート |
| Ionic Framework | 3 | 1.0 | 年 1 メジャー前後、後方互換重視だが明示 LTS なし |
| Capacitor | 3 | 1.0 | 半年〜年 1、Android/iOS SDK に追従、Ionic Enterprise で SLA 強化可 |
| KMP | 3 | 1.0 | Kotlin 同期、2023 Stable 到達、明示 LTS なしだが安定 |
| Compose Multiplatform | 2 | 1.0 | JetBrains 主導だが API 進化中、明示 LTS なし |
| .NET MAUI | 4 | 1.0 | .NET 同期(年 1)、LTS=3 年(STS=18ヶ月)、月次パッチ |
| NativeScript | 2 | 1.0 | リリース頻度低下、明示 LTS なし、モメンタム低 |
| Apache Cordova | 1 | 1.0 | プロジェクト縮小、新規非推奨 |
| Xamarin | 1 | 1.0 | 2024-05-01 EOL、.NET MAUI が後継 |
| PWA | 4 | 1.0 | Web プラットフォーム標準、ブラウザベンダーが LTS 相当 |

---

## B2: 開発企業・コミュニティの分散度

### アンカー

- **5**: 多ベンダー体制 or 巨大独立コミュニティ(Java OpenJDK、React Native)
- **4**: 主導企業 + 強力な複数企業/財団参加(.NET、TypeScript、React)
- **3**: 単一企業主導だが財団化・企業規模大(Kotlin、Angular)
- **2**: 単一企業の戦略依存(Flutter、Dart、Ionic)
- **1**: 個人 or 小チーム依存(Vue、NativeScript)

### スコア表

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native | 5 | 1.0 | Google + AOSP 巨大コミュニティ、複数企業実装(Samsung 等) |
| Flutter | 2 | 1.0 | Google 単独主導、戦略依存 |
| React Native | 5 | 1.0 | Meta + Microsoft + Callstack + Expo + コミュニティ |
| Expo | 3 | 1.0 | Expo 社主導だが RN コア + 複数企業の上に成立 |
| Ionic Framework | 2 | 1.0 | Ionic(営利企業)単独 |
| Capacitor | 2 | 1.0 | Ionic(営利企業)単独 |
| KMP | 3 | 1.0 | JetBrains 主導 + Kotlin Foundation + Google 後援 |
| Compose Multiplatform | 2 | 1.0 | JetBrains 単独主導 |
| .NET MAUI | 4 | 1.0 | Microsoft 主導 + .NET Foundation |
| NativeScript | 1 | 1.0 | OpenJS Foundation 配下だがモメンタム低 |
| Apache Cordova | 2 | 1.0 | Apache Foundation 配下だが活動縮小 |
| Xamarin | 1 | 1.0 | EOL、メンテナンスのみ |
| PWA | 5 | 1.0 | W3C / WHATWG / 複数ブラウザベンダーで標準化 |

---

## B3: ネイティブハードウェアアクセス

### アンカー

- **5**: 業務HT周辺機器ベンダーSDKが公式言語前提(Kotlin / Java + Android)
- **4**: 主要機能はFW公式 or 高品質ラッパで網羅(Flutter)
- **3**: コミュニティラッパ経由で実装可だが品質ばらつき(RN / Expo)
- **2**: 拡張プラグイン経由で限定的(Ionic / Capacitor)
- **1**: 原理的不可 or 重大制約(PWA)

### スコア表

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native | 5 | 1.0 | Zebra / Honeywell / Datalogic SDK が Java/Kotlin 前提、Intent / EMDK 全機能 |
| Flutter | 4 | 1.0 | 公式 plugin + Zebra など主要HT で公式サンプル、Platform Channel で逃げ道あり |
| React Native | 3 | 1.0 | 3rd party ラッパ経由、新アーキで品質改善傾向 |
| Expo | 3 | 1.0 | Expo SDK + Custom Native Module(Bare 化必要な場合あり) |
| Ionic Framework | 2 | 1.0 | Capacitor プラグイン経由、HW SDK は限定的 |
| Capacitor | 2 | 1.0 | プラグイン経由、業務HT は自前ラッパ必要 |
| KMP | 5 | 1.0 | Android 側は Kotlin 直接、HW SDK 完全アクセス |
| Compose Multiplatform | 4 | 1.0 | Compose UI + Android 側は Kotlin 経由で完全アクセス |
| .NET MAUI | 3 | 1.0 | Platform Specifics / Handler 経由、HT ベンダーは MAUI 対応途上 |
| NativeScript | 3 | 1.0 | JS 経由で Java API 直接呼出だがエコシステム狭い |
| Apache Cordova | 2 | 1.0 | プラグイン経由、HT 系は自前実装多 |
| Xamarin | 3 | 1.0 | C# Bindings 経由(EOL のため新規不可) |
| PWA | 1 | 1.0 | Web API 限定(BLE / NFC / Camera は実験的、HT SDK は原理的不可) |

---

## B4: セキュリティ・脆弱性対応体制

### アンカー

- **5**: 月次CVE窓口 + OS暗号API/Keystore直結 + 公式セキュリティチーム(Android / Jetpack、OpenJDK)
- **4**: 公式セキュリティ窓口あり、対応速度高(Kotlin、Flutter、Angular)
- **3**: GitHub Advisory ベース、対応速度中(React Native、TypeScript)
- **2**: コミュニティ依存で対応速度ばらつき(Vue、サードパーティ多用FW)
- **1**: 玉石混交の3rd party依存が中核(Capacitor 旧プラグイン、Cordova)

### スコア表

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native | 5 | 1.0 | Android Security Bulletin 月次 + AndroidX 公式、Keystore / Biometric 直結 |
| Flutter | 4 | 1.0 | security@flutter.dev + Google セキュリティチーム |
| React Native | 3 | 1.0 | GitHub Security Advisories + Meta 対応 |
| Expo | 3 | 1.0 | Expo + RN コア両方の対応に依存 |
| Ionic Framework | 3 | 1.0 | 公式 + 商用契約で SLA 強化可 |
| Capacitor | 2 | 1.0 | 公式コアは安定、プラグインは玉石混交 |
| KMP | 4 | 1.0 | JetBrains Security + Kotlin 公式 |
| Compose Multiplatform | 3 | 1.0 | JetBrains Security 同等だが広域に新しい |
| .NET MAUI | 4 | 1.0 | Microsoft Security Response Center、月次 Patch Tuesday |
| NativeScript | 2 | 1.0 | コミュニティ依存、対応速度ばらつき |
| Apache Cordova | 1 | 1.0 | プラグイン依存、メンテ放棄多 |
| Xamarin | 2 | 1.0 | Microsoft 対応継続だが EOL 後はベストエフォート |
| PWA | 4 | 1.0 | ブラウザベンダー(Chrome / Safari / Firefox)が定期パッチ |

---
````

- [ ] **Step 2: ファイル内容を確認**

```bash
wc -l docs/checks/01_business_suitability.md
grep -c "^### スコア表" docs/checks/01_business_suitability.md
```

Expected: 100+ 行、`### スコア表` が 4 件

- [ ] **Step 3: コミット**

```bash
git add docs/checks/01_business_suitability.md
git commit -m "$(cat <<'EOF'
docs(checks): start 01_business_suitability with B1-B4

Header, virtual persona, and 4 of 8 items: release predictability,
governance distribution, native HW access, security response. Each
item has anchor + 13-FW score table with rationale.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)"
```

---

### Task 4: 01_business_suitability.md(B5〜B8 + 集計)

**Files:**
- Modify: `docs/checks/01_business_suitability.md`

B5〜B8 と集計表(全 13 FW の単純平均/重み付き平均/★換算)を追記する。

- [ ] **Step 1: B5 を追記**

`docs/checks/01_business_suitability.md` の末尾(`---` の直前)に以下を追記:

````markdown
## B5: 商用サポート可否

### アンカー

- **5**: 複数ベンダーが SLA 付き商用サポート提供(Java: Oracle / Red Hat / Azul / AWS)
- **4**: 単独ベンダーが商用サポート提供(.NET、Kotlin / IntelliJ、Ionic Enterprise)
- **3**: 公式サポートはないがコンサル企業経由で実質可(React Native: Callstack 等)
- **2**: 限定的・ベンダー間接(Flutter Partner 経由)
- **1**: 商用サポートなし(コミュニティのみ)

### スコア表

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native | 4 | 1.0 | JetBrains 商用契約(Kotlin)+ Google Cloud 関連、HT ベンダー保守契約 |
| Flutter | 2 | 1.0 | Google 直接商用サポートなし、Flutter Partner 経由 |
| React Native | 3 | 1.0 | Meta 直接契約なし、Callstack / Expo / Microsoft 経由 |
| Expo | 4 | 1.0 | EAS 商用プラン(Production / Enterprise)で SLA 付与 |
| Ionic Framework | 4 | 1.0 | Ionic Enterprise 公式契約 |
| Capacitor | 4 | 1.0 | Ionic Enterprise 公式契約 |
| KMP | 4 | 1.0 | JetBrains 商用契約 |
| Compose Multiplatform | 4 | 1.0 | JetBrains 商用契約 |
| .NET MAUI | 5 | 1.0 | Microsoft 商用サポート、Azure DevOps 統合 |
| NativeScript | 1 | 1.0 | 商用サポートほぼなし |
| Apache Cordova | 1 | 1.0 | 商用サポートなし |
| Xamarin | 2 | 1.0 | Microsoft 拡張サポート(EOL 後は限定) |
| PWA | 1 | 1.0 | Web 標準として直接の商用サポートはなし(Web ホスティング契約は別) |

---

## B6: ライセンス安定性

### アンカー

- **5**: Apache / MIT / BSD で枯れている(Kotlin、Flutter、TypeScript、.NET)
- **4**: GPL 系だが Classpath Exception 等で実質クリア(OpenJDK)
- **3**: ベンダーライセンス併存だが実用上問題なし(Oracle JDK with NFTC)
- **2**: 商用条件が複雑、規模で課金(Unreal Engine の売上ロイヤリティ)
- **1**: 過去に一方的な商用条件変更の前例あり(Unity ランタイム料金騒動)

### スコア表

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native | 5 | 1.0 | Apache 2.0(AOSP)、Jetpack Compose も Apache 2.0 |
| Flutter | 5 | 1.0 | BSD-3-Clause |
| React Native | 5 | 1.0 | MIT(旧 Meta 特許条項撤廃済) |
| Expo | 5 | 1.0 | MIT(EAS Build / Update は有料プラン) |
| Ionic Framework | 5 | 1.0 | MIT(Enterprise 契約は有償) |
| Capacitor | 5 | 1.0 | MIT |
| KMP | 5 | 1.0 | Apache 2.0 |
| Compose Multiplatform | 5 | 1.0 | Apache 2.0 |
| .NET MAUI | 5 | 1.0 | MIT |
| NativeScript | 5 | 1.0 | Apache 2.0 |
| Apache Cordova | 5 | 1.0 | Apache 2.0 |
| Xamarin | 5 | 1.0 | MIT |
| PWA | 5 | 1.0 | Web 標準仕様自体は無料、ブラウザベンダーごとライセンス |

---

## B7: 後方互換性(破壊的変更頻度の低さ)

### アンカー

- **5**: 10 年単位の互換維持文化(Java「Write Once, Run Forever」)
- **4**: メジャーアップデートでも軽微な修正で済む(React、TypeScript)
- **3**: 規則的だがマイグレーション必要(Angular、.NET)
- **2**: メジャー毎に大規模 Migration Guide が出る(Flutter、Kotlin 2.0)
- **1**: メジャー間で事実上再書き(Vue 2→3、React Native 新アーキ移行)

### スコア表

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native | 3 | 1.0 | Target API 強制引き上げ、Compose API 進化、ng update 相当の自動補助あり |
| Flutter | 2 | 1.0 | API が頻繁に改定、大規模 Migration Guide |
| React Native | 1 | 1.0 | 新アーキ(JSI / Fabric)移行、ライブラリ互換性問題深刻 |
| Expo | 2 | 1.0 | SDK バージョンごとにライブラリ入替 |
| Ionic Framework | 4 | 1.0 | 後方互換重視、メジャー間でも軽微 |
| Capacitor | 3 | 1.0 | Android / iOS SDK 追従でメジャー時に変更あり |
| KMP | 3 | 1.0 | Stable 後は安定、ただし K2 切替の影響あり |
| Compose Multiplatform | 2 | 1.0 | API 進化中、Stable 化前後で変更頻度高 |
| .NET MAUI | 3 | 1.0 | .NET メジャー毎に整理、LTS 切替で対応必要 |
| NativeScript | 3 | 1.0 | 変更頻度低いがエコシステム追従不足 |
| Apache Cordova | 4 | 1.0 | プロジェクト縮小により実質変更少(=後方互換維持) |
| Xamarin | 1 | 1.0 | EOL、MAUI への移行は実質再書き |
| PWA | 4 | 1.0 | Web 標準は後方互換重視、Service Worker 等の API 仕様も安定 |

---

## B8: エンタープライズ採用実績

### アンカー

- **5**: 業務基幹で広範採用、Fortune 500 多数(Java、Android Native)
- **4**: エンタープライズ採用が広く認知(.NET、Kotlin、TypeScript)
- **3**: 業務利用は伸びているが業界別偏り(Flutter、React Native)
- **2**: 採用例はあるがニッチ(Ionic、KMP)
- **1**: 業務利用ほぼ未検証 or 縮小(NativeScript、Cordova 新規)

### スコア表

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native | 5 | 1.0 | 物流 / 小売 / 金融など業務基幹で世界規模採用 |
| Flutter | 3 | 1.0 | Google Pay / Alibaba / BMW など、業務系は伸びているが偏りあり |
| React Native | 3 | 1.0 | Facebook / Instagram / Discord、業務系も増えているが業界偏り |
| Expo | 3 | 1.0 | 中規模アプリで採用増、エンタープライズ用途も拡大中 |
| Ionic Framework | 3 | 1.0 | 業務アプリ採用多数(社内ツール特化) |
| Capacitor | 3 | 1.0 | Ionic と同様に社内業務アプリで広く採用 |
| KMP | 2 | 1.0 | 採用例増加中だがまだニッチ |
| Compose Multiplatform | 1 | 1.0 | Stable 直後で業務採用例少 |
| .NET MAUI | 4 | 1.0 | Microsoft エコシステム企業で広く採用、Xamarin からの移行先 |
| NativeScript | 1 | 1.0 | エンタープライズ採用ほぼ未検証 |
| Apache Cordova | 2 | 1.0 | 過去に多数採用されたがレガシ移行中 |
| Xamarin | 2 | 1.0 | EOL のため新規採用なし、既存資産のみ |
| PWA | 3 | 1.0 | Twitter / Starbucks / Pinterest など、業務系は社内ツール中心 |

---
````

- [ ] **Step 2: 集計表を追記**

ファイル末尾(全項目の後)に以下を追記:

````markdown
## 集計

各 FW の B1〜B8 を平均し、★換算したもの。重み付き平均は重み = 1.0(均等)前提のため単純平均と同値。利用者は重みを変更して再計算する。

| FW | B1 | B2 | B3 | B4 | B5 | B6 | B7 | B8 | 単純平均 | 重み付き平均 | ★換算 |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Android Native | 4 | 5 | 5 | 5 | 4 | 5 | 3 | 5 | 4.50 | 4.50 | ★★★★★ |
| Flutter | 2 | 2 | 4 | 4 | 2 | 5 | 2 | 3 | 3.00 | 3.00 | ★★★ |
| React Native | 2 | 5 | 3 | 3 | 3 | 5 | 1 | 3 | 3.13 | 3.13 | ★★★ |
| Expo | 2 | 3 | 3 | 3 | 4 | 5 | 2 | 3 | 3.13 | 3.13 | ★★★ |
| Ionic Framework | 3 | 2 | 2 | 3 | 4 | 5 | 4 | 3 | 3.25 | 3.25 | ★★★ |
| Capacitor | 3 | 2 | 2 | 2 | 4 | 5 | 3 | 3 | 3.00 | 3.00 | ★★★ |
| KMP | 3 | 3 | 5 | 4 | 4 | 5 | 3 | 2 | 3.63 | 3.63 | ★★★★ |
| Compose Multiplatform | 2 | 2 | 4 | 3 | 4 | 5 | 2 | 1 | 2.88 | 2.88 | ★★★ |
| .NET MAUI | 4 | 4 | 3 | 4 | 5 | 5 | 3 | 4 | 4.00 | 4.00 | ★★★★ |
| NativeScript | 2 | 1 | 3 | 2 | 1 | 5 | 3 | 1 | 2.25 | 2.25 | ★★ |
| Apache Cordova | 1 | 2 | 2 | 1 | 1 | 5 | 4 | 2 | 2.25 | 2.25 | ★★ |
| Xamarin | 1 | 1 | 3 | 2 | 2 | 5 | 1 | 2 | 2.13 | 2.13 | ★★ |
| PWA | 4 | 5 | 1 | 4 | 1 | 5 | 4 | 3 | 3.38 | 3.38 | ★★★ |

> 注: 各セルのスコアは上記アンカーに基づく初期値(2026-04-28 時点)。利用者は自プロジェクトの優先度に応じて重み列を編集し、上記表の単純平均/重み付き平均を再計算する。

---

## 更新履歴

- **2026-04-28**: 初版作成。B1〜B8 のアンカー定義と 13 FW の初期スコアを記載。
````

- [ ] **Step 3: 確認**

```bash
wc -l docs/checks/01_business_suitability.md
grep -c "^### スコア表" docs/checks/01_business_suitability.md
```

Expected: 250+ 行、`### スコア表` が 8 件

- [ ] **Step 4: コミット**

```bash
git add docs/checks/01_business_suitability.md
git commit -m "$(cat <<'EOF'
docs(checks): complete 01_business_suitability with B5-B8 and totals

Adds commercial support, license stability, backward compatibility,
enterprise adoption (B5-B8) and a 13-FW summary table with simple
average, weighted average, and 5-star conversion.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)"
```

---

### Task 5: 02_learning_difficulty.md(L1〜L4 + ヘッダ)

**Files:**
- Create: `docs/checks/02_learning_difficulty.md`

習得難易度 8 項目のうち L1〜L4 を記載。**5 = 最も易しい** であることに注意。

仮想ペルソナ: 他言語経験はあるが当該言語未経験のプログラマー。FW の主言語に対する評価。

- [ ] **Step 1: ヘッダと L1〜L4 を作成**

`docs/checks/02_learning_difficulty.md` を新規作成し、以下を書く:

````markdown
# 習得難易度(L1〜L8)

各 FW の主言語 + FW 固有の概念を新規習得する難易度を 8 項目で評価する。
**5 = 最も易しい**(他項目と同じく「望ましい状態が高得点」に揃える)。

評価ルール詳細は [`README.md`](./README.md) 参照。

- スコア: 1〜5(5 = 最も易しい)
- 重み: デフォルト 1.0(利用者が調整可)
- 仮想ペルソナ: 他言語経験はあるが当該言語未経験の一般プログラマー

| 項目 | 内容 |
|---|---|
| L1 | 構文・記法のシンプルさ |
| L2 | 型システムの取っつきやすさ |
| L3 | 並行・非同期モデルの理解負荷 |
| L4 | パラダイム混在度の少なさ |
| L5 | エコシステム・ツーリング学習量 |
| L6 | 公式ドキュメント・教材充実度 |
| L7 | AI / LLM 補完の精度 |
| L8 | トラブルシューティング容易度 |

---

## L1: 構文・記法のシンプルさ

### アンカー

- **5**: 数日で読み書きできる、暗黙ルールが少ない(Java、C#、Kotlin)
- **4**: 標準的な構文だが慣用句あり(Dart、TypeScript)
- **3**: 言語機能多くスタイル分岐あり(Swift)
- **2**: 暗黙ルール多数(JavaScript: this 束縛・プロトタイプ)
- **1**: メタ構文・特殊記法多用

### スコア表

| FW(主言語) | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native(Kotlin) | 5 | 1.0 | Java 経験者なら数日、初学者でも 1〜2 週で読める |
| Flutter(Dart) | 4 | 1.0 | Java 構文親和、Widget Tree の宣言的記法に慣れ要 |
| React Native(JS / TS) | 2 | 1.0 | JSX + Hooks + JS 暗黙ルール混在 |
| Expo(JS / TS) | 2 | 1.0 | RN と同等 |
| Ionic Framework(TS) | 4 | 1.0 | TS 標準構文 + Angular / React / Vue 流派あり |
| Capacitor(TS) | 4 | 1.0 | プラグイン API は明快、TS 標準 |
| KMP(Kotlin) | 5 | 1.0 | Kotlin 単独習得は容易、KMP 固有概念は別途 |
| Compose Multiplatform(Kotlin) | 4 | 1.0 | Kotlin + Compose 宣言的記法 |
| .NET MAUI(C#) | 5 | 1.0 | C# は Java 親和、XAML は宣言的で読みやすい |
| NativeScript(TS / JS) | 4 | 1.0 | XML テンプレート + TS、構文は標準的 |
| Apache Cordova(JS) | 3 | 1.0 | JS 標準 + プラグイン API、Web 経験あれば容易 |
| Xamarin(C#) | 4 | 1.0 | C# 標準 + Xamarin.Forms XAML、概念古め |
| PWA(JS / TS) | 4 | 1.0 | Web 標準のみ、Service Worker のイベント駆動が壁 |

---

## L2: 型システムの取っつきやすさ

### アンカー

- **5**: 強い型推論 + null safety が自明(Kotlin、Dart 3.0+)
- **4**: 静的型 + 型推論あり(TypeScript、C#、Swift)
- **3**: 静的型だが冗長(Java の旧バージョン)
- **2**: 動的型に後付け型(JavaScript + JSDoc)
- **1**: 動的型のみ(素のJavaScript)

### スコア表

| FW(主言語) | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native(Kotlin) | 5 | 1.0 | null safety + 型推論、Java 互換 |
| Flutter(Dart) | 5 | 1.0 | Dart 3.0+ で null safety 強制、型推論あり |
| React Native(JS / TS) | 4 | 1.0 | TS 採用前提なら 4、素 JS なら 1 |
| Expo(JS / TS) | 4 | 1.0 | TS テンプレートが推奨 |
| Ionic Framework(TS) | 4 | 1.0 | Angular との組合せで TS 必須 |
| Capacitor(TS) | 4 | 1.0 | TS 標準 |
| KMP(Kotlin) | 5 | 1.0 | Kotlin 同等 |
| Compose Multiplatform(Kotlin) | 5 | 1.0 | Kotlin 同等 |
| .NET MAUI(C#) | 4 | 1.0 | C# nullable reference types、ジェネリクス具現化 |
| NativeScript(TS / JS) | 4 | 1.0 | TS 推奨 |
| Apache Cordova(JS) | 1 | 1.0 | 素の JS 想定 |
| Xamarin(C#) | 3 | 1.0 | C# 旧バージョン互換、null safety は後発 |
| PWA(JS / TS) | 4 | 1.0 | TS 採用前提なら 4 |

---

## L3: 並行・非同期モデルの理解負荷

### アンカー

- **5**: 同期的記述に近い(Kotlin coroutine + suspend、Dart async / await)
- **4**: 単一モデルで習得可(JavaScript Promise / async)
- **3**: 複数モデル併存だが選択明確(Java の Future / CompletableFuture / Reactive)
- **2**: モデル選択でコード分岐(RxJava + coroutine 混在)
- **1**: スレッド直接管理が必要(Java 古い書き方)

### スコア表

| FW(主言語) | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native(Kotlin) | 5 | 1.0 | coroutine + Flow が公式推奨、suspend で同期的記述 |
| Flutter(Dart) | 5 | 1.0 | async / await + Future / Stream、宣言的でシンプル |
| React Native(JS / TS) | 4 | 1.0 | Promise / async / await + Hooks の useEffect 等 |
| Expo(JS / TS) | 4 | 1.0 | RN と同等 |
| Ionic Framework(TS) | 3 | 1.0 | Promise + RxJS Observable 併存(Angular 採用時) |
| Capacitor(TS) | 4 | 1.0 | Promise ベースの Capacitor API |
| KMP(Kotlin) | 5 | 1.0 | coroutine 共通利用、Platform 間で同じモデル |
| Compose Multiplatform(Kotlin) | 5 | 1.0 | KMP 同等 |
| .NET MAUI(C#) | 4 | 1.0 | async / await + Task、TPL Dataflow など選択肢多い |
| NativeScript(TS / JS) | 4 | 1.0 | Promise ベース、Observable 任意 |
| Apache Cordova(JS) | 4 | 1.0 | Promise + コールバック、シンプル |
| Xamarin(C#) | 4 | 1.0 | C# async / await |
| PWA(JS / TS) | 4 | 1.0 | Promise + Service Worker のイベント駆動 |

---

## L4: パラダイム混在度の少なさ

### アンカー

- **5**: 単一パラダイムで完結(Java OOP)
- **4**: OOP + 関数型ハイブリッドだが OOP 主導(Kotlin、C#)
- **3**: 複数パラダイム選択可だがデフォルト明確(Swift、Dart)
- **2**: 関数型・リアクティブ・OOP 混在(TypeScript + React + Hooks)
- **1**: フレームワーク毎にパラダイムが変わる(Angular vs React vs Vue で要再学習)

### スコア表

| FW(主言語) | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native(Kotlin) | 4 | 1.0 | OOP + 関数型ハイブリッド、Compose で宣言的 UI |
| Flutter(Dart) | 3 | 1.0 | OOP + 宣言的 UI(Widget)+ 関数型要素 |
| React Native(JS / TS) | 2 | 1.0 | 関数型 + Hooks + JSX、複数パラダイム混在 |
| Expo(JS / TS) | 2 | 1.0 | RN と同等 |
| Ionic Framework(TS) | 1 | 1.0 | Angular / React / Vue で UI パラダイムが変わる |
| Capacitor(TS) | 4 | 1.0 | プラグイン API はシンプル、UI 層は別 |
| KMP(Kotlin) | 4 | 1.0 | Kotlin OOP + 関数型 |
| Compose Multiplatform(Kotlin) | 3 | 1.0 | Compose の宣言的 UI が追加される |
| .NET MAUI(C#) | 4 | 1.0 | C# OOP + LINQ 関数型 + XAML 宣言的 UI |
| NativeScript(TS / JS) | 3 | 1.0 | XML テンプレート + 任意 UI フレームワーク |
| Apache Cordova(JS) | 4 | 1.0 | Web 標準のみ、UI フレームワーク選択は別 |
| Xamarin(C#) | 4 | 1.0 | C# OOP + XAML |
| PWA(JS / TS) | 4 | 1.0 | Web 標準 + 任意フレームワーク |

---
````

- [ ] **Step 2: ファイル内容を確認**

```bash
wc -l docs/checks/02_learning_difficulty.md
grep -c "^### スコア表" docs/checks/02_learning_difficulty.md
```

Expected: 100+ 行、`### スコア表` が 4 件

- [ ] **Step 3: コミット**

```bash
git add docs/checks/02_learning_difficulty.md
git commit -m "$(cat <<'EOF'
docs(checks): start 02_learning_difficulty with L1-L4

Header (5=easiest), virtual persona, and 4 of 8 items: syntax simplicity,
type system approachability, async model load, paradigm consistency.
Each item has anchor + 13-FW score table.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)"
```

---

### Task 6: 02_learning_difficulty.md(L5〜L8 + 集計)

**Files:**
- Modify: `docs/checks/02_learning_difficulty.md`

L5〜L8 と集計表を追記する。

- [ ] **Step 1: L5〜L8 を追記**

`docs/checks/02_learning_difficulty.md` の末尾に以下を追記:

````markdown
## L5: エコシステム・ツーリング学習量

### アンカー

- **5**: 単一CLIで完結(Flutter `flutter` コマンド)
- **4**: 標準ビルドツールで完結(Kotlin: Gradle、C#: dotnet CLI)
- **3**: ビルドツール + 依存管理が分離(Java: Gradle / Maven)
- **2**: バンドラ + パッケージマネージャ + ランタイム多重(RN: Metro + npm + Gradle + Xcode)
- **1**: 複数バンドラ・複数ランタイム選択(Web 全般)

### スコア表

| FW(主言語) | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native(Kotlin) | 4 | 1.0 | Gradle 単独、Android Studio 統合 |
| Flutter(Dart) | 5 | 1.0 | flutter CLI で全部完結 |
| React Native(JS / TS) | 2 | 1.0 | Metro + npm + Gradle + Xcode |
| Expo(JS / TS) | 4 | 1.0 | EAS で抽象化、初学者に易 |
| Ionic Framework(TS) | 3 | 1.0 | npm + Ionic CLI + Capacitor + Web ビルドツール |
| Capacitor(TS) | 4 | 1.0 | Capacitor CLI 中心、シンプル |
| KMP(Kotlin) | 2 | 1.0 | Gradle Kotlin DSL + Xcode + Android Studio + KMP plugin |
| Compose Multiplatform(Kotlin) | 2 | 1.0 | KMP と同等 + iOS UI 統合 |
| .NET MAUI(C#) | 4 | 1.0 | dotnet CLI / Visual Studio 統合 |
| NativeScript(TS / JS) | 3 | 1.0 | NS CLI + npm |
| Apache Cordova(JS) | 3 | 1.0 | Cordova CLI + Web ビルドツール |
| Xamarin(C#) | 3 | 1.0 | Visual Studio 中心、Xamarin.Forms 概念追加 |
| PWA(JS / TS) | 1 | 1.0 | バンドラ・パッケージマネージャ・ランタイム選択肢多 |

---

## L6: 公式ドキュメント・教材充実度

### アンカー

- **5**: 日本語含めて潤沢(Java、Kotlin、React、Vue、Angular、TypeScript)
- **4**: 英語豊富 + 日本語コミュニティ活発(Flutter、Dart)
- **3**: 英語豊富だが日本語限定的(C#、Swift)
- **2**: 英語の散発記事中心(KMP、Compose Multiplatform)
- **1**: 英語でも限定的(NativeScript、レガシFW)

### スコア表

| FW(主言語) | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native(Kotlin) | 5 | 1.0 | 公式 codelab + Kotlin もくもく会、書籍多数、日本語潤沢 |
| Flutter(Dart) | 4 | 1.0 | flutter.jp / FlutterKaigi、近年急増 |
| React Native(JS / TS) | 3 | 1.0 | 英語記事多、日本語は限定的 |
| Expo(JS / TS) | 3 | 1.0 | RN 同等、Expo 公式 docs は良質 |
| Ionic Framework(TS) | 3 | 1.0 | レガシー情報多、日本語は中程度 |
| Capacitor(TS) | 3 | 1.0 | Ionic と同レベル |
| KMP(Kotlin) | 2 | 1.0 | 海外記事頼り、日本語限定 |
| Compose Multiplatform(Kotlin) | 2 | 1.0 | 新しく日本語資料少 |
| .NET MAUI(C#) | 3 | 1.0 | Microsoft Learn + Microsoft 社内向け中心 |
| NativeScript(TS / JS) | 1 | 1.0 | ほぼ皆無 |
| Apache Cordova(JS) | 2 | 1.0 | レガシ情報のみ、新規教材ほぼなし |
| Xamarin(C#) | 2 | 1.0 | EOL のため新規教材なし |
| PWA(JS / TS) | 4 | 1.0 | MDN / web.dev / 各ブラウザベンダ公式が充実 |

---

## L7: AI / LLM 補完の精度

### アンカー

- **5**: 型情報多く誤補完少(TypeScript、Kotlin)
- **4**: 学習データ量が多く高精度(Java、JavaScript、C#)
- **3**: 中程度(Dart、Swift)
- **2**: 学習データ少なく型情報も限定的(KMP 新機能、Compose MP)
- **1**: 動的すぎて補完信頼性低(素の JavaScript の動的構造)

### スコア表

| FW(主言語) | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native(Kotlin) | 5 | 1.0 | Kotlin 型情報 + 大量学習データ |
| Flutter(Dart) | 3 | 1.0 | 中規模学習データ、型情報あり |
| React Native(JS / TS) | 5 | 1.0 | TS 想定で精度高、JS 単独だと 4 |
| Expo(JS / TS) | 5 | 1.0 | RN 同等 |
| Ionic Framework(TS) | 5 | 1.0 | TS 中心、Web 知識多 |
| Capacitor(TS) | 5 | 1.0 | TS 中心 |
| KMP(Kotlin) | 3 | 1.0 | Kotlin 自体は高精度だが KMP 固有 API は学習データ少 |
| Compose Multiplatform(Kotlin) | 2 | 1.0 | 新しく学習データ限定 |
| .NET MAUI(C#) | 4 | 1.0 | C# 高精度、MAUI 固有はやや学習データ少 |
| NativeScript(TS / JS) | 3 | 1.0 | TS 中心だが NS 固有は学習データ少 |
| Apache Cordova(JS) | 4 | 1.0 | JS 大量学習データだが Cordova 固有は古い |
| Xamarin(C#) | 4 | 1.0 | C# 高精度、ただし EOL で誤補完(MAUI と混在)あり得 |
| PWA(JS / TS) | 5 | 1.0 | Web 標準は学習データ最多 |

---

## L8: トラブルシューティング容易度

### アンカー

- **5**: エラーメッセージ自明 + 統合デバッガ強力(Kotlin + Android Studio、C# + Visual Studio)
- **4**: スタックトレース読みやすい(Java、Dart)
- **3**: 標準的(TypeScript、Swift)
- **2**: スタックトレース難読 or デバッガ制約あり(JavaScript の minified 環境)
- **1**: 多層エラーで原因特定困難(RN: Metro + ネイティブ層 + Xcode / Gradle 多層)

### スコア表

| FW(主言語) | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native(Kotlin) | 5 | 1.0 | Logcat + Android Studio Profiler、エラー直球 |
| Flutter(Dart) | 4 | 1.0 | flutter logs + DevTools |
| React Native(JS / TS) | 1 | 1.0 | Metro + ネイティブ層 + Xcode / Gradle で多層 |
| Expo(JS / TS) | 2 | 1.0 | Expo DevTools で改善されるが多層性は同じ |
| Ionic Framework(TS) | 3 | 1.0 | Web 開発と同感覚(Chrome DevTools)、ネイティブ問題は別途 |
| Capacitor(TS) | 3 | 1.0 | Web 側 Chrome DevTools、ネイティブは Android Studio / Xcode |
| KMP(Kotlin) | 3 | 1.0 | Android 側強力、iOS 側は Xcode 併用必要 |
| Compose Multiplatform(Kotlin) | 3 | 1.0 | KMP 同等 |
| .NET MAUI(C#) | 5 | 1.0 | Visual Studio Diagnostic Tools 強力 |
| NativeScript(TS / JS) | 3 | 1.0 | NS DevTools + Chrome DevTools |
| Apache Cordova(JS) | 3 | 1.0 | Web 開発感覚、ネイティブは別 |
| Xamarin(C#) | 4 | 1.0 | Visual Studio Debugger + Mac リモート(EOL) |
| PWA(JS / TS) | 4 | 1.0 | Chrome DevTools + Service Worker デバッガ充実 |

---

## 集計

| FW(主言語) | L1 | L2 | L3 | L4 | L5 | L6 | L7 | L8 | 単純平均 | 重み付き平均 | ★換算 |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Android Native(Kotlin) | 5 | 5 | 5 | 4 | 4 | 5 | 5 | 5 | 4.75 | 4.75 | ★★★★★ |
| Flutter(Dart) | 4 | 5 | 5 | 3 | 5 | 4 | 3 | 4 | 4.13 | 4.13 | ★★★★ |
| React Native(JS / TS) | 2 | 4 | 4 | 2 | 2 | 3 | 5 | 1 | 2.88 | 2.88 | ★★★ |
| Expo(JS / TS) | 2 | 4 | 4 | 2 | 4 | 3 | 5 | 2 | 3.25 | 3.25 | ★★★ |
| Ionic Framework(TS) | 4 | 4 | 3 | 1 | 3 | 3 | 5 | 3 | 3.25 | 3.25 | ★★★ |
| Capacitor(TS) | 4 | 4 | 4 | 4 | 4 | 3 | 5 | 3 | 3.88 | 3.88 | ★★★★ |
| KMP(Kotlin) | 5 | 5 | 5 | 4 | 2 | 2 | 3 | 3 | 3.63 | 3.63 | ★★★★ |
| Compose Multiplatform(Kotlin) | 4 | 5 | 5 | 3 | 2 | 2 | 2 | 3 | 3.25 | 3.25 | ★★★ |
| .NET MAUI(C#) | 5 | 4 | 4 | 4 | 4 | 3 | 4 | 5 | 4.13 | 4.13 | ★★★★ |
| NativeScript(TS / JS) | 4 | 4 | 4 | 3 | 3 | 1 | 3 | 3 | 3.13 | 3.13 | ★★★ |
| Apache Cordova(JS) | 3 | 1 | 4 | 4 | 3 | 2 | 4 | 3 | 3.00 | 3.00 | ★★★ |
| Xamarin(C#) | 4 | 3 | 4 | 4 | 3 | 2 | 4 | 4 | 3.50 | 3.50 | ★★★★ |
| PWA(JS / TS) | 4 | 4 | 4 | 4 | 1 | 4 | 5 | 4 | 3.75 | 3.75 | ★★★★ |

> 注: 各セルのスコアは上記アンカーに基づく初期値(2026-04-28 時点)。

---

## 更新履歴

- **2026-04-28**: 初版作成。L1〜L8 のアンカー定義と 13 FW の初期スコアを記載。
````

- [ ] **Step 2: 確認**

```bash
wc -l docs/checks/02_learning_difficulty.md
grep -c "^### スコア表" docs/checks/02_learning_difficulty.md
```

Expected: 250+ 行、`### スコア表` が 8 件

- [ ] **Step 3: コミット**

```bash
git add docs/checks/02_learning_difficulty.md
git commit -m "$(cat <<'EOF'
docs(checks): complete 02_learning_difficulty with L5-L8 and totals

Adds ecosystem learning load, doc richness, AI completion fit, and
troubleshooting ease (L5-L8) plus 13-FW summary with averages and
star conversion. 5=easiest convention preserved.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)"
```

---

### Task 7: 03_developer_experience.md(D1〜D4 + ヘッダ)

**Files:**
- Create: `docs/checks/03_developer_experience.md`

開発体験 8 項目のうち D1〜D4 を記載。

データソース: `selection_criteria.md` G1〜G13 + `framework_profiles.md` 開発環境節。

- [ ] **Step 1: ヘッダと D1〜D4 を作成**

`docs/checks/03_developer_experience.md` を新規作成し、以下を書く:

````markdown
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
- **4**: 公式 IDE 一発で完結、中程度(Android Native、Flutter Android のみ、.NET MAUI)
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
````

- [ ] **Step 2: ファイル内容を確認**

```bash
wc -l docs/checks/03_developer_experience.md
grep -c "^### スコア表" docs/checks/03_developer_experience.md
```

Expected: 100+ 行、`### スコア表` が 4 件

- [ ] **Step 3: コミット**

```bash
git add docs/checks/03_developer_experience.md
git commit -m "$(cat <<'EOF'
docs(checks): start 03_developer_experience with D1-D4

Header and 4 of 8 items: IDE integration, Hot Reload, setup/build
speed, debugger/profiler quality. Each item has anchor + 13-FW
score table.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)"
```

---

### Task 8: 03_developer_experience.md(D5〜D8 + 集計)

**Files:**
- Modify: `docs/checks/03_developer_experience.md`

D5〜D8 と集計表を追記する。

- [ ] **Step 1: D5〜D8 を追記**

`docs/checks/03_developer_experience.md` の末尾に以下を追記:

````markdown
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
````

- [ ] **Step 2: 確認**

```bash
wc -l docs/checks/03_developer_experience.md
grep -c "^### スコア表" docs/checks/03_developer_experience.md
```

Expected: 250+ 行、`### スコア表` が 8 件

- [ ] **Step 3: コミット**

```bash
git add docs/checks/03_developer_experience.md
git commit -m "$(cat <<'EOF'
docs(checks): complete 03_developer_experience with D5-D8 and totals

Adds test framework maturity, CI/CD ease, library ecosystem, and
dev-OS flexibility (D5-D8) plus 13-FW summary table.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)"
```

---

### Task 9: 00_overview.md

**Files:**
- Create: `docs/checks/00_overview.md`

01〜03 の集計★を転記し、端末対応マトリクスと併せて 1 行/FW でサマリ表を作成する。

依存: Task 4 / 6 / 8 が完了して各シートに集計★が確定していること。

- [ ] **Step 1: ヘッダ + 端末対応 + サマリ表を作成**

`docs/checks/00_overview.md` を新規作成し、以下を書く:

````markdown
# 概要(モバイル FW 評価チェックシート 一覧)

13 FW の **端末対応** と **3 種★点** を 1 表にまとめたサマリビュー。
評価ルール詳細は [`README.md`](./README.md)、各スコアの根拠は [`01_business_suitability.md`](./01_business_suitability.md) / [`02_learning_difficulty.md`](./02_learning_difficulty.md) / [`03_developer_experience.md`](./03_developer_experience.md) 参照。

凡例:
- ★★★★★ = 最良
- ★ = 最低
- 端末: ◎ = 全機能利用可 / ○ = 限定利用(拡張ライブラリ経由)/ × = 不可

---

## 端末対応マトリクス

| # | FW | 主言語 | Android | iOS | Windows | macOS | Linux | 組込機器 |
|---|---|---|---|---|---|---|---|---|
| 1 | Android Native | Kotlin | ◎ | × | × | × | × | ◎ |
| 2 | Flutter | Dart | ◎ | ◎ | ○ | ○ | ○ | ○ |
| 3 | React Native | JS / TS | ◎ | ◎ | ○ | ○ | × | × |
| 4 | Expo | JS / TS | ◎ | ◎ | × | × | × | × |
| 5 | Ionic Framework | TS | ◎ | ◎ | ○ | ○ | ○ | × |
| 6 | Capacitor | TS | ◎ | ◎ | ○ | ○ | ○ | × |
| 7 | KMP | Kotlin | ◎ | ◎ | ○ | ○ | ○ | △ |
| 8 | Compose Multiplatform | Kotlin | ◎ | ◎ | ◎ | ◎ | ◎ | × |
| 9 | .NET MAUI | C# | ◎ | ◎ | ◎ | ◎ | × | × |
| 10 | NativeScript | TS / JS | ◎ | ◎ | × | × | × | × |
| 11 | Apache Cordova | JS | ◎ | ◎ | × | × | × | × |
| 12 | Xamarin | C# | ◎ | ◎ | × | × | × | × |
| 13 | PWA | JS / TS | △ | △ | △ | △ | △ | × |

> 注: 端末欄の凡例
> - **◎**: 公式・第一級サポートで動作(プラットフォーム公式言語含む)
> - **○**: 拡張ライブラリ経由で動作(下表「限定利用ライブラリ等」参照)
> - **△**: ブラウザ経由で動作(PWA は OS の PWA 対応に依存、機能制限あり)
> - **×**: 動作不可
> - **Android Native の組込機器**: 業務HT(ハンディターミナル)等の Android ベース機器を含む

---

## 限定利用ライブラリ等

| FW | 限定利用ライブラリ等 |
|---|---|
| Android Native | — |
| Flutter | Flutter Desktop / Flutter Embedded / Flutter Web |
| React Native | RN for Windows / RN for macOS |
| Expo | (現状なし、Expo for Web は Web 出力のみ) |
| Ionic Framework | Capacitor Electron(デスクトップ)、Capacitor 単独で iOS / Android |
| Capacitor | Capacitor Electron / Tauri 統合 |
| KMP | Compose Multiplatform / Kotlin/JS / Kotlin Native |
| Compose Multiplatform | (本体で全プラットフォーム対応) |
| .NET MAUI | Mac Catalyst、Linux はサードパーティ実装 |
| NativeScript | — |
| Apache Cordova | — |
| Xamarin | — |
| PWA | OS の PWA 対応(Chromium 系強、Safari は機能制限) |

---

## サマリ(3 種★点)

| # | FW | 主言語 | 業務適正度 | 開発体験 | 習得難易度 |
|---|---|---|---|---|---|
| 1 | Android Native | Kotlin | ★★★★★ | ★★★★ | ★★★★★ |
| 2 | Flutter | Dart | ★★★ | ★★★★ | ★★★★ |
| 3 | React Native | JS / TS | ★★★ | ★★★★ | ★★★ |
| 4 | Expo | JS / TS | ★★★ | ★★★★ | ★★★ |
| 5 | Ionic Framework | TS | ★★★ | ★★★★ | ★★★ |
| 6 | Capacitor | TS | ★★★ | ★★★★ | ★★★★ |
| 7 | KMP | Kotlin | ★★★★ | ★★★ | ★★★★ |
| 8 | Compose Multiplatform | Kotlin | ★★★ | ★★★ | ★★★ |
| 9 | .NET MAUI | C# | ★★★★ | ★★★★ | ★★★★ |
| 10 | NativeScript | TS / JS | ★★ | ★★★ | ★★★ |
| 11 | Apache Cordova | JS | ★★ | ★★★ | ★★★ |
| 12 | Xamarin | C# | ★★ | ★★★ | ★★★★ |
| 13 | PWA | JS / TS | ★★★ | ★★★★★ | ★★★★ |

> 各★は対応シートの単純平均から導出した初期値。利用者は重みを編集して再計算する。

---

## 候補絞り込みのヒント

- **業務適正度 ★★★★以上**: Android Native / KMP / .NET MAUI(エンタープライズ向け第一候補)
- **開発体験 ★★★★以上**: Android Native / Flutter / React Native / Expo / Ionic / Capacitor / .NET MAUI / PWA(日々の生産性が高い)
- **習得難易度 ★★★★以上(=易しい)**: Android Native / Flutter / Capacitor / KMP / .NET MAUI / Xamarin / PWA
- **3 種すべて ★★★★以上**: **Android Native / .NET MAUI**(汎用的な業務アプリで両立しやすい)
- **3 種すべて ★★★以上**: 上記 + Flutter / KMP / Expo / Ionic / Capacitor / PWA / React Native / Compose Multiplatform(計 10 候補、NativeScript / Apache Cordova / Xamarin のみ除外)

---

## 詳細シートへの導線

- 業務適正度の根拠 → [`01_business_suitability.md`](./01_business_suitability.md)
- 習得難易度の根拠 → [`02_learning_difficulty.md`](./02_learning_difficulty.md)
- 開発体験の根拠 → [`03_developer_experience.md`](./03_developer_experience.md)
- 言語特性(対応領域・採用例・PWA 対応等)→ [`04_language_features.md`](./04_language_features.md)

---

## 更新履歴

- **2026-04-28**: 初版作成。13 FW の端末対応・3 種★を集約。
````

- [ ] **Step 2: 確認**

```bash
wc -l docs/checks/00_overview.md
ls docs/checks/
```

Expected: 80+ 行、`docs/checks/` に 6 ファイル(README + 00〜04)

- [ ] **Step 3: 全シート整合性チェック**

各スコアシートの集計★と概要シートの★が一致しているか目視確認:

```bash
grep -E "(Android Native|Flutter|React Native|Expo|Ionic Framework|Capacitor|KMP|Compose Multiplatform|.NET MAUI|NativeScript|Apache Cordova|Xamarin|PWA)" docs/checks/00_overview.md | grep -E "★"
```

Expected: 13 行のサマリが表示され、★点が `01〜03` の集計と一致

- [ ] **Step 4: コミット**

```bash
git add docs/checks/00_overview.md
git commit -m "$(cat <<'EOF'
docs(checks): add 00_overview with device matrix and 3-star summary

13 FWs by 6 device categories (Android/iOS/Win/Mac/Linux/Embedded)
plus business/DX/learning star ratings consolidated from 01-03 sheets.
Includes shortcut filters for "all 4-star+" candidates.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)"
```

---

## 完了確認

すべてのタスク完了後、以下を確認:

- [ ] `docs/checks/` 配下に 6 ファイル(README.md + 00〜04 の 5 シート)
- [ ] 各スコアシート(01/02/03)に 8 つのアンカー + 8 つのスコア表 + 集計表
- [ ] 概要シート(00)の★点が 01/02/03 の集計と整合
- [ ] 言語仕様シート(04)に 6 言語のカード
- [ ] README に評価ルール・利用ワークフロー・固有事情合成方法
- [ ] 9 コミット(各タスク 1 コミット)
- [ ] `git log --oneline -10` で `docs(checks):` プレフィックスを 9 件確認

```bash
ls docs/checks/
git log --oneline -12 | grep "docs(checks)"
```

Expected: 6 ファイル列挙 / 9 コミット表示
