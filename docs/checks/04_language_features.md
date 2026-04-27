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
