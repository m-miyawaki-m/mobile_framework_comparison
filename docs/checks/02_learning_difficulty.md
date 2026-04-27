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
