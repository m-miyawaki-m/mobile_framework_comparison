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
