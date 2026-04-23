# 詳細プロファイル: Kotlin(言語)

本ドキュメントは `framework_profiles.md` の「Kotlin」項目を **言語単体の視点** から深掘りした詳細プロファイル。
`profile_android_native.md` は FW としての Android ネイティブ環境を扱うのに対し、本ファイルは **Kotlin という言語自体** の採用判断に必要な情報を集約する。

- 情報は **2026 年 4 月時点**。バージョンや EOL は公式で採用時点再確認必須
- 観点 A〜H(`selection_criteria.md`)を言語単体の視点に投影
- 本件前提(Android 業務アプリ / jQuery+Java チーム)下での評価を随所に付記

---

## 0. 結論サマリ

| 項目 | 評価 |
|---|---|
| 総合推奨度(本件) | ★★★★★(Android + Java 経験者チームで競合なし) |
| 技術リスク | 最小(Android公式、JVM 世界標準に乗る) |
| 保守リスク | 最小(JetBrains + Google + Kotlin Foundation) |
| 学習曲線形状 | **初手楽 → 中盤スロープ(coroutine)→ 長期安定** |
| 立ち上げ期間(Java経験者) | 基礎 2 週、実務 1〜2ヶ月、エキスパート 6ヶ月〜1年 |
| Android 外での転用価値 | ◎(サーバ Ktor / Spring、Desktop Compose、Gradle DSL、CLI、KMP) |
| AI/LLM 補完相性 | ◎(型情報豊富、学習データも多い) |

---

## 1. 基本情報

### 1.1 開発元・ガバナンス・Bus Factor

| 項目 | 内容 |
|---|---|
| 発案者 | Andrey Breslav(JetBrains、2011 年提唱) |
| 開発元 | **JetBrains**(主開発元、IntelliJ IDEA を作る企業) |
| 公式後援 | **Google**(Android 公式推奨言語、2019-05 ~) |
| ガバナンス | **Kotlin Foundation**(JetBrains + Google 共同設立、2018)、言語仕様変更を管理 |
| Bus Factor | **中〜高**。JetBrains 本体依存度が中心だが、Google/Foundation の存在で単一企業リスクは軽減 |
| コントリビューション | オープン、GitHub 上で議論・PR 可 |

### 1.2 ライセンス

| 対象 | ライセンス |
|---|---|
| Kotlin コンパイラ | **Apache License 2.0** |
| 標準ライブラリ | Apache License 2.0 |
| kotlinx ライブラリ群 | Apache License 2.0 |
| 商用利用 | 無制限、改変・再配布可 |
| 商用サポート | **JetBrains 商用契約**で相談窓口・優先サポート可 |

### 1.3 バージョン履歴(主要マイルストーン)

| 年月 | バージョン | 主な変更 |
|---|---|---|
| 2016-02 | **1.0** | 言語仕様安定化、Android/JVM 正式 GA |
| 2017-05 | — | Google I/O で Android 公式サポート発表 |
| 2018-10 | 1.3 | **coroutine 安定化**、`inline class`、型推論強化 |
| 2019-05 | — | Google が Android 向けに **Kotlin First** 宣言 |
| 2020-08 | 1.4 | **null safety** 型推論改善、KMM Alpha |
| 2021-08 | 1.5 | JVM 記録、`StateFlow` 一般化 |
| 2022-06 | 1.7 | K2 compiler Alpha |
| 2023-04 | 1.8 | K2 compiler Beta |
| 2023-07 | 1.9 | KAPT → KSP 2 推奨、K2 Beta |
| **2023-11** | — | **Kotlin Multiplatform Stable** 到達 |
| 2024-05 | **2.0** | **K2 compiler デフォルト化**、性能 2x |
| 2024-05 | — | **Compose Multiplatform for iOS Stable** |
| 2024-11 | 2.1 | `when` の guard、多変数 `typealias`、Kotlin/Wasm β |
| 2025-06 頃 | 2.2(予定) | Context parameters、Kotlin/Wasm Stable 見込み |
| 2026-04 時点 | 2.x 系 | K2 が既定、KMP 定着 |

### 1.4 リリース・アップデートサイクル

| 対象 | 頻度 |
|---|---|
| メジャー(2.0、2.1、2.2…) | **6ヶ月おき** |
| マイナー / パッチ | 随時 |
| EAP(Early Access Preview) | 継続 |
| リリース予告 | 公式ロードマップで 1 年先まで明示 |

**業務アプリ観点**: 6ヶ月サイクルは .NET / Angular と同じ規則性。計画が立てやすい。

### 1.5 LTS・サポート期限・EOL

| 項目 | 内容 |
|---|---|
| 明示 LTS | **なし**(JetBrains 方針) |
| 後方互換 | **事実上維持**。旧 Kotlin で書いたコードが新 Kotlin でも動く前提が強い |
| EOL | 明示なし。ただし古いバージョンは K2 移行などで非推奨化 |
| 採用指針 | 原則 **メジャー 2 世代以内を維持**(2025 年時点なら 2.0 以上推奨) |

### 1.6 破壊的変更頻度・後方互換ポリシー(H15 / F6)

| 局面 | 変更影響度 |
|---|---|
| 言語文法レベル | **極めて低い**。deprecation を 1〜2 バージョンかけて段階化 |
| 標準ライブラリ | 低い。`@Deprecated` マーク → 削除は複数バージョン猶予 |
| 2.0 K2 移行 | **中**(一部の KAPT プロセッサが KSP 2 へ要移行)だが **ソース互換は維持** |
| Android 向け付随変更 | Android SDK 側の変更のほうが大きい |

### 1.7 脆弱性対応・CVE 体制

| 項目 | 内容 |
|---|---|
| 窓口 | JetBrains Security(`security@kotlinlang.org`) |
| 公開フロー | GitHub Security Advisories |
| 対応速度 | 迅速(企業バックアップ) |
| 脆弱性履歴 | 言語本体の CVE は極めて少ない(JVM/Android側の CVE のほうが主) |

### 1.8 商用サポート

- **JetBrains 商用ライセンス**(IntelliJ IDEA Ultimate 契約など)で相談可
- 金融/医療/公共で SLA 必要な場合は交渉可能
- 直接の **Kotlin 言語 SLA 契約**は限定的だが、JetBrains の企業契約で通例カバー

---

## 2. 言語特性

### 2.1 パラダイム

| パラダイム | 対応 |
|---|---|
| オブジェクト指向 | ◎ class、interface、継承、可視性修飾子、data class |
| 関数型 | ◎ first-class functions、lambdas、高階関数、`map`/`filter`/`fold` |
| 型駆動 | ◎ sealed class、enum、pattern(`when`)、型推論 |
| リアクティブ | ◎ Flow、StateFlow、SharedFlow(公式) |
| DSL 設計 | ◎ 型安全 DSL(Gradle Kotlin DSL、Compose、Ktor の routing はこの例) |
| メタプログラミング | 中(Reflection / KSP / Compiler Plugin) |
| 並行プログラミング | ◎ coroutine + structured concurrency |

### 2.2 型システム

| 特性 | 内容 |
|---|---|
| 静的型付け | ◎ コンパイル時検査 |
| 型推論 | 強力(ほぼどこでも型注釈省略可) |
| **null safety** | **言語コアに統合**。`String` と `String?` は別型、`?.` / `!!` / `?:` 演算子 |
| ジェネリクス | ◎ variance 注釈(`in` / `out`)、bounds、reified(`inline fun`) |
| Sum type | sealed class / sealed interface で表現 |
| Data class | 構造的等価・copy・分解宣言を自動生成 |
| Value class / Inline class | JVM 上でボクシング回避 |
| 型エイリアス | `typealias` |
| 公称的 | ◎(構造的ではない) |
| 依存型 | × |

### 2.3 メモリ管理モデル(H2)

| ターゲット | モデル |
|---|---|
| Kotlin/JVM(Android / Server) | **JVM GC**(ART 上は世代別 GC、サーバは G1 / ZGC / Shenandoah 選択可) |
| Kotlin/Native(iOS / Desktop / Embedded) | **Memory Manager v2**(並行 Mark & Sweep、2022 Stable)。Objective-C/Swift ARC と統合 |
| Kotlin/JS | JS エンジン GC(Hermes / V8) |
| Kotlin/Wasm | Wasm GC(ブラウザ/ランタイム依存) |

**業務アプリ観点**: Android は ART の GC 制御下で運用、堅牢。低遅延が必要なら `ZGC` 等を選択するのはサーバ側のみ。

### 2.4 並行・非同期モデル

**中核: Kotlin Coroutines(`kotlinx.coroutines`)**

| 概念 | 内容 |
|---|---|
| `suspend fun` | 中断可能関数(中断点で CPU を他の仕事に譲る) |
| **Structured Concurrency** | 親子関係でキャンセル伝播、リソース漏れ防止 |
| `CoroutineScope` | ライフサイクル管理単位(Activity / ViewModel に紐付け推奨) |
| `CoroutineContext` | Dispatcher、Job、例外ハンドラを束ねる |
| `Dispatchers` | `Default`(CPU bound)/ `IO`(ブロッキング IO)/ `Main`(UI)/ `Unconfined` |
| `async` / `await` | 並列実行 |
| `launch` | fire and forget |
| `withContext` | Dispatcher 切替 |
| `Flow` | cold stream(RxJava 相当) |
| `StateFlow` / `SharedFlow` | ホットストリーム(状態ホルダ / イベントバス) |
| `Channel` | Producer-Consumer |

**Java/Spring 経験者への親和性**: `CompletableFuture` / `reactor.Mono` を使った経験があれば理解は早い。むしろ coroutine のほうが命令的に書ける分直感的。

### 2.5 エラーハンドリング哲学(H3)

| 機構 | 使い所 |
|---|---|
| 例外(`throw` / `try/catch`) | Java 互換、主流。Kotlin は **検査例外を廃止** |
| `Result<T>` | 関数型的に成功/失敗を値として扱う |
| `?.` + `?:`(Elvis) | null 経路をその場で処理 |
| sealed class | ドメイン固有エラーの網羅的分岐 |
| `runCatching { }` + `onSuccess` / `onFailure` | 例外 → Result 変換 |

**特徴**: Java の **検査例外を廃止**。宣言負担は減るが、「想定外の例外」を拾う習慣が重要。業務アプリでは `Result`(または Either/Arrow)+ sealed class で明示モデリングが推奨。

### 2.6 メタプログラミング(H5)

| 技術 | 内容 |
|---|---|
| **Kotlin Reflection** | `kotlin-reflect` ライブラリ、クラス構造・アノテーション参照 |
| **KAPT**(Kotlin Annotation Processing Tool) | Java APT の Kotlin 互換、**K1 時代の主流**、遅い |
| **KSP**(Kotlin Symbol Processing) | **K2 時代の後継**、2 倍以上速い、マルチプラットフォーム対応 |
| **Compiler Plugins** | Compose Compiler、all-open、no-arg、serialization、Parcelize などが実装例 |
| **kotlinx.serialization** | コンパイラプラグイン駆動の JSON/Protobuf シリアライザ |
| Source Generation | Anvil(DI)、Moshi Sealed など、KSP ベースが主流化 |
| Annotation Processing | Hilt / Room / Dagger / Moshi は全部 Annotation 処理ベース |

**業務アプリ観点**: Room、Hilt、Moshi が **KSP 2 前提に移行中**。新規採用では最初から KSP ベースを選ぶ。

### 2.7 相互運用性・FFI(H6)

| 他言語 | 連携方法 |
|---|---|
| **Java** | **完全相互運用**、bidirectional。Android Studio で Java↔Kotlin 自動変換 |
| C(JNI) | Android NDK 経由、Java JNI と同様 |
| **C / Objective-C / Swift**(Kotlin Native) | `cinterop` ツールで自動バインディング生成、iOS SDK 呼び出し可 |
| JS | Kotlin/JS + `external` 宣言で TypeScript 型定義を投影 |
| Wasm(ブラウザ) | Kotlin/Wasm + JSInterop |

**業務アプリ観点**: 既存 Java ライブラリ(Jackson / OkHttp / Apache Commons / 社内共通ライブラリ)を **一切書き換えずそのまま使える**のが圧倒的に強い。

### 2.8 設計哲学(H17)

| 原則 | 意味 |
|---|---|
| Pragmatic | 「Java より良く、Scala より単純に」。実用主義 |
| Interoperable | Java との完全双方向互換 |
| Safe | null safety、immutability(`val`)を基本 |
| Tool-friendly | IDE・コンパイラ・ビルドが最初から統合設計 |
| Concise | ボイラープレート最小化(data class、property、lambdas) |

### 2.9 コーディング規約・スタイル文化(H16)

| 項目 | 内容 |
|---|---|
| 公式スタイルガイド | https://kotlinlang.org/docs/coding-conventions.html |
| Android 向け追加 | https://developer.android.com/kotlin/style-guide |
| IntelliJ デフォルト | 公式規約に準拠、`.editorconfig` で共有推奨 |
| Linter デフォルトの厳しさ | **ktlint** はオフィシャル規約準拠でほぼ何もしなくても動く、**Detekt** は規則追加 |
| チーム文化 | 公式規約が強く、「自社規約」の必要性が低い |

---

## 3. 標準ライブラリ・ランタイム(H4)

### 3.1 コンパイル先とランタイム方式

| ターゲット | コンパイル先 | 実行環境 | 実行方式 |
|---|---|---|---|
| **Kotlin/JVM** | JVM bytecode(`.class`) | HotSpot / OpenJDK / GraalVM / ART(Android) | JIT(+AOT on ART) |
| **Kotlin/Android** | DEX bytecode | ART | AOT + JIT(ART) |
| **Kotlin/Native** | LLVM → ネイティブバイナリ | Linux/macOS/Win/iOS/組込 | AOT |
| **Kotlin/JS** | JS コード | ブラウザ / Node.js | JIT(JS エンジン) |
| **Kotlin/Wasm** | WebAssembly | ブラウザ / Node.js / Wasm ランタイム | AOT |

### 3.2 標準ライブラリ(`kotlin.*`)の範囲

| パッケージ | 内容 |
|---|---|
| `kotlin` | 基本型(`Int` / `String` / `Nothing` / `Unit`)、基本関数 |
| `kotlin.collections` | List / Set / Map、immutable/mutable 区別、操作関数(`map` / `filter` 等) |
| `kotlin.sequences` | 遅延評価(Java Stream 相当) |
| `kotlin.text` | 文字列・Regex |
| `kotlin.io` | 標準入出力、File 拡張 |
| `kotlin.ranges` | Range(`1..100`) |
| `kotlin.reflect` | Reflection(別ライブラリ `kotlin-reflect.jar`) |
| `kotlin.concurrent` | 低レベル同期(通常は coroutine 使用) |

**kotlinx 拡張ライブラリ**(JetBrains 公式):

| パッケージ | 内容 |
|---|---|
| **kotlinx.coroutines** | 並行処理の事実上の標準 |
| **kotlinx.serialization** | JSON / Protobuf / CBOR シリアライザ(コンパイラプラグイン) |
| **kotlinx.datetime** | マルチプラットフォーム日時 |
| **kotlinx.io** | マルチプラットフォーム IO |
| kotlinx.collections.immutable | 永続コレクション |
| kotlinx.html | HTML DSL |

**業務アプリ観点**: kotlinx 群は事実上の標準として扱ってよい。java.time / java.util.concurrent から kotlinx 版へ移行が推奨傾向。

### 3.3 JVM 標準ライブラリとの関係

Kotlin/JVM では **JVM 標準ライブラリ(java.*)全てが直接利用可**。Kotlin の標準ライブラリは JVM の上に薄いレイヤを被せるか、JS/Native 向けに別実装を提供する構造。

---

## 4. 開発環境・ツーリング(H7 / H8 / H13)

### 4.1 コンパイラ

| 項目 | K1(旧) | K2(現行、2.0+) |
|---|---|---|
| 内部アーキテクチャ | 古い | モジュール化・並列化 |
| **コンパイル速度** | 基準 | **1.5〜2 倍速** |
| メモリ使用 | 多い | 少ない |
| IDE 連携 | 旧 PSI 統合 | **K2 IDE plugin** で高速解析 |
| KAPT | 動作 | 動作するが **KSP 推奨** |
| Compose Compiler | 別プラグインに分離(Kotlin 2.0+) | 同左 |
| 安定性 | 安定 | 2.0 以降 **デフォルト、Stable** |

### 4.2 IDE・LSP・補完品質(H8)

| IDE | 評価 | 備考 |
|---|---|---|
| **IntelliJ IDEA Ultimate / Community** | ◎ 最強 | JetBrains 本家、全機能最高速 |
| **Android Studio** | ◎ | IntelliJ Community ベース、Android 特化 |
| **JetBrains Fleet** | ○ | 次世代軽量 IDE、Kotlin 対応成熟中 |
| VS Code + Kotlin Language Server | △ | Community 製 LSP、IntelliJ に比べ補完・リファクタ精度で劣る |
| Vim / Emacs + kotlin-language-server | △ | 同上 |

**Kotlin LSP プロトコル**: JetBrains が公式 LSP 提供は未対応(2026 時点)。VS Code での開発は Android Studio/IntelliJ のほうが体験が圧倒的に良い。

**補完・リファクタ能力**: IntelliJ の kotlin プラグインは **型駆動の補完精度が高く**、Smart Completion、Rename、Extract Function などが強力。Java 時代からの JetBrains のリファクタリング機能資産を継承。

### 4.3 ビルドツール・パッケージ管理

| ツール | 用途 |
|---|---|
| **Gradle**(Kotlin DSL 推奨) | 事実上の標準、Android/KMP 必須 |
| Maven | 可、サーバ・ライブラリ配布で一部採用 |
| Bazel | Large-scale Mono-repo 向け(Uber 等) |
| Amper(JetBrains、実験的) | YAML/TOML 的な軽量ビルド定義、将来 Gradle 代替を狙う |

**パッケージレジストリ**:

- **Maven Central**(公式最大)
- **Google Maven**(Android 用)
- **GitHub Packages / JitPack**(OSS 配布)

### 4.4 Lint / Formatter

| ツール | 役割 |
|---|---|
| **ktlint** | 公式規約準拠、スタイル統一、`ktlint --format` |
| **Detekt** | 静的解析、複雑度、依存方向、禁止パターン |
| Android Lint | Android 向けチェック(Kotlin 対応) |
| IntelliJ Inspection | IDE 内の即時解析 |
| SonarQube | 大規模プロジェクトの品質ダッシュボード |

### 4.5 デバッガ・プロファイラ

| ツール | 用途 |
|---|---|
| **IntelliJ Debugger / Android Studio Debugger** | JVM ブレークポイント、Coroutine aware(スタック追跡) |
| **Coroutine Debugger**(IntelliJ 組込) | コルーチン階層・中断点の可視化 |
| **IntelliJ Profiler**(Async Profiler 統合) | CPU / Memory プロファイリング |
| **Android Studio Profiler** | Android 専用(Memory / Network / Energy) |
| YourKit / JProfiler | 商用 JVM プロファイラ |

**スタックトレース可読性**(H7): Kotlin のスタックトレースは Java と同等(JVM bytecode のため)。**Coroutine Stack Trace** は IntelliJ で可視化可、従来のスレッドモデルより追いやすい。

### 4.6 コンパイル速度(H13)

| ビルド種別 | 体感 |
|---|---|
| **インクリメンタル(編集→実行)** | K2 で大幅改善、秒オーダー |
| **クリーンビルド(Android アプリ 中規模)** | 数十秒〜数分 |
| Gradle 構成キャッシュ | `--configuration-cache` で起動高速化 |
| Gradle ビルドキャッシュ | ローカル + Remote Cache(Gradle Enterprise) |
| 並列化 | `org.gradle.parallel=true` |

**業務アプリ観点**: Android Studio + K2 で **小規模編集は数秒で実行可**。CI ではフルビルド時間(数分)を織り込む。

---

## 5. 用途別フレームワーク(何が作れるか)

### 5.1 Android モバイル

| 用途 | 代表 |
|---|---|
| 宣言的 UI | **Jetpack Compose**(公式) |
| 従来 UI | Android View XML(レガシ継続) |
| DI | **Hilt**(Dagger の薄いラッパ) / Koin |
| DB | Room / SQLDelight |
| 通信 | Retrofit / Ktor Client |
| シリアライズ | kotlinx.serialization / Moshi / Gson |
| 非同期 | Coroutines / Flow |
| バックグラウンド | WorkManager |
| 状態管理 | ViewModel + StateFlow |

### 5.2 iOS モバイル(Kotlin Multiplatform)

| 用途 | 代表 |
|---|---|
| ビジネスロジック共通化 | **KMP**(Android/iOS/Desktop/Web) |
| UI 共通化 | **Compose Multiplatform for iOS**(2024-05 Stable) |
| UI ネイティブ継続 | SwiftUI + KMP 共通ロジック呼び出し |
| 通信 | Ktor Client |
| DB | SQLDelight / Realm KMP |
| DI | Koin / Kodein |
| 代表採用企業 | Netflix、Cash App、McDonald's、Philips、Forbes |

### 5.3 サーバサイド

| 用途 | 代表 |
|---|---|
| 軽量 Web | **Ktor**(JetBrains 公式、coroutine ファースト) |
| エンタープライズ | **Spring Boot**(Kotlin first-class サポート 2017〜) |
| Functional | http4k / Arrow |
| DI | Spring / Koin / Kodein |
| DB | Spring Data / Exposed / jOOQ |
| マイクロサービス | Micronaut Kotlin、Quarkus Kotlin |
| メッセージング | kafka-clients-kotlin、RabbitMQ |

**業務アプリ観点**: 既存の Java+Spring サーバ資産は **そのまま Kotlin 化可**(Spring のアノテーションが Kotlin で動く)。移行は段階的に可能。

### 5.4 デスクトップ

| 用途 | 代表 |
|---|---|
| **Compose Multiplatform Desktop** | Windows / macOS / Linux 向け宣言的 UI |
| JavaFX + TornadoFX | Kotlin DSL ラッパ |
| Swing | Kotlin で書き直し可(レガシ用途) |

### 5.5 Web

| 用途 | 代表 |
|---|---|
| Kotlin/JS + React Wrapper | `kotlin-react` 公式、但し TS React 比で小規模 |
| Kotlin/Wasm + Compose HTML | 2025〜本格化予定(実験的) |
| ssr / フルスタック | Ktor + kotlin-react |

### 5.6 組込・IoT

| 用途 | 代表 |
|---|---|
| Kotlin/Native on Raspberry Pi | LLVM ネイティブコード、ARM64 対応 |
| Android Things | 2022 年終了 |

### 5.7 CLI・スクリプト

| 用途 | 代表 |
|---|---|
| CLI アプリ | **Clikt**(宣言的 CLI)、kotlinx-cli |
| スクリプト | `.kts`(Kotlin Scripts)、`kotlinc -script` |
| Gradle ビルドスクリプト | Kotlin DSL |
| Notebook | Kotlin Jupyter、Kotlin Notebook(JetBrains) |

### 5.8 データ・ML

| 用途 | 代表 |
|---|---|
| ML | **KotlinDL**(JetBrains、TensorFlow ラッパ) |
| 数値計算 | multik |
| データ処理 | Kotlin DataFrame(JetBrains、Pandas 相当) |

**結論**: **Kotlin は Android + サーバ + Desktop + Gradle DSL + KMP モバイルまで 1 言語で広くカバー**。データサイエンスやゲームは弱いが、業務アプリの 90% 以上のユースケースはカバー。

---

## 6. パフォーマンス特性(H1 / H14)

### 6.1 起動時間

| 環境 | 実測傾向 |
|---|---|
| Android(ART) | Java と同等〜わずかに遅い(数十ミリ秒差、体感ほぼ無し) |
| JVM サーバ(HotSpot) | Java と同等、warmup 後はほぼ同一 |
| **GraalVM native-image** | ミリ秒オーダー起動、Lambda 用途で有効 |
| Kotlin/Native | AOT のため即時起動 |

### 6.2 スループット

| 環境 | 対 Java 比 |
|---|---|
| Kotlin/JVM(warmup 後) | **99%〜100%**(最終 JIT コードはほぼ同一) |
| JVM 短命プロセス | 数%遅い(クラスロード・初期化分) |
| Coroutine vs Thread | **軽量**(数万 coroutine ≒ 数百 thread のコスト) |

### 6.3 メモリフットプリント

| 項目 | サイズ |
|---|---|
| kotlin-stdlib JAR | 約 1.7 MB |
| Android APK 寄与(R8 縮小後) | 約 200〜400 KB |
| Coroutine 1本あたりのメモリ | 数百バイト(thread の 1/10,000 以下) |
| Kotlin Native バイナリ | AOT のため数 MB 〜 |

### 6.4 GC 挙動

JVM GC に依存。Android ART はモバイル向けに調整済み GC(Concurrent Copying GC など)。業務アプリで GC pause が顕在化することは稀。

### 6.5 バイナリ・APK サイズ寄与(H14)

| 要素 | 影響 |
|---|---|
| kotlin-stdlib | R8 縮小で数百 KB まで圧縮 |
| kotlinx.coroutines | 数百 KB 〜 |
| kotlinx.serialization | 中程度 |
| Compose 関連 | **数 MB**(Compose Runtime + Material3) |
| R8 最適化 | 必須。ProGuard rules を各ライブラリ推奨設定で |

### 6.6 ベンチマーク参照

| ベンチマーク | リンク |
|---|---|
| Kotlin K2 性能改善 | https://blog.jetbrains.com/kotlin/2024/04/k2-compiler-performance-benchmarks-and-how-to-measure-them-on-your-projects/ |
| Compose vs XML Views 性能 | https://developer.android.com/jetpack/compose/performance |

---

## 7. エコシステム・コミュニティ(H11 / H12)

### 7.1 パッケージ・ライブラリ規模

| レジストリ | Kotlin 関連規模 |
|---|---|
| Maven Central | Kotlin-specific 数千(`org.jetbrains.kotlinx:*`)+ Java 互換ライブラリ多数 |
| Google Maven(AndroidX) | Compose / Lifecycle / WorkManager 他 100+ モジュール |
| Gradle Plugin Portal | Kotlin 製プラグイン多数 |

### 7.2 GitHub 定量指標(H11、2026-04 時点目安)

| リポジトリ | Stars(概算) |
|---|---|
| JetBrains/kotlin | 約 50k |
| Kotlin/kotlinx.coroutines | 約 15k |
| JetBrains/compose-multiplatform | 約 18k |
| ktorio/ktor | 約 13k |
| KSP / Kotlin/ksp | 約 3.5k |

### 7.3 年次調査(Stack Overflow Developer Survey 等)

| 調査項目 | 近年傾向 |
|---|---|
| Stack Overflow Developer Survey — Most Loved | **TOP 10 圏内を継続**(2019〜) |
| Stack Overflow Tag Activity | Android 向け質問は Kotlin が Java を逆転済 |
| GitHub Octoverse Top Languages | TOP 15 圏内で安定 |
| JetBrains Developer Ecosystem Survey | Kotlin 利用者の 70%+ が「今後も使い続けたい」と回答 |

### 7.4 日本語リソース(H12)

| 種別 | 代表 |
|---|---|
| 公式日本語ドキュメント | Kotlin 公式サイトに日本語版あり(翻訳コミュニティ経由) |
| 書籍 | 『Kotlin イン・アクション』『Kotlin スタートブック』『実践 Kotlin』 |
| 国内カンファレンス | **Kotlin Fest**(2018〜、東京、年 1)、Droidcon Tokyo |
| 国内コミュニティ | Kotlin Tokyo、各地の勉強会(connpass) |
| 日本語技術ブログ | Yahoo! Japan、DeNA、メルカリ、ヤフー、サイバーエージェント各社の Android チーム |

### 7.5 認定・トレーニング

- **JetBrains Academy**(公式オンライン学習、無償プラン有)
- **Android Developer Kotlin Training**(Google 公式)
- **Kotlin for Java Developers**(Coursera、JetBrains 提供)
- 公式認定資格は存在しない(Java の Oracle Certified 相当は無し)

### 7.6 コミュニティガバナンス

- **Kotlin KEEP**(Kotlin Evolution and Enhancement Process): https://github.com/Kotlin/KEEP
- 言語変更は提案 → 議論 → 採択のオープンプロセス
- Kotlin Foundation の役員会が方針承認

---

## 8. 主要採用事例(H9)

### 8.1 Android モバイル

| 企業 | 用途 |
|---|---|
| Google | Google Pay、多数の Android 純正アプリ |
| Pinterest、Square、Uber、Trello、Duolingo、Evernote、Airbnb | Android アプリ |
| Netflix | Android + iOS(KMP)、サーバ |
| 国内: メルカリ、LINE、Yahoo!、DeNA、サイバーエージェント、Nintendo(アプリ) | Android アプリ |

### 8.2 サーバサイド

| 企業 | 用途 |
|---|---|
| Amazon | 一部サービス(Ktor/Spring Kotlin) |
| Adobe、Expedia、Zalando | バックエンド |
| 国内: mixi、atama plus、freee、ラクスル 等 | サーバ(Spring + Kotlin) |

### 8.3 KMP

| 企業 | 用途 |
|---|---|
| **Netflix** | iOS/Android ロジック共通化 |
| **Cash App**(Block) | iOS/Android/Desktop 統一 |
| **McDonald's**、Philips、Forbes、Careem、9GAG | 各所 |

### 8.4 ドメイン別

| ドメイン | 採用実績 |
|---|---|
| **物流・倉庫・小売 HT** | Zebra 公式サンプル・パートナー実装多 |
| **Fintech** | Cash App、Square |
| **金融**(国内) | 銀行系サーバで Spring+Kotlin 採用増 |
| **医療** | Philips(KMP) |
| **官公庁**(国内) | 限定的、Java 資産継続が主 |
| **SI(国内)** | 大手 SIer では Java 中心だが、Kotlin 採用案件は増加傾向 |
| **ゲーム** | △ Unity/C# が主、libGDX Kotlin 採用は少数 |

---

## 9. AI/LLM 補完相性(H10)

| 観点 | Kotlin |
|---|---|
| 学習データ量 | ◎ **Java と共通基盤**(JVM) + 独自の大量コード資産 |
| 型情報活用 | ◎ 静的型付け + null safety で **補完精度が高い** |
| GitHub Copilot 精度 | ◎ Android / Compose は十分な学習量、Copilot の提案が信頼できる |
| ChatGPT / Claude 精度 | ◎ Android 関連ドキュメント質問への回答精度が高い |
| 誤補完傾向 | 稀に古い API(View system など)を提案することあり |
| Compose DSL 補完 | ◎ 型情報豊富なので、Modifier チェーンも精度高く提案 |
| 結論 | **AI 開発支援が最も効く言語の 1 つ** |

---

## 10. 学習パス(H18)

### 10.1 学習曲線の形状

```
難易度
  │
  │              ┌─────────  エキスパート(6ヶ月〜)
  │           ___/           言語機能・規約・パフォーマンス
  │         _/
  │       _/                 実務(1〜2ヶ月)
  │   ___/                   coroutine・Flow・Compose
  │ _/
  │/                         基礎(1〜2週)
  │                          文法・null safety
  └──────────────────────→ 時間
```

**形状の特徴**:

- **初手楽**(Java 経験者は 1〜2 週で書ける)
- **中盤スロープ**(coroutine、Flow、Compose 状態管理は別モデル)
- **長期安定**(後方互換が強く、一度習得すれば陳腐化しにくい)

### 10.2 基礎習得(1〜2 週、Java 経験者前提)

| 項目 | 内容 |
|---|---|
| null safety | `?` / `!!` / `?:` / `let` |
| data class | `copy` / 分解宣言 |
| extension function | Java にない拡張機能 |
| scope functions | `let` / `apply` / `run` / `with` / `also` |
| lambda / ラムダ受渡 | `it` / SAM 変換 |
| プロパティ | getter/setter 自動化 |
| when 式 | switch の上位互換 |

**演習**: Android Studio の Java→Kotlin 自動変換で既存 Java コードを変換 → 動作確認 → 手修正 で身体に馴染ませる。

### 10.3 実務レベル(1〜2 ヶ月)

| 項目 | 内容 |
|---|---|
| coroutine | structured concurrency、CoroutineScope、Dispatcher |
| Flow / StateFlow | cold/hot stream、collect、operator chain |
| sealed class + when | ドメインモデル、UI 状態 |
| 高階関数 + inline | 関数型の基本パターン |
| delegate(`by lazy`、`by viewModels()`)| |
| Jetpack Compose 基礎 | State、remember、Modifier、Material3 |
| Hilt DI | `@HiltViewModel`、`@Inject` |
| Room + Coroutine | suspend fun DAO |

### 10.4 エキスパート(6 ヶ月〜)

| 項目 | 内容 |
|---|---|
| ジェネリクス variance | `in` / `out`、type projection |
| inline / crossinline / noinline | 性能と使い分け |
| Compiler Plugin / KSP | Room/Hilt 等の仕組み理解 |
| Compose 内部(リコンポジション) | 最適化、Stability |
| Kotlin Multiplatform | expected/actual、共通ロジック設計 |
| DSL 設計 | Ktor Route / Gradle DSL のような受け手設計 |

### 10.5 推奨教材・書籍

| カテゴリ | 教材 |
|---|---|
| 公式 | [Kotlin 公式 Docs](https://kotlinlang.org/docs/home.html)、[Kotlin Koans](https://play.kotlinlang.org/koans/) |
| Google 公式コース | [Android Basics with Compose](https://developer.android.com/courses) |
| 書籍(日本語) | 『Kotlin イン・アクション』『実践 Kotlin』『Android アプリ開発の教科書 Kotlin 対応』 |
| 書籍(英語) | "Kotlin in Action, 2nd ed."、"Atomic Kotlin"(Bruce Eckel) |
| 動画 | Android Developers YouTube、KotlinConf セッション |
| オンライン学習 | JetBrains Academy、Coursera "Kotlin for Java Developers" |

---

## 11. 落とし穴・バッドパターン

| # | 落とし穴 | 対策 |
|---|---|---|
| 1 | `launch` 濫用で structured concurrency が壊れる(リーク・孤児コルーチン) | CoroutineScope を明示、`viewModelScope` / `lifecycleScope` に紐付け |
| 2 | `!!`(non-null assertion)で null safety を潰す | `?.` / `?:` / `requireNotNull(message)` を使う |
| 3 | `lateinit var` の使いどころを誤る(初期化前アクセスで Exception) | コンストラクタで初期化できるなら `val` |
| 4 | Reflection(`kotlin-reflect`)濫用でパフォーマンス劣化・APK サイズ増 | 必要な箇所のみ使用、KSP で置換可能ならそちらに |
| 5 | `inline fun` の濫用(バイナリサイズ膨張) | reified 必要時・高階関数の呼び出しオーバーヘッドが気になる時のみ |
| 6 | `Flow.collect` を Main スレッドで重い処理付きで呼ぶ | `.flowOn(Dispatchers.IO)` 等で適切にオフロード |
| 7 | `data class` で mutable state(`var`)を持つ | immutable(`val`)で設計、更新は `copy()` |
| 8 | Compose で副作用を `@Composable` 関数内で直接実行 | `LaunchedEffect` / `DisposableEffect` / `rememberCoroutineScope` |
| 9 | KAPT 使い続けてビルド遅い | **KSP** 対応プロセッサに乗り換え |
| 10 | `@Serializable` だがコンパイラプラグイン有効化忘れ | `kotlinx-serialization` gradle plugin を追加 |
| 11 | sealed class 漏れで分岐が壊れる | `when` を **式として使う**(Kotlin が網羅性強制) |
| 12 | `runBlocking` を本番コードで使ってスレッド浪費 | テスト以外では使わない、`runCatching`/ `async`/`launch` を検討 |

---

## 12. 業務アプリでの適性評価

### 12.1 本件(Android 業務アプリ + jQuery/Java チーム)での評価

| 観点 | 評価 | コメント |
|---|---|---|
| 言語習熟難易度 | ★★★★★ | Java 経験者は 1〜2 週で書ける |
| 既存資産流用度 | ★★★★★ | Java ライブラリ完全相互運用 |
| Android 向け適合 | ★★★★★ | Google 公式推奨、新機能は Kotlin ファースト |
| HT ベンダ SDK | ★★★★★ | Java 前提の SDK を Kotlin から呼ぶだけ |
| 長期保守 | ★★★★★ | 後方互換強、コミュニティ健全 |
| 採用市場 | ★★★★☆ | Java 比では薄いが、Android エンジニア市場では主流 |
| **総合** | ★★★★★ | **本件では競合なし** |

### 12.2 業務アプリ一般での評価

| ドメイン | 推奨度 |
|---|---|
| Android モバイル | ★★★★★(Google 公式) |
| iOS 単独 | ★★☆☆☆(Swift が自然、KMP で部分共有は◎) |
| Spring サーバ | ★★★★★(Kotlin ファーストサポート) |
| 軽量サーバ(Ktor) | ★★★★★ |
| デスクトップ(Compose MP) | ★★★★☆(成熟中) |
| Web フロントエンド | ★★☆☆☆(TS/JS が自然) |
| CLI / スクリプト | ★★★☆☆(Node/Python の代替には弱い) |
| ゲーム | ★★☆☆☆(Unity C# が主流) |
| データサイエンス | ★★☆☆☆(Python が主流) |

---

## 13. 言語ロードマップ(H15)

公式ロードマップ: https://kotlinlang.org/docs/roadmap.html

| 中期テーマ(2026 以降も継続) | 内容 |
|---|---|
| K2 コンパイラ完全移行 | すべての IDE・ツールが K2 前提へ |
| **Kotlin/Wasm** Stable | ブラウザ ネイティブ速度 |
| **Compose Multiplatform** iOS/Web 成熟 | SwiftUI / React の代替候補 |
| Context Parameters | スコープ内で暗黙的な引数を渡す機能 |
| Value Classes 拡張 | マルチフィールド対応 |
| KSP 2 成熟 | KAPT 代替完了 |
| Kotlin Notebook | データサイエンス強化 |
| **Amper**(新ビルド定義)| Gradle DSL の代替可能な簡潔な YAML/TOML 的構成 |
| JVM 新機能追従 | Virtual Thread、Valhalla、Panama |

---

## 14. 参照リンク

### 14.1 公式

| 対象 | URL |
|---|---|
| Kotlin 公式 | https://kotlinlang.org/ |
| ドキュメント | https://kotlinlang.org/docs/home.html |
| リリース履歴 | https://kotlinlang.org/docs/releases.html |
| ロードマップ | https://kotlinlang.org/docs/roadmap.html |
| コーディング規約 | https://kotlinlang.org/docs/coding-conventions.html |
| Kotlin Foundation | https://kotlinfoundation.org/ |
| KEEP(言語進化プロセス)| https://github.com/Kotlin/KEEP |
| Kotlin Blog(JetBrains)| https://blog.jetbrains.com/kotlin/ |
| GitHub 組織 | https://github.com/JetBrains/kotlin |
| Try Kotlin(Playground)| https://play.kotlinlang.org/ |

### 14.2 標準・公式ライブラリ

| 対象 | URL |
|---|---|
| kotlinx.coroutines | https://github.com/Kotlin/kotlinx.coroutines |
| kotlinx.serialization | https://github.com/Kotlin/kotlinx.serialization |
| kotlinx.datetime | https://github.com/Kotlin/kotlinx-datetime |
| KSP | https://github.com/google/ksp |
| Compose Multiplatform | https://github.com/JetBrains/compose-multiplatform |
| Kotlin Multiplatform | https://kotlinlang.org/docs/multiplatform.html |
| Ktor | https://ktor.io/ |

### 14.3 Android 向け

| 対象 | URL |
|---|---|
| Android Kotlin Guide | https://developer.android.com/kotlin |
| Android Kotlin Style Guide | https://developer.android.com/kotlin/style-guide |
| Jetpack Compose | https://developer.android.com/jetpack/compose |
| Android Codelabs(Kotlin) | https://developer.android.com/courses |

### 14.4 品質・ツーリング

| 対象 | URL |
|---|---|
| ktlint | https://pinterest.github.io/ktlint/ |
| Detekt | https://detekt.dev/ |
| MockK | https://mockk.io/ |
| Kotest | https://kotest.io/ |

### 14.5 コミュニティ・学習

| 対象 | URL |
|---|---|
| KotlinConf(公式)| https://kotlinconf.com/ |
| Kotlin Fest(日本)| https://www.kotlinfest.com/ |
| JetBrains Academy | https://www.jetbrains.com/academy/ |
| Kotlin Koans | https://play.kotlinlang.org/koans/ |
| Kotlin Weekly(ニュースレター)| https://www.kotlinweekly.net/ |
| Stack Overflow Developer Survey | https://survey.stackoverflow.co/ |

### 14.6 脆弱性・セキュリティ

| 対象 | URL |
|---|---|
| Kotlin Security | security@kotlinlang.org |
| GitHub Security Advisories | https://github.com/advisories |
| OpenJDK Security | https://openjdk.org/groups/vulnerability/ |

---

## 15. 本ファイルの位置付け

- **言語単体** の詳細プロファイル(`profile_android_native.md` は FW プロファイル)
- 観点 A〜H(`selection_criteria.md`)を Kotlin 言語に投影した実例
- 本ファイルの粒度が妥当であれば、同構造で TypeScript / Dart / Java の詳細プロファイルも作成予定

---

## 更新履歴

- **2026-04-23**: 初版作成。H 節(言語特性深掘り観点、18 項目)を反映した雛形。
