# 詳細プロファイル: Kotlin Multiplatform (KMP) + Compose Multiplatform

`framework_profiles.md` の「Kotlin Multiplatform」「Compose Multiplatform」項目を **FW 単体の視点** から深掘りした詳細プロファイル。
本件(Android 業務アプリ / HT / jQuery+Java チーム / Android 永続)では参考扱いだが、将来 iOS 展開の可能性がある場合に重要となるため同粒度で整理する。

- 情報は **2026 年 4 月時点**

---

## 0. 結論サマリ

| 項目 | 評価 |
|---|---|
| 総合推奨度(本件、Android 永続前提) | ★★☆☆☆(iOS 展開なしではネイティブ Kotlin の劣化版になる) |
| 総合推奨度(iOS 展開あり) | ★★★★☆(既存 Android ネイティブチームの段階的 iOS 展開で最適) |
| 技術リスク | 低〜中(JetBrains 戦略、Compose MP iOS 成熟中) |
| 保守リスク | 低〜中(Kotlin に準ずる後方互換性) |
| 学習曲線形状 | **初手中(KMP 固有概念)→ 中盤スロープ(Kotlin/Native、iOS 連携)→ エキスパートは Compose MP まで** |
| 立ち上げ期間(Android Kotlin 経験者)| 2〜3 ヶ月(UI 共通化なし)、3〜5 ヶ月(Compose MP 採用) |
| Windows 開発(Android 側)| ◎ |
| iOS 対応 | **Mac 必須**、Xcode 併用 |
| AI/LLM 補完相性 | ◎(Kotlin と共通) |

---

## 1. 基本情報

### 1.1 開発元・ガバナンス・Bus Factor

| 項目 | 内容 |
|---|---|
| 開発元 | **JetBrains**(Kotlin と一体、Kotlin Team 内の Multiplatform グループ) |
| ガバナンス | **単一企業主導**(JetBrains) |
| Google との関係 | Kotlin 本体は共同後援、KMP は JetBrains 単独 |
| Compose Multiplatform | JetBrains |
| Bus Factor | **中〜高**(JetBrains の中核戦略、Kotlin コミュニティが健全) |

### 1.2 ライセンス

| 対象 | ライセンス | 商用制約 |
|---|---|---|
| Kotlin Multiplatform | Apache License 2.0 | なし |
| Compose Multiplatform | Apache License 2.0 | なし |
| kotlinx.\* ライブラリ | Apache License 2.0 | なし |
| 商用サポート | JetBrains ビジネス契約で相談可 | — |

### 1.3 バージョン履歴(主要マイルストーン)

| 年月 | バージョン | 主な変更 |
|---|---|---|
| 2017-11 | Kotlin 1.2 | 初期 Multiplatform API(実験的) |
| 2018-11 | Kotlin 1.3 | Kotlin/Native Alpha |
| 2019 | — | Multiplatform Mobile(KMM)ブランド誕生 |
| 2021-10 | Compose for Desktop 1.0 | Compose Multiplatform Desktop 安定化 |
| 2022-04 | Kotlin 1.6.20 | Multiplatform Beta |
| 2022 後半 | Kotlin/Native Memory Manager v2 | GC 改善、ARC 統合 |
| 2023-04 | Kotlin 1.8.20 | Compose Multiplatform iOS Alpha |
| **2023-11** | **Kotlin 1.9.20** | **Kotlin Multiplatform Stable 到達** |
| 2024-05 | Compose Multiplatform 1.6.10 | **iOS Stable 到達**(1.6.10) |
| 2024-11 | Kotlin 2.1 | K2 コンパイラ KMP 完全対応 |
| 2025 | — | Swift Export(Kotlin → Swift エクスポート)実験開始、Compose MP iOS 成熟 |
| **2026-04 時点** | Kotlin 2.x 系 | K/N 成熟、Compose MP iOS 本番採用拡大、Wasm Stable |

### 1.4 リリース・アップデートサイクル

| 対象 | 頻度 |
|---|---|
| Kotlin 本体(KMP 含む) | 6 ヶ月メジャー(Kotlin と同期) |
| Compose Multiplatform | Kotlin とほぼ同期、四半期 |
| kotlinx.\* ライブラリ | 随時 |

### 1.5 LTS・サポート期限・EOL(F5)

| 項目 | 内容 |
|---|---|
| 明示 LTS | **なし**(Kotlin 方針に準ずる) |
| 後方互換 | Kotlin 本体同様、強く維持 |
| Compose MP | Stable 到達後は下位互換重視 |

### 1.6 破壊的変更頻度・後方互換ポリシー(F6)

| 局面 | 変更影響度 |
|---|---|
| Kotlin 言語レベル | 低(Kotlin 本体と同じ) |
| KMP API(Gradle プラグイン等) | 中(Stable 到達後に大きな変更は減) |
| Compose Multiplatform(iOS) | 中(Stable 達成後も機能追加あり) |
| Kotlin/Native | 中(Memory Manager v2 以降は安定) |

### 1.7 脆弱性対応・CVE 体制(F7)

| 項目 | 内容 |
|---|---|
| 窓口 | JetBrains Security(`security@kotlinlang.org`) |
| 公開フロー | GitHub Security Advisories |
| 対応速度 | 迅速 |

### 1.8 商用サポート(F8)

| 手段 | 内容 |
|---|---|
| **JetBrains ビジネス契約** | IntelliJ IDEA Ultimate + 企業サポート |
| **JetBrains KMP 公式パートナー** | 企業導入支援 |
| 商用 SLA | 直接の KMP 単独 SLA は限定的 |

---

## 2. フレームワーク特性

### 2.1 アーキテクチャ

```
┌─────────────────────────────────────────────┐
│  Android 側 UI       │  iOS 側 UI            │
│  - Jetpack Compose   │  - SwiftUI            │
│  - または Compose MP │  - または Compose MP  │
├─────────────────────────────────────────────┤
│  common モジュール(Kotlin 共通コード)        │
│  - ビジネスロジック                          │
│  - ドメインモデル                            │
│  - ユースケース                              │
│  - リポジトリ                                │
├─────────────────────────────────────────────┤
│  ライブラリ(Kotlin Multiplatform 対応)     │
│  - Ktor Client / SQLDelight / Koin          │
│  - kotlinx.coroutines / serialization        │
├─────────────────────────────────────────────┤
│  ターゲット別 Source Set                     │
│  - androidMain / iosMain                    │
│  - jvmMain / jsMain / nativeMain            │
├─────────────────────────────────────────────┤
│  コンパイル先                                │
│  - Android: DEX(JVM)                        │
│  - iOS: Kotlin/Native(LLVM)                │
│  - Server: JVM                              │
│  - Desktop: JVM / Kotlin/Native             │
│  - Web: JS / Wasm                           │
└─────────────────────────────────────────────┘
```

### 2.2 expected / actual 宣言

KMP の中核概念。プラットフォーム固有の実装差異を吸収する。

```kotlin
// commonMain
expect class Platform() {
    val name: String
}

// androidMain
actual class Platform actual constructor() {
    actual val name = "Android ${android.os.Build.VERSION.RELEASE}"
}

// iosMain
actual class Platform actual constructor() {
    actual val name = "iOS ${UIDevice.currentDevice.systemVersion}"
}
```

### 2.3 Source Set 構成

| Source Set | 内容 |
|---|---|
| `commonMain` | 全プラットフォーム共通コード |
| `commonTest` | 共通テスト |
| `androidMain` | Android 固有実装 |
| `iosMain` / `iosArm64Main` / `iosSimulatorArm64Main` / `iosX64Main` | iOS 固有 |
| `jvmMain` | Kotlin/JVM(サーバ / Desktop) |
| `jsMain` / `wasmJsMain` | Web |
| `nativeMain` | Kotlin/Native 全般 |

### 2.4 言語(Kotlin)

- 本プロファイル全般は [`profile_kotlin.md`](./profile_kotlin.md) 参照
- 要点: Java 完全相互運用、null safety、coroutine、K2 コンパイラ

### 2.5 Compose Multiplatform

| ターゲット | 状態(2026-04) | 備考 |
|---|---|---|
| Android | ◎ Stable(Jetpack Compose そのもの) | Google 公式 |
| Desktop(Win/Mac/Linux) | ◎ Stable(2021 年〜) | |
| **iOS** | **◎ Stable(2024-05〜)** | SwiftUI 代替候補 |
| Web(Wasm) | β〜Stable(2026 前半見込み) | Compose HTML もあり |

### 2.6 並行・非同期モデル

| 機構 | 内容 |
|---|---|
| Kotlin Coroutines | common でも利用可(kotlinx.coroutines はマルチプラットフォーム) |
| Flow / StateFlow | 同上 |
| iOS 側 | coroutine を Swift から呼ぶ場合は `NativeCoroutines`(SKIE 等)で bridge |

### 2.7 メタプログラミング

| 技術 | 内容 |
|---|---|
| **KSP**(Kotlin Symbol Processing) | KMP 対応、共通のコード生成が可能 |
| kotlinx.serialization | KMP ネイティブ対応、JSON/Protobuf |
| SQLDelight | KMP 対応 SQL 生成 |
| Arrow(関数型)| KMP 対応 |

### 2.8 FFI・ネイティブ連携(H6)

| 手段 | 内容 |
|---|---|
| **cinterop** | Kotlin/Native で C ヘッダをラップ、iOS SDK を Kotlin から呼び出せる |
| **Objective-C/Swift interop** | Kotlin/Native で自動バインディング |
| **Swift Export**(実験) | Kotlin コードを Swift フレンドリな形でエクスポート |
| **SKIE**(Touchlab 製) | Kotlin の機能(coroutine、sealed class、default arg)を Swift から自然に呼ぶラッパ |
| Android(Java/Kotlin) | commonMain → androidMain の actual で直接利用 |

### 2.9 設計哲学

| 原則 | 意味 |
|---|---|
| **段階的共通化** | 全部共通化ではなく、必要な層だけ(ロジック→UI) |
| ネイティブ UX 維持 | UI はネイティブ(SwiftUI / Compose)を基本とし、共通化は選択的 |
| Kotlin らしさ | 型安全、coroutine、DSL などをそのまま iOS にも |
| プラットフォーム差の透過 | expected/actual で吸収、ビジネスロジックは純粋 |

---

## 3. 開発環境・ツーリング

### 3.1 CLI・SDK

| コマンド | 用途 |
|---|---|
| `./gradlew build` | 共通 + 各プラットフォームビルド |
| `./gradlew iosArm64MainKlibrary` 等 | iOS ターゲットビルド |
| `./gradlew embedAndSignAppleFrameworkForXcode` | iOS 向け Framework 作成 |
| Xcode | iOS UI・実機/シミュ実行 |
| Android Studio | Android 側実行、KMP 全体管理 |

### 3.2 IDE

| IDE | 評価 |
|---|---|
| **Android Studio + Kotlin Multiplatform plugin** | ◎ 主力 |
| **IntelliJ IDEA Ultimate** | ◎ Kotlin の本家、全機能 |
| **JetBrains Fleet** | ○ KMP 対応進化中、将来の本命候補 |
| Xcode | iOS UI のみ、KMP ロジックの表示は限定的 |

### 3.3 LSP・補完品質(H8)

- Kotlin 本体と同じく JetBrains 製で最高水準
- Fleet は LSP ベース、KMP 両ターゲットを統合表示

### 3.4 ビルド・依存管理

| 項目 | 内容 |
|---|---|
| ビルドシステム | **Gradle(Kotlin DSL)** |
| パッケージ | Maven Central + KMP 対応ライブラリ |
| KMP Gradle Plugin | `org.jetbrains.kotlin.multiplatform` |
| Compose MP Plugin | `org.jetbrains.compose` |

### 3.5 Lint / Formatter

- Kotlin と同じ: ktlint、Detekt、IntelliJ Inspection

### 3.6 デバッガ・プロファイラ

| ツール | 用途 |
|---|---|
| Android Studio Debugger | Android 側 + KMP common |
| Xcode Debugger | iOS 側(ネイティブ層、Kotlin/Native 生成物) |
| **Kotlin/Native Profiler** | Android Studio の KMP plugin 経由 |
| Instruments(Xcode) | iOS 側のメモリ/CPU 解析 |

### 3.7 Hot Reload

| 対象 | 機能 |
|---|---|
| Compose Android | Live Edit(実験的) / Apply Changes |
| **Compose Multiplatform**(Desktop) | Hot Reload 可(JetBrains Toolbox 統合) |
| Compose Multiplatform(iOS) | 限定的 |
| SwiftUI + KMP ロジック | Xcode Preview は Swift 部分のみ |

### 3.8 コンパイル速度(H13)

| 局面 | 体感 |
|---|---|
| Android 側インクリメンタル | 秒オーダー |
| Kotlin/Native 初回ビルド | **遅い**(数分)、LLVM コンパイルのため |
| Kotlin/Native インクリメンタル | 改善中だが Kotlin/JVM より遅い |
| iOS Framework ビルド | 数十秒〜数分 |

### 3.9 Windows 開発環境

| 項目 | 内容 |
|---|---|
| Android 開発 | **◎ 完全対応** |
| iOS 開発 | **Mac 必須**(Xcode 必要) |
| KMP common の開発 | Windows でも可(Android ターゲットで動作確認) |
| Compose MP Desktop | Windows 対応 |
| ディスク占有 | Android Studio + Xcode(Mac)+ KMP plugin = 50〜80 GB |

**業務アプリ観点**: Android のみなら Windows で完結、iOS 対応が必要な瞬間に Mac が必要になる。CI で MacinCloud / GitHub Actions macOS runner を使えば Mac 不要化も可能だが、iOS UI の開発・Preview には Mac が現実解。

---

## 4. パフォーマンス特性

### 4.1 起動時間

| 局面 | 体感 |
|---|---|
| Android 側 | ネイティブと同等(DEX 動作) |
| iOS 側(Kotlin/Native AOT) | ネイティブ Swift と同等 |
| Compose MP(iOS) | Stable 以降は実用的 |

### 4.2 スループット

| 指標 | 値 |
|---|---|
| ビジネスロジック(common) | Kotlin と同等 |
| Android UI | Jetpack Compose と同等 |
| iOS UI(Compose MP) | SwiftUI と同水準(Stable 以降) |
| iOS UI(SwiftUI + KMP logic) | ネイティブ同等 |

### 4.3 メモリフットプリント

| 項目 | 値 |
|---|---|
| Android 側 | ネイティブ同等 |
| iOS 側 | **Kotlin/Native ランタイム + Memory Manager v2** が追加される |
| アプリ全体 | ネイティブ比で若干増 |

### 4.4 アプリサイズ(H14)

| 項目 | サイズ |
|---|---|
| Android APK | ネイティブとほぼ同じ |
| iOS IPA | **Kotlin/Native ランタイム分増**(数 MB) |
| Compose MP iOS | ネイティブ比で数 MB 増 |

---

## 5. エコシステム・コミュニティ

### 5.1 パッケージ規模

| 対象 | 規模 |
|---|---|
| Maven Central の KMP 対応ライブラリ | 数百〜(急増中) |
| 公式 kotlinx.\* | 主要ライブラリ |
| サードパーティ KMP 対応 | Square、Touchlab、Arrow、Ktor、Koin 等 |

### 5.2 GitHub 定量指標(H11、2026-04 時点目安)

| リポジトリ | Stars(概算) |
|---|---|
| `JetBrains/compose-multiplatform` | 約 18k |
| `Kotlin/kotlinx.coroutines` | 約 15k |
| `Kotlin/kotlinx.serialization` | 約 6k |
| `InsertKoinIO/koin` | 約 10k |
| `cashapp/sqldelight` | 約 6k |

### 5.3 年次調査

| 調査 | 位置 |
|---|---|
| Stack Overflow Developer Survey | Kotlin が上位、KMP 単独の集計はまだ少 |
| JetBrains Developer Ecosystem Survey | **KMP 利用者急増中**(2023→2025で倍増報告) |
| KMPShip 統計 | KMP 採用率 12%→23%(2024→2025) |

### 5.4 日本語リソース(H12)

| 種別 | 代表 |
|---|---|
| 書籍(日本語) | 単独の書籍は少ない、Kotlin 書籍で部分的に扱う |
| カンファレンス | **DroidKaigi**(KMP セッション増加)、**Kotlin Fest** |
| 日本語ブログ | Cash App、Netflix の事例紹介記事が多い |
| 国内採用事例 | LINE、メルカリ(検証)、LayerX(検証)など増加傾向 |

### 5.5 認定・トレーニング

- **Touchlab**(KMP 専業、SKIE 提供):英語トレーニング
- JetBrains Academy
- 公式 Kotlin Multiplatform Hands-on
- Kotlin Conf 講演

---

## 6. 業務アプリ視点(HT 特化)

### 6.1 HT スキャナ連携

| 機能 | 実装 |
|---|---|
| Android 側 | **androidMain で Native と同じコード**(Zebra DataWedge Intent Broadcast 等) |
| iOS 側(業務 HT 用途) | iOS 版の HT は少数(Honeywell CT シリーズ等)、通常は外付けスキャナ |
| KMP common | スキャン結果を受け取るビジネスロジック(共通化可) |

**業務アプリ観点**: **HT は Android が主戦場**。KMP の iOS 共通化価値が直接効く場面は HT 単独では限定的。社内スマホ業務アプリ(iPhone 運用)との統合が必要な組織で価値。

### 6.2 MDM 連携

| 機能 | 代表実装 |
|---|---|
| App Config | Android 側の RestrictionsManager を androidMain で利用 |
| Device Owner / Kiosk | 同上、Android Native の API 直接利用 |

### 6.3 オフライン・同期

| 機能 | 代表ライブラリ |
|---|---|
| SQLite | **SQLDelight**(KMP 対応、型安全 SQL) |
| KVS | **multiplatform-settings** |
| バックグラウンドタスク | androidMain で WorkManager 直使用、iOS 側は BGTaskScheduler |
| 通信 | **Ktor Client**(KMP 対応、Dart Dio 相当) |
| JSON | **kotlinx.serialization** |
| 日時 | **kotlinx-datetime** |
| DI | **Koin**(KMP 対応)、Kodein |

### 6.4 セキュリティ

| 機能 | 代表実装 |
|---|---|
| Android Keystore | androidMain で直接利用 |
| iOS Keychain | iosMain で `CFDictionary` 呼出 |
| KMP Secure Storage | multiplatform-settings + 暗号化プラグイン |
| 生体認証 | 各プラットフォーム側で実装、common で共通 API 定義 |

### 6.5 カメラ・スキャン

| 機能 | 代表実装 |
|---|---|
| Android: CameraX / ML Kit | androidMain で利用 |
| iOS: AVFoundation / Vision | iosMain で利用 |
| 共通 API | common で `expect` 宣言、各プラットフォームで `actual` |

### 6.6 プッシュ通知

| 機能 | 代表実装 |
|---|---|
| FCM(Android) | androidMain |
| APNS(iOS) | iosMain |
| 共通 | トークン管理・リスナーを common |

---

## 7. 主要採用事例(H9)

### 7.1 グローバル

| 企業 | 用途 |
|---|---|
| **Netflix** | Android/iOS アプリのビジネスロジック共通化 |
| **Cash App**(Block) | Android/iOS/Desktop 統一、Square の金融アプリ |
| **McDonald's** | グローバルモバイル |
| **Philips** | ヘルステック |
| **Forbes** | メディアアプリ |
| **Careem**(Uber 中東) | 配車アプリ |
| **9GAG** | SNS |
| **VMware** | 社内ツール |
| **Touchlab** | KMP コンサル兼実装 |

### 7.2 国内

| 企業 | 用途 |
|---|---|
| LINE | 一部機能検証 |
| メルカリ | 検証段階 |
| LayerX、DeNA(ゲーム除く)、freee | 検証・PoC |

### 7.3 ドメイン別

| ドメイン | 採用実績 |
|---|---|
| **Fintech**(Cash App、Square) | ◎ 大規模採用 |
| **SaaS モバイル** | ○ |
| **ヘルステック**(Philips)| ○ |
| **メディア**(Forbes、9GAG)| ○ |
| **配車・フードデリ**(Careem、McDonald's)| ○ |
| **HT 業務** | △(実績少) |
| **金融基幹(国内)** | △(Java+Kotlin サーバ主、モバイル KMP は検証段階) |
| **ゲーム** | △(Unity/Unreal が主流) |

---

## 8. AI/LLM 補完相性(H10)

| 観点 | 評価 |
|---|---|
| 学習データ量 | ◎ Kotlin と共通、Android 学習資産が使える |
| 型情報活用 | ◎ Kotlin の強型付け |
| expected/actual 補完 | ○ 最新 IDE で補完可、AI もパターン認識 |
| Compose Multiplatform 補完 | ○(Jetpack Compose と共通パターン) |
| iOS 側の Swift / SwiftUI 補完 | ◎(Swift 本体の補完) |
| 全体 | ◎ Kotlin + Android + SwiftUI が AI で同時扱える |

---

## 9. 学習パス(H18)

### 9.1 学習曲線の形状

```
難易度
  │                 __________ エキスパート(6〜12ヶ月)
  │                /            Compose MP iOS、Swift Export、Kotlin/Native 最適化
  │           ____/
  │          /                   実務(3〜5ヶ月)
  │     ____/                    KMP セットアップ、expected/actual、iOS 連携
  │    /                         
  │   /                          基礎(2〜3ヶ月、Kotlin 経験者から)
  │  /                           Multiplatform 概念、Gradle KMP plugin
  │ /                            
  └──────────────────────→ 時間
```

**形状の特徴**:

- **初手中**(Multiplatform の概念、Source Set、Gradle 構成)
- **中盤スロープ**(Kotlin/Native、iOS Framework 生成、Xcode 連携)
- **エキスパートは Compose MP まで**(SwiftUI との違い、iOS UX 再現)

### 9.2 推奨学習パス(Android Kotlin 経験者から)

| 月 | テーマ |
|---|---|
| 1 | KMP 基本概念(common / platform-specific)、Gradle KMP plugin、簡易サンプル |
| 2 | expected/actual、ライフサイクル、iOS Framework 生成、Xcode 連携 |
| 3 | Ktor Client、SQLDelight、Koin、kotlinx.serialization、kotlinx-datetime |
| 4 | Swift/Kotlin bridge(SKIE、NativeCoroutines)、iOS メモリモデル |
| 5〜6 | Compose Multiplatform for iOS(UI 共通化)、または SwiftUI + KMP 連携 |

### 9.3 推奨教材

| カテゴリ | 教材 |
|---|---|
| 公式 | [Kotlin Multiplatform 公式](https://kotlinlang.org/docs/multiplatform.html)、[Compose MP 公式](https://www.jetbrains.com/lp/compose-multiplatform/) |
| Hands-on | https://kotlinlang.org/docs/multiplatform-mobile-getting-started.html |
| Touchlab | KMM Introduction、SKIE ドキュメント |
| 動画 | KotlinConf、JetBrains ウェビナー |
| 書籍 | 『Kotlin Multiplatform by Tutorials』(raywenderlich / Kodeco) |

---

## 10. 落とし穴・バッドパターン

| # | 落とし穴 | 対策 |
|---|---|---|
| 1 | Android 永続前提で KMP を採用 | 素直にネイティブ Kotlin 採用 |
| 2 | iOS UI も Compose MP で完全共通化しようとする | 複雑な iOS UX は **SwiftUI ネイティブ + KMP ロジック共通**が無難 |
| 3 | common モジュールでプラットフォーム API を呼ぼうとする | expected/actual で各プラットフォームに寄せる |
| 4 | Kotlin/Native ビルドが遅くイライラ | インクリメンタル活用、**必要な iOS ターゲットのみビルド**(simulatorArm64 中心) |
| 5 | coroutine を Swift から扱いづらい | **SKIE** や `NativeCoroutines` を使う |
| 6 | Kotlin の default argument が Swift から使えない | SKIE で自動生成 |
| 7 | iOS 側で Kotlin の sealed class が使いづらい | SKIE で enum ラップ |
| 8 | Gradle 構成が複雑化 | Gradle Convention Plugin、Version Catalog |
| 9 | ライブラリが KMP 非対応で詰む | Ktor / SQLDelight / Koin の定番セット内で戦う、**必要なら common から expected/actual で抽象化** |
| 10 | メモリリーク(Kotlin/Native Memory Manager)| Memory Manager v2 採用確認、循環参照注意 |
| 11 | Compose MP の iOS バグに当たる | 最新版維持、Issue 追跡、ネイティブ SwiftUI に切替可能な設計 |
| 12 | iOS チームと Android チームの温度差 | 共通設計のレビュー体制、Android 側から iOS 視点も議論 |

---

## 11. 業務アプリでの適性評価

### 11.1 本件(Android HT + 20 画面 + Android 永続)

| 観点 | 評価 | コメント |
|---|---|---|
| 言語習熟難易度 | ★★★★☆ | Kotlin 経験ある前提 |
| 既存 Android 資産流用 | ★★★★★ | androidMain でそのまま利用 |
| iOS 共通化の利点 | ★☆☆☆☆ | **Android 永続で価値ゼロ** |
| Gradle / ビルド複雑化 | ★★★☆☆ | 余分な KMP 構成が乗る |
| HT スキャナ連携 | ★★★★★ | Android 側の扱いはネイティブと同じ |
| 国内人材 | ★★☆☆☆ | KMP 経験者少なめ |
| 長期保守 | ★★★★☆ | Kotlin 後方互換に準ずる |
| **総合** | ★★☆☆☆ | **Android 永続前提では不採用、iOS 展開の可能性があれば ★★★★☆** |

### 11.2 業務アプリ一般

| シナリオ | 推奨度 |
|---|---|
| Android 永続 | ★★☆☆☆(ネイティブ Kotlin の劣化) |
| Android 既存 + iOS 新規展開 | ★★★★★(KMP の本領) |
| Android/iOS 並行新規開発(中〜大規模)| ★★★★☆ |
| 金融・ヘルステック(同一ロジック両 OS) | ★★★★★ |
| 社内業務 + 共通サーバ(Ktor)| ★★★★★ |
| HT 単独・Android 永続 | ★★☆☆☆ |
| ゲーム | ★★☆☆☆(Unity 主流) |
| MVP / 短期 | ★★☆☆☆(立ち上げ重いため不向き) |

---

## 12. ロードマップ(H15)

| 中期テーマ | 内容 |
|---|---|
| **Compose Multiplatform iOS 成熟** | SwiftUI 代替へ本格前進 |
| **Swift Export** | Kotlin コードを Swift から自然に使える |
| **Kotlin/Wasm** Stable | Web ターゲットでも KMP ロジック共通化 |
| K2 コンパイラ完全対応 | ビルド高速化 |
| Kotlin/Native Memory Manager 継続改善 | GC / ARC 統合の最適化 |
| Fleet IDE | KMP 専用 IDE として成熟 |
| AI 統合 | JetBrains AI Assistant で KMP コード補完 |

公式: https://kotlinlang.org/docs/roadmap.html

---

## 13. 参照リンク

### 13.1 公式

| 対象 | URL |
|---|---|
| Kotlin Multiplatform 公式 | https://kotlinlang.org/docs/multiplatform.html |
| Compose Multiplatform 公式 | https://www.jetbrains.com/lp/compose-multiplatform/ |
| KMP Hands-on | https://kotlinlang.org/docs/multiplatform-mobile-getting-started.html |
| Kotlin Multiplatform Tutorials | https://kotlinlang.org/docs/multiplatform-tutorials.html |
| KMP Stable アナウンス | https://blog.jetbrains.com/kotlin/2023/11/kotlin-multiplatform-stable/ |
| Compose MP iOS Stable アナウンス | https://blog.jetbrains.com/kotlin/2024/05/compose-multiplatform-for-ios-is-in-beta/ |
| Kotlin Roadmap | https://kotlinlang.org/docs/roadmap.html |

### 13.2 主要ライブラリ

| 対象 | URL |
|---|---|
| Ktor(Client/Server)| https://ktor.io/ |
| SQLDelight | https://cashapp.github.io/sqldelight/ |
| Koin | https://insert-koin.io/ |
| kotlinx.coroutines | https://github.com/Kotlin/kotlinx.coroutines |
| kotlinx.serialization | https://github.com/Kotlin/kotlinx.serialization |
| kotlinx-datetime | https://github.com/Kotlin/kotlinx-datetime |
| multiplatform-settings | https://github.com/russhwolf/multiplatform-settings |
| SKIE(Swift/Kotlin interop)| https://skie.touchlab.co/ |

### 13.3 業務・HT 向け

- Android 側の HT 連携は [`profile_android_native.md`](./profile_android_native.md) を参照
- iOS 側は標準的な AVFoundation、BT、NFC 等の API を iosMain で利用

### 13.4 開発環境・ツール

| 対象 | URL |
|---|---|
| Android Studio + KMP plugin | https://plugins.jetbrains.com/plugin/14936-kotlin-multiplatform-mobile |
| JetBrains Fleet | https://www.jetbrains.com/fleet/ |
| Touchlab KMP Quickstart | https://github.com/touchlab/KaMPKit |

### 13.5 コミュニティ・学習

| 対象 | URL |
|---|---|
| KotlinConf | https://kotlinconf.com/ |
| Kotlin Fest(日本) | https://www.kotlinfest.com/ |
| Touchlab ブログ | https://touchlab.co/blog |
| Kotlin Multiplatform Slack | https://kotlinlang.slack.com/(招待制、公式より取得) |
| KaMPKit(テンプレート) | https://github.com/touchlab/KaMPKit |
| KMPShip(統計) | https://www.kmpship.app/blog |

### 13.6 セキュリティ

| 対象 | URL |
|---|---|
| Kotlin Security | security@kotlinlang.org |
| GitHub Security Advisories | https://github.com/JetBrains/kotlin/security/advisories |

---

## 14. 本ファイルの位置付け

- FW 詳細プロファイル(同粒度シリーズの 4 本目)
- 本件 Android 永続前提では **不採用推奨**、ただし将来 iOS 展開の可能性がある案件で再評価の価値あり
- 言語単体の詳細は [`profile_kotlin.md`](./profile_kotlin.md) 参照、本ファイルは KMP 特有の側面のみ

---

## 更新履歴

- **2026-04-23**: 初版作成。FW 詳細プロファイル(Kotlin Multiplatform + Compose Multiplatform)。
