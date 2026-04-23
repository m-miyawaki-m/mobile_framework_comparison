# 詳細プロファイル: ネイティブ Android(Kotlin + Jetpack Compose)

本ドキュメントは `framework_profiles.md` の「Android ネイティブ」項目の深掘り版。
業務アプリ・HT(ハンディターミナル)・Windows 開発チーム・jQuery+Java 経験者 という本件前提に沿って実務面を詳細に整理する。

- 情報は **2026年4月時点**。バージョンや EOL 日付は採用時点で公式再確認
- 本ファイルは「どこまで掘るべきか」の雛形。他 FW についても同レベルまで深掘りするか判断材料として使用

---

## 0. 結論サマリ

| 項目 | 評価 |
|---|---|
| **本件での総合推奨度** | ★★★★★(Android 永続+Java チーム+HT で競合なし) |
| **技術リスク** | 最小(Google 公式、AOSP、各社 HT SDK が前提とする言語) |
| **保守的リスク** | 最小(LTS 相当の長期サポート、Target SDK 毎年引き上げへの公式追従) |
| **学習コスト** | 言語 = 極小(Java→Kotlin 自動変換)/ UI(Compose) = 中 |
| **立ち上げ期間目安** | 2〜3ヶ月(Kotlin 2 週 + Android 基礎 2 週 + Compose 4 週 + 業務固有統合 4 週) |
| **Windows 開発** | 完全対応、Mac 不要 |

---

## 1. 基本情報(詳細)

### 1.1 開発元・ガバナンス

| 項目 | 内容 |
|---|---|
| 開発元 | Google(Android Open Source Project, AOSP) |
| OS 側 | Google Android チーム |
| Jetpack/AndroidX | Google + AndroidX コントリビュータコミュニティ |
| 言語(Kotlin) | JetBrains(+ Google が後援、Kotlin Foundation 管理) |
| ガバナンス | Google 主導 + オープンソース公開(AOSP は Apache 2.0) |
| Bus Factor | **高**(Android 全世界シェア 70%+、Google の戦略基盤) |

### 1.2 ライセンス詳細

| 対象 | ライセンス | 備考 |
|---|---|---|
| AOSP(OS本体) | Apache License 2.0 | 商用利用・改変・再配布可 |
| AndroidX / Jetpack | Apache License 2.0 | 同上 |
| Jetpack Compose | Apache License 2.0 | 同上 |
| Kotlin | Apache License 2.0 | JetBrains |
| Android Studio | Apache License 2.0(ベースの IntelliJ IDEA Community) | 商用サポートは JetBrains 契約で IntelliJ IDEA Ultimate |
| Gradle | Apache License 2.0 | |
| Google Play Services | **プロプライエタリ**(Google 独自ライブラリ、Google からの配布) | GMS 搭載端末のみ、非搭載HT では利用不可 |
| ML Kit / Firebase SDK | Firebase/GMS 配下、Google 利用規約 | オフライン ML Kit 等は端末内で完結 |

**注意**: 業務用 HT の中には GMS 非搭載機種(中国系、Zebra の一部)もあり、その場合は Firebase/GMS 依存を避ける設計が必要。

### 1.3 バージョン履歴(主要マイルストーン)

| 年 | バージョン | コードネーム | 主な変更 |
|---|---|---|---|
| 2016 | Android 7.0 / API 24 | Nougat | Multi-Window |
| 2017 | Android 8.0〜8.1 / API 26〜27 | Oreo | Background 制限、Notification Channel |
| 2018 | Android 9 / API 28 | Pie | Google Play が Target SDK 最低を毎年引き上げ開始 |
| 2019 | Android 10 / API 29 | Q | Scoped Storage |
| 2020 | Android 11 / API 30 | R | Package Visibility、Scoped Storage 強制化 |
| 2021 | Android 12 / API 31 | S | Material You、Splash Screen API、Jetpack Compose 1.0 GA |
| 2022 | Android 13 / API 33 | T | 通知権限の Opt-in 化 |
| 2023 | Android 14 / API 34 | U | Foreground Service の型宣言必須 |
| 2024 | Android 15 / API 35 | V | 2025-08 以降 新規アプリは Target 35 必須 |
| 2025 | Android 16 / API 36 | — | Material 3 Expressive、予測型バック標準 |
| 2026(予定) | Android 17 / API 37 | — | Google Play Target は 36 に引き上げ予定(毎年1年遅れ) |

### 1.4 リリースサイクル

| 対象 | 頻度 |
|---|---|
| Android OS メジャー | 年 1(8 月末〜9 月) |
| Android Security Bulletin | **月次**(毎月 1 日) |
| AndroidX(Jetpack) | ライブラリ別、**月次〜四半期** |
| Jetpack Compose BOM | 月次(BOM で依存バージョンを一元管理) |
| Android Studio | 年 4 リリーストレイン(Canary → Beta → RC → Stable、通常数ヶ月ごと) |
| Android Gradle Plugin(AGP) | Android Studio と同期(年 3〜4 メジャー) |
| Kotlin | 6ヶ月メジャー(2.0、2.1、2.2…) |
| Gradle | 年 2〜3 メジャー |

### 1.5 LTS・サポート期限

| 対象 | ポリシー |
|---|---|
| Android OS | 各メジャー **5 年のセキュリティ更新**(端末ベンダー側の対応が前提)。Pixel は 7 年(Pixel 8 以降) |
| AndroidX | 明示 LTS なし、実際は年単位で後方互換維持 |
| Compose | 明示 LTS なし、BOM 単位で整合性保証 |
| Kotlin | 明示 LTS なし、事実上の後方互換維持 |
| **Google Play Target SDK 要件** | **毎年 8 月末、前年の API レベルを最低要件に引き上げ**。2025-08 時点で API 35(Android 15)必須、新規/更新アプリ対象 |

**業務アプリ運用上のポイント**: 端末寿命 5〜7 年に対し、Google Play 配信アプリは毎年 Target SDK 引き上げ必須。MDM 経由で社内配布する場合は Play 要件を回避できるが、セキュリティ観点で無視はできない。

---

## 2. 言語・ランタイム

### 2.1 Kotlin 言語

| 項目 | 内容 |
|---|---|
| 推奨バージョン | Kotlin 2.1 系(K2 コンパイラ既定) |
| Java 相互運用 | 完全。Kotlin から Java ライブラリそのまま使用、逆も可 |
| 自動変換 | Android Studio の「Java to Kotlin Converter」で既存 Java ファイルを Kotlin に変換 |
| 主要機能 | null safety、data class、extension function、scope functions(let/apply/run/with/also)、coroutine、Flow、sealed class、inline/value class |

### 2.2 ランタイム(ART + AOT)

| 項目 | 内容 |
|---|---|
| ランタイム | **ART(Android Runtime)**。Dalvik の後継 |
| コンパイル方式 | **AOT + JIT ハイブリッド**。初回起動後バックグラウンドで AOT 最適化 |
| 実行形式 | DEX(Dalvik Executable)。APK/AAB 内に格納 |
| 64bit 対応 | Android 10 以降は 64bit 必須(Google Play)。HT は端末により 32bit 実機あり(ARMv7/armeabi-v7a) |

### 2.3 Android SDK / API レベル戦略

| 設定 | 業務アプリ(HT 想定) |
|---|---|
| compileSdk | **最新安定**(Android 16 / API 36、2026-04 時点) |
| targetSdk | **最新 or 1 世代前**(コンプライアンスと Play 要件のバランス) |
| minSdk | **API 24(Android 7.0)推奨**。HT 現役機種カバー。API 21(Android 5.0)まで下げる案件もあり |
| buildToolsVersion | AGP と同期(通常は AGP が自動選択) |

### 2.4 主要 Jetpack ライブラリ(業務アプリ定番)

| カテゴリ | ライブラリ | 用途 |
|---|---|---|
| UI | **Jetpack Compose** | 宣言的 UI、Material 3 |
| Navigation | **Navigation Compose** | 画面遷移 |
| State | **ViewModel**、**StateFlow**(Kotlin) | 画面ライフサイクル対応状態保持 |
| DI | **Hilt** | Dagger の上位ラッパ、Spring 経験者に馴染む |
| DB | **Room** | SQLite ORM、型安全クエリ |
| 永続化(軽量) | **DataStore** | SharedPreferences 後継 |
| 非同期 | **Kotlin Coroutines + Flow** | `suspend fun`、構造化並行性 |
| バックグラウンド | **WorkManager** | オフラインキュー・定期同期・制約付き実行 |
| 通信 | **Retrofit + OkHttp** または **Ktor Client** | REST、gRPC、クライアント証明書 |
| シリアライズ | **kotlinx.serialization** | Kotlin ネイティブ、JSON/Protobuf |
| 認証 | **Biometric API** | 指紋・顔認証 |
| セキュリティ | **Android Keystore**、**EncryptedSharedPreferences** | キー管理・暗号化 |
| テスト | AndroidX Test、Espresso、Compose Test、Robolectric | 各種テスト |

---

## 3. 開発環境(詳細)

### 3.1 Android Studio

| 項目 | 内容 |
|---|---|
| 現行安定版(2026-04) | Android Studio Meerkat/Narwhal 系(年次コードネーム) |
| ベース IDE | IntelliJ IDEA Community(JetBrains) |
| ライセンス | Apache License 2.0 |
| 商用 IDE 代替 | IntelliJ IDEA Ultimate(有償、Android プラグイン+Web/DB 等統合) |
| 更新チャネル | Canary / Dev / Beta / RC / Stable |
| 自動更新 | IDE 内で自動検出、バージョン併用可 |

### 3.2 JDK 要件

| 対象 | JDK |
|---|---|
| Android Studio 2023.x 以降 | **JDK 17 が同梱**(JetBrains Runtime) |
| ビルド用 | JDK 17 推奨、一部案件では JDK 21 にも移行 |
| チーム共有 | SDKMAN!(Mac/Linux) / Azure JDK / Temurin / Amazon Corretto のいずれか |
| Windows | Temurin または Microsoft Build of OpenJDK が配布面で無難 |

### 3.3 ビルドシステム(Gradle + AGP)

| 項目 | 内容 |
|---|---|
| ビルドツール | **Gradle 8.x** |
| ビルドプラグイン | **Android Gradle Plugin(AGP)8.x** |
| DSL | Kotlin DSL(推奨)または Groovy DSL |
| マルチモジュール | `settings.gradle.kts` で `:app`、`:feature:*`、`:data`、`:core` 等を分割 |
| ビルドバリアント | `debug` / `release` / 拠点別(例: `production` / `staging` / `dev`)、ABI 別 APK Split |
| シグネチャ | `signingConfigs` に keystore 指定、CI 用は `gradle.properties` で管理 |
| ProGuard/R8 | R8(AGP 標準)、`proguard-rules.pro` |
| ビルドキャッシュ | Gradle Build Cache、Remote Cache(Gradle Enterprise) |
| 依存バージョン集中管理 | **Version Catalog**(`libs.versions.toml`) 推奨 |

### 3.4 依存管理(Maven 座標・BOM)

```kotlin
// settings.gradle.kts
dependencyResolutionManagement {
    repositories {
        google()
        mavenCentral()
    }
}

// app/build.gradle.kts 例
dependencies {
    val composeBom = platform("androidx.compose:compose-bom:2026.02.00")
    implementation(composeBom)
    implementation("androidx.compose.material3:material3")
    implementation("androidx.activity:activity-compose:1.10.0")
    implementation("androidx.lifecycle:lifecycle-viewmodel-compose:2.9.0")
    implementation("com.google.dagger:hilt-android:2.52")
    implementation("androidx.work:work-runtime-ktx:2.10.0")
    implementation("com.squareup.retrofit2:retrofit:2.11.0")
    implementation("androidx.room:room-runtime:2.7.0")
    kapt("androidx.room:room-compiler:2.7.0")
}
```

**BOM(Bill of Materials)**: Compose BOM を使うと整合性確保が楽。Firebase にも BOM あり。

### 3.5 対応開発 OS・ハードウェア

| OS | 可否 | コメント |
|---|---|---|
| Windows 10 / 11(x64 / arm64) | ◎ | 業務アプリ開発の主要環境 |
| macOS(Intel / Apple Silicon) | ◎ | Apple Silicon ネイティブ対応 |
| Linux(Ubuntu/Fedora 等 x64) | ◎ | |
| Chromebook(Linux コンテナ)| △ | 制約多い、非推奨 |

**推奨マシンスペック**:

- CPU: 8 コア以上(ビルド並列化に効く)
- RAM: **32 GB 以上推奨**(16 GB は Compose + Emulator で不足感)
- SSD: 500 GB(SDK + Emulator + プロジェクト + キャッシュ)
- GPU: エミュレータ用に HAXM/AMD-V 有効、NVIDIA/Apple M シリーズは Emulator 加速に有利

### 3.6 Windows 開発環境セットアップ手順(概略)

1. **Android Studio** インストール([公式](https://developer.android.com/studio))
2. SDK Manager で必要 API レベル(例: API 24 / 33 / 35 / 36)をインストール
3. AVD Manager でエミュレータ作成(Pixel 系推奨)
4. **JDK 17** は Android Studio 同梱の JBR をそのまま使用(追加インストール不要)
5. Windows で Hyper-V / Windows Hypervisor Platform 有効化(Android Emulator 高速化)
6. Git for Windows、GitHub CLI
7. Gradle 専用のユーザー `gradle.properties` 設定(ヒープ、並列化、ビルドキャッシュ)
8. Windows Defender の除外パスに `~/.gradle`、`~/.android`、プロジェクトフォルダを追加(ビルド速度向上)

**HT 実機開発**: USB ドライバ(機種別、Zebra は公式 USB ドライバ配布)、ADB over Wi-Fi(5.0 以降)、Vysor 等の画面共有ツールが便利。

### 3.7 エミュレータ・実機デバッグ

| 項目 | ツール |
|---|---|
| 公式エミュレータ | **Android Emulator**(QEMU ベース、GPU 加速) |
| ARM エミュレーション | x86_64 ホスト上で ARM イメージ実行可(遅い、実機推奨) |
| HT 固有動作 | 実機必須。DataWedge 等のスキャナ SDK はエミュレータで再現不可 |
| デバッグ | **Android Studio Debugger**、Logcat、Layout Inspector、App Inspection(Database/Network/BackgroundTask) |
| 実機接続 | USB ADB、ADB over Wi-Fi、Wireless Debugging(Android 11+)、pair by QR |
| デバイス Farm | **Firebase Test Lab**(Google)、AWS Device Farm、BrowserStack App Live |

### 3.8 プロファイラ・Inspection

| ツール | 対象 |
|---|---|
| **Android Studio Profiler** | CPU、Memory、Network、Energy(Power Rail、Android 13+) |
| Layout Inspector | Compose ツリー検査、リコンポジション計測 |
| App Inspection | Database Inspector(Room)、Network Inspector、Background Task Inspector |
| Baseline Profile / Macrobenchmark | 起動時間・スクロール性能計測、AOT 最適化 |
| Perfetto | システム全体トレース、低レベル解析 |
| LeakCanary(OSS) | メモリリーク検出、デファクト標準 |

---

## 4. 開発体験

### 4.1 コード→動作までのフィードバックループ

| 機能 | 速度 | 状態保持 | 制約 |
|---|---|---|---|
| **Live Edit**(Compose、実験的) | 速(秒) | △ | @Composable 関数本体の編集に限定 |
| **Apply Changes**(コード) | 中(数秒〜10秒) | ○ | クラス追加・削除等は不可 |
| **Apply Changes**(リソース) | 速 | ○ | リソースのみ変更時 |
| **Run**(通常実行) | 遅(30 秒〜数分) | × | クリーンな状態 |
| **Install Run**(差分インストール) | 中 | × | |

Flutter/RN の Hot Reload には届かない。**本件影響**: 画面数 20 なら体感差は中程度、小さな UI 調整が多い初期フェーズで生産性に効く。

### 4.2 Lint・Formatter・静的解析

| ツール | 役割 |
|---|---|
| **Android Lint** | Android 固有チェック(パフォーマンス、互換性、セキュリティ、アクセシビリティ) |
| **ktlint** | Kotlin スタイル、`ktlint --format` で自動整形 |
| **Detekt** | Kotlin 静的解析、複雑度・依存方向・禁止パターン |
| **Android Studio Inspection** | IDE 内の即時解析 |
| **SonarQube / SonarCloud** | 大規模プロジェクトでの品質ダッシュボード |
| **OWASP Dependency-Check** | 依存ライブラリの CVE 検出 |

### 4.3 デバッグワークフロー(業務アプリでよく使う)

- ブレークポイント + Evaluate Expression
- **Log Timber**(OSS、Logcat 拡張)で構造化ログ
- Crashlytics(Firebase)クラッシュ収集 — GMS 非搭載 HT なら **Sentry** / **自社サーバ送信** 代替
- **WorkManager Diagnostics**(`adb shell dumpsys jobscheduler`)
- **DataWedge Profile Export**: 実機で設定→Export して他機に配布

---

## 5. テスト戦略

### 5.1 Unit Test(JUnit + MockK)

| 対象 | ツール |
|---|---|
| テストランナー | JUnit 4 / 5 |
| モック | **MockK**(Kotlin 用、Mockito より Kotlin 親和) |
| アサーション | kotlin-test、AssertJ、Truth |
| Coroutine テスト | `kotlinx-coroutines-test`、`runTest` |
| 実行環境 | JVM 上(高速、ローカルユニット) |

### 5.2 Instrumented Test(端末/エミュレータ実行)

| 対象 | ツール |
|---|---|
| ランナー | **AndroidX Test Runner** |
| Espresso | View 系 UI テスト |
| **Compose Test** | Compose UI テスト(`createComposeRule()`) |
| Hilt Test | DI 差し替え |

### 5.3 E2E / UI 自動化

| ツール | 特徴 |
|---|---|
| **Maestro** | YAML ベース、クロスプラットフォーム、学習コスト低 |
| Appium | 歴史あり、言語問わない |
| UI Automator | Android 公式、端末間操作 |
| Robot Pattern | Kotlin DSL で E2E 構造化 |

### 5.4 デバイス Farm

| サービス | 特徴 |
|---|---|
| **Firebase Test Lab** | Google 公式、Robo Test で自動探索 |
| **AWS Device Farm** | 多機種、HT 相当の古い Android もあり |
| BrowserStack App Live | 手動テスト向き |

### 5.5 Code Coverage

- Android Gradle Plugin の JaCoCo 統合
- `gradle testDebugUnitTest jacocoTestReport`
- 80% 以上を業務ロジック層(ViewModel/UseCase/Repository)に目標設定が現実的

---

## 6. CI/CD・配信

### 6.1 CI/CD 候補

| サービス | 特徴 |
|---|---|
| **GitHub Actions** | Windows/Linux/macOS runner、OSS なら無償枠大きい |
| **Bitrise** | モバイル特化、事前設定されたステップ豊富 |
| **Jenkins(Gradle Enterprise 連携)** | オンプレ運用、社内ポリシー厳しい現場向け |
| Codemagic | Flutter 特化だが Android 対応あり |
| Azure DevOps Pipelines | .NET 系資産組織向け |

### 6.2 署名・バージョニング

| 項目 | 内容 |
|---|---|
| キーストア | `*.jks` または `*.keystore`、社内に**1つだけ**(紛失不可) |
| パスワード管理 | GitHub Secrets、Azure Key Vault、HashiCorp Vault 経由で CI 注入 |
| Play App Signing(Play 配信時) | Google がアップロードキーと異なる配布キーを管理、キー紛失リスク軽減 |
| versionCode | 整数、単調増加(例: `YYMMDDNN`) |
| versionName | `1.2.3` セマンティックバージョン |

### 6.3 成果物フォーマット

| 形式 | 用途 |
|---|---|
| **AAB(Android App Bundle)** | Play Store 配信必須(2021 年 8 月以降、新規アプリ) |
| **APK** | MDM 配信・サイドロード・HT 配布、Play 経由以外 |
| APK Split(ABI/Density) | サイズ削減 |
| Play Asset Delivery | 大容量アセット(主にゲーム) |

### 6.4 MDM 配信(HT 業務アプリ向け)

| MDM | 配信フロー |
|---|---|
| **SOTI MobiControl** | APK アップロード → プロファイル作成 → 配信 |
| **VMware Workspace ONE** | 旧 AirWatch、App Config 動的注入対応 |
| **Microsoft Intune** | Android Enterprise 統合 |
| **Zebra StageNow** | Zebra 端末一括構成、バーコードキッティング |
| **Google Android Enterprise Zero-Touch** | 端末初期化時に自動構成・アプリ配布 |

**App Config(Managed Configuration)**: `<app-restrictions>` を Manifest 宣言、MDM からキー・バリューで注入可能。社内 API エンドポイント等を端末別に切り替える用途。

### 6.5 Play Store 配信(参考)

| 機能 | 説明 |
|---|---|
| Internal Testing | 100 人以内の社内テスター、即時配信 |
| Closed Testing | メール/Google Group 単位 |
| Open Testing | 誰でも参加 URL |
| Staged Rollout | % 指定で段階配信(障害時に停止可) |
| In-App Update | `AppUpdateManager` で強制/推奨更新 |

### 6.6 OTA(Over-The-Air Update)

ネイティブ Android には **公式の OTA 機構は存在しない**(Play の Staged Rollout や MDM 配信がその代替)。業務アプリで OTA が必要な場合:

- In-App Update(Play 依存)
- MDM Silent Install
- 自社サーバから APK をダウンロード → `ACTION_INSTALL_PACKAGE`(Android 8 以降、`REQUEST_INSTALL_PACKAGES` 権限要、MDM/Device Owner で無音インストール)

**対比**: Flutter/RN(CodePush/EAS Update)のような JS バンドル差替え相当の機能は原理的にない(AOT コンパイル済み DEX のため)。

---

## 7. 業務アプリ視点(HT 特化)

### 7.1 HT(ハンディターミナル)連携

#### 7.1.1 Zebra DataWedge

| モード | 実装 |
|---|---|
| **Keyboard Wedge(Keystroke Output)** | スキャン結果を **キー入力イベント** として EditText に届ける。実装コード量 0、最短 |
| **Intent Output** | 任意の `Intent` を Broadcast、`BroadcastReceiver` で受信。詳細制御・MultiBarcode・プロファイル切替対応 |

**サンプル(Intent Output 受信)**:

```kotlin
class DataWedgeReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context, intent: Intent) {
        val data = intent.getStringExtra("com.symbol.datawedge.data_string")
        val labelType = intent.getStringExtra("com.symbol.datawedge.label_type")
        // viewModel へ流し込み
    }
}
```

`AndroidManifest.xml` に `<receiver>` 登録 + カスタムアクション名(プロファイルに合わせる)を指定。

#### 7.1.2 Zebra EMDK(Enterprise Mobility Development Kit)

DataWedge より低レベル。スキャナハードを直接制御、DataWedge では不可能な使い方が必要な場合のみ採用。

- 依存: EMDK for Android(Gradle には Google/Zebra の Maven リポジトリ追加)
- API: `EMDKManager`、`BarcodeManager`、`ScannerConfig`
- 代替: DataWedge Intent API で大半の要件は満たせる

#### 7.1.3 Honeywell / Datalogic

| ベンダー | SDK |
|---|---|
| **Honeywell** | Honeywell Mobility SDK(Android、Java/Kotlin) |
| **Datalogic** | Datalogic SDK for Android(Java/Kotlin) |
| **Point Mobile** | PM Scanner SDK |
| **Urovo / CipherLab 等** | 独自 SDK、Keyboard Wedge + 独自 Intent の組合せが多い |

いずれも Java/Kotlin 前提の公式 SDK。**ネイティブ選択が運用・ドキュメント面で圧倒的に楽**。

#### 7.1.4 Keyboard Wedge vs Intent API の使い分け

| 要件 | 推奨モード |
|---|---|
| スキャン = 単純なテキスト入力 | Keyboard Wedge(実装不要) |
| 画面にフォーカスがない状態で受信 | **Intent API** |
| プロファイル切替(画面別) | **Intent API**(DataWedge プロファイル API) |
| MultiBarcode | **Intent API** |
| 画面ボタンからのスキャン起動(SOFT_SCAN_TRIGGER) | **Intent API** |
| スキャン結果のメタ情報(シンボル種別等) | **Intent API** |

### 7.2 MDM 連携

#### 7.2.1 App Config(Managed Configuration)

`res/xml/app_restrictions.xml` に宣言:

```xml
<restrictions xmlns:android="http://schemas.android.com/apk/res/android">
    <restriction
        android:key="api_base_url"
        android:title="API Base URL"
        android:restrictionType="string" />
</restrictions>
```

アプリ側:

```kotlin
val rm = context.getSystemService(Context.RESTRICTIONS_SERVICE) as RestrictionsManager
val bundle = rm.applicationRestrictions
val apiUrl = bundle.getString("api_base_url")
```

MDM 側(SOTI / Intune / Workspace ONE 等)からキー・バリューで注入可能。**ネイティブは公式 API で確実、他 FW はプラグイン経由**。

#### 7.2.2 Device Owner / Kiosk / Lock Task

| モード | 手段 |
|---|---|
| **Device Owner** 化 | `dpm set-device-owner` コマンド(初期セットアップ時のみ、または NFC/QR プロビジョニング) |
| **Lock Task Mode** | `startLockTask()`、Home/Back/Recent を封じる |
| **Kiosk Launcher** | カスタムランチャーアプリを `HOME` インテントで登録 |
| **Managed Profile** | 仕事用プロファイル分離(端末が個人兼用時) |

### 7.3 オフライン・同期

#### 7.3.1 Room + SQLite

- Room = AndroidX の公式 SQLite ORM、**型安全な SQL クエリ**
- Migration: スキーマバージョン管理 `Migration(1, 2)`
- テスト: `Room.inMemoryDatabaseBuilder()` + JUnit で単体テスト可

#### 7.3.2 WorkManager

業務アプリの **オフラインキュー・自動再送** の定番。

- 制約: ネットワーク・充電中・バッテリー残量等
- `PeriodicWorkRequest` で定期同期
- **Doze モード・App Standby でも確実に実行される**(JobScheduler 統合)
- HT の 8 時間連続稼働でもバッテリー制御と両立

#### 7.3.3 DataStore

SharedPreferences 後継。Preferences DataStore(Key-Value)と Proto DataStore(型付き)の 2 種。Coroutine と Flow で非同期アクセス。

### 7.4 通信

#### 7.4.1 HTTP クライアント

| ライブラリ | 特徴 |
|---|---|
| **Retrofit + OkHttp** | デファクト標準、インターフェース宣言で RESTfull |
| **Ktor Client** | Kotlin ネイティブ、KMP でも使える |
| OkHttp 単体 | 低レベル制御、Retrofit の内部 |
| HttpURLConnection | 標準 API、非推奨 |

#### 7.4.2 クライアント証明書認証 / VPN / 社内ネットワーク

- **KeyChain API** でユーザー/端末証明書取得
- **OkHttp の SSLSocketFactory / X509TrustManager** で証明書ピン留め
- VPN: Android VPN Service、OpenVPN / Cisco AnyConnect 等のクライアントアプリ経由
- MDM からの Wi-Fi/VPN プロファイル一括配信

#### 7.4.3 gRPC

- `grpc-okhttp`、`grpc-protobuf-lite`、`grpc-kotlin-stub`
- Protobuf で IDL 定義、型安全なクライアントコード自動生成
- 業務基幹系で REST の上位代替候補

### 7.5 セキュリティ

| 機能 | API |
|---|---|
| ハードウェアキーストア | **Android Keystore**(TEE / StrongBox バックアップ) |
| 暗号化 Preferences | **EncryptedSharedPreferences**(Jetpack Security) |
| 暗号化ファイル | **EncryptedFile** |
| 生体認証 | **Biometric API**(指紋 / 顔)+ CryptoObject 連携 |
| ネットワークセキュリティ構成 | `res/xml/network_security_config.xml` で証明書ピン留め、クリアテキスト禁止 |
| Root 検出 | **Play Integrity API**(Play 依存、非 GMS 端末は別手段) |

### 7.6 パフォーマンス・バッテリー

| 観点 | 対策 |
|---|---|
| 起動時間 | **Baseline Profile** で初回起動を AOT、**Startup Tracing** で計測 |
| UI 性能 | Compose のリコンポジション最小化、`remember`、`derivedStateOf`、Lazy* で可視領域のみ描画 |
| メモリ | LeakCanary で開発中検出、Profiler で実機計測 |
| バッテリー | Doze / App Standby を尊重、WakeLock は最小限、Foreground Service は型宣言(Android 14+) |
| APK サイズ | R8 最適化、Dynamic Feature Module、画像の WebP 化 |

---

## 8. Java/jQuery チームの学習パス(詳細 12 週ロードマップ)

| 週 | テーマ | 具体内容 | 成果物 |
|---|---|---|---|
| 1 | Kotlin 基礎(Java 差分) | null safety、data class、extension function、scope functions | サンプルコードで Java→Kotlin 変換演習 |
| 2 | Kotlin 応用 | Coroutines、Flow、sealed class、inline/value class、delegation | 簡単な CLI ツール作成 |
| 3 | Android 基礎 1 | Activity/Fragment/Intent、マニフェスト、権限、リソース、Gradle | Hello World アプリ |
| 4 | Android 基礎 2 | ライフサイクル、Broadcast Receiver、Service、Notification | Log 収集アプリ |
| 5 | Jetpack Compose 基礎 | 基本コンポーザブル、State、Modifier、Theme、Material 3 | 静的な画面 3 枚 |
| 6 | Compose 応用 | LazyColumn/Row、Navigation Compose、アニメーション、複雑 State | マスタ一覧 + 詳細画面 |
| 7 | アーキテクチャ | ViewModel、StateFlow、Hilt(DI)、Clean Architecture 簡易版 | MVVM サンプル |
| 8 | データ層 | Retrofit、Room、Repository パターン、Coroutines 結合 | API 連携 + ローカルキャッシュ |
| 9 | バックグラウンド | WorkManager(Constraints、Periodic、Expedited) | オフラインキュー + 自動再送 |
| 10 | 業務固有統合 1 | **DataWedge**(Keyboard Wedge + Intent)、AndroidManifest 登録 | スキャン画面 |
| 11 | 業務固有統合 2 | **MDM App Config**、Kiosk、クライアント証明書、Biometric | 設定注入 + 認証画面 |
| 12 | テスト + CI/CD | JUnit+MockK、Compose Test、GitHub Actions、署名 | CI パイプライン構築 |

### 8.1 推奨教材・書籍(2026 年時点)

| 分類 | 教材 |
|---|---|
| 公式 | [Android Developer Guides](https://developer.android.com/guide)、[Android Codelabs](https://developer.android.com/courses)、[Kotlin Docs](https://kotlinlang.org/docs/home.html) |
| 学習コース | Google 公式「Android Basics with Compose」 |
| 書籍(Kotlin) | 『Kotlin in Action』『実践 Kotlin』 |
| 書籍(Android) | 『Android アプリ開発の教科書』(技術評論社系)、『Android Compose Design Patterns』(Manning) |
| 日本語 Blog | DroidKaigi Conference、Yahoo Japan Tech Blog、DeNA Engineers' Blog、メルカリ Engineering |
| 動画 | Google I/O sessions、Android Developers YouTube Channel |

### 8.2 初期 PoC 推奨スコープ

業務アプリの本格着手前に、以下を 2〜3 週間の PoC で実施推奨:

1. **DataWedge Intent 受信 + Compose 画面反映** — HT 固有要件の最大リスクを先に潰す
2. **Room + WorkManager + Retrofit** — オフラインキュー + 自動再送の最小実装
3. **MDM App Config 注入** — 運用フェーズまで見通した確認
4. **社内 API 証明書認証** — 通信層の疎通確認

これで **「HT 業務アプリの主要リスクが技術的に通ること」** を証明してから本開発に入る。

---

## 9. 既知の落とし穴・運用上の注意

| # | 落とし穴 | 対策 |
|---|---|---|
| 1 | Gradle ビルド時間が数分〜10 分 | マルチモジュール化、Gradle Build Cache、Version Catalog、`--configuration-cache`、JVM ヒープ拡大、Windows Defender 除外、SSD 必須 |
| 2 | **Target SDK 毎年引き上げ**で動かなくなる API | 毎年の Android Developer Preview(2〜6 月)で事前検証、Firebase Test Lab で回帰テスト |
| 3 | Compose BOM を使わず個別バージョン指定 → 整合性崩壊 | BOM で一元管理 |
| 4 | 依存ライブラリの CVE 放置 | Dependabot + OWASP Dependency-Check、CI で定期スキャン |
| 5 | R8 最適化で Reflection を使うライブラリが壊れる | `proguard-rules.pro` で `-keep` 追加、GSON/Retrofit/Room はテンプレ keep ルール公開あり |
| 6 | 64bit 必須 Android と 32bit HT の混在 | ABI Split + `ndk.abiFilters` で制御 |
| 7 | GMS 非搭載 HT で Firebase/Play Service 依存が動かない | オフライン ML Kit、Sentry、独自クラッシュ収集 |
| 8 | HT 端末別 DataWedge プロファイル差異 | プロファイル JSON を `assets` に入れ、起動時に Intent で流し込む運用 |
| 9 | Android Studio の Kotlin/AGP バージョン不整合でビルド崩壊 | Kotlin / AGP / Compose Compiler Plugin の互換性マトリクスを公式で確認(Kotlin 2.0+ は Compose Compiler 別モジュール化) |
| 10 | Baseline Profile 未設定で初回起動体感遅 | AGP の Baseline Profile Gradle Plugin で CI 生成 |

---

## 10. コスト見積もり(参考、2026 年 Windows + Android 業務アプリ想定)

### 10.1 学習・立ち上げコスト

| 項目 | 人月 |
|---|---|
| Kotlin + Android 基礎学習(2 名)| 1.0 人月 × 2 = 2.0 人月 |
| Compose 習熟 | 0.5 人月 × 2 = 1.0 人月 |
| HT 固有統合 PoC | 0.5 人月 |
| 業務アプリ 20 画面実装(本開発) | 4〜6 人月 |
| CI/CD 整備 | 0.5 人月 |

### 10.2 ツール・ライセンス費用

| 項目 | 費用 |
|---|---|
| Android Studio | **無償** |
| Android SDK / Gradle / Kotlin | **無償** |
| IntelliJ IDEA Ultimate(任意) | 約 $169/年(個人)、$649/年(企業) |
| Play Console(配信する場合) | **$25 一度きり** |
| Firebase / Crashlytics(GMS 搭載端末時) | 無償枠大 |
| Bitrise / GitHub Actions | 無償枠 + 使用量課金 |
| MDM(既存ライセンスに乗る前提) | 既契約に依存 |
| Zebra / Honeywell SDK | **無償**(公式配布) |

### 10.3 運用コスト(年次)

| 項目 | 費用・工数 |
|---|---|
| Target SDK 引き上げ追従 | 年 0.5〜1 人月 |
| セキュリティパッチ・依存更新 | 月 0.1〜0.2 人月 |
| 新機種 HT 対応 | 機種別 0.2〜0.5 人月 |
| Compose / AndroidX メジャー更新 | 年 0.5 人月 |

---

## 11. 参照リンク(拡充版)

### 11.1 公式ドキュメント

| 対象 | URL |
|---|---|
| Android Developers | https://developer.android.com/ |
| Kotlin | https://kotlinlang.org/ |
| Jetpack Compose | https://developer.android.com/jetpack/compose |
| AndroidX リリース | https://developer.android.com/jetpack/androidx/versions |
| Compose BOM バージョン | https://developer.android.com/jetpack/compose/bom |
| Android Studio | https://developer.android.com/studio |
| Android Gradle Plugin リリースノート | https://developer.android.com/build/releases/gradle-plugin |
| Kotlin / AGP / Compose 互換性 | https://developer.android.com/jetpack/androidx/releases/compose-compiler |
| Android Security Bulletin | https://source.android.com/docs/security/bulletin |
| Play Console Target API 要件 | https://support.google.com/googleplay/android-developer/answer/11926878 |

### 11.2 Jetpack ライブラリ

| 対象 | URL |
|---|---|
| Hilt(DI) | https://developer.android.com/training/dependency-injection/hilt-android |
| Room(DB) | https://developer.android.com/training/data-storage/room |
| WorkManager | https://developer.android.com/topic/libraries/architecture/workmanager |
| DataStore | https://developer.android.com/topic/libraries/architecture/datastore |
| Navigation Compose | https://developer.android.com/jetpack/compose/navigation |
| Biometric | https://developer.android.com/training/sign-in/biometric-auth |
| Jetpack Security(EncryptedSharedPreferences 等)| https://developer.android.com/topic/security/data |
| Baseline Profile | https://developer.android.com/topic/performance/baselineprofiles/overview |

### 11.3 HT・業務用

| 対象 | URL |
|---|---|
| Zebra DataWedge 公式 | https://techdocs.zebra.com/datawedge/latest/guide/about/ |
| Zebra DataWedge Intent API | https://techdocs.zebra.com/datawedge/latest/guide/api/ |
| Zebra EMDK for Android | https://techdocs.zebra.com/emdk-for-android/latest/guide/about/ |
| Honeywell Developer(SDK 入手) | https://developer.honeywell.com/ |
| Datalogic Developer Portal | https://developer.datalogic.com/ |
| Android Enterprise | https://www.android.com/enterprise/ |
| Android Management API | https://developers.google.com/android/management |
| Managed Configurations | https://developer.android.com/work/managed-configurations |

### 11.4 テスト・CI

| 対象 | URL |
|---|---|
| AndroidX Test | https://developer.android.com/training/testing |
| Compose Testing | https://developer.android.com/jetpack/compose/testing |
| Firebase Test Lab | https://firebase.google.com/docs/test-lab |
| Maestro | https://maestro.mobile.dev/ |
| Bitrise | https://devcenter.bitrise.io/en/steps-and-workflows/steps-for-android.html |
| GitHub Actions Android | https://github.com/android-actions/setup-android |

### 11.5 コード品質・セキュリティ

| 対象 | URL |
|---|---|
| ktlint | https://pinterest.github.io/ktlint/ |
| Detekt | https://detekt.dev/ |
| OWASP Dependency-Check | https://owasp.org/www-project-dependency-check/ |
| LeakCanary | https://square.github.io/leakcanary/ |
| Timber | https://github.com/JakeWharton/timber |

---

## 12. 本ファイルの位置付け

- 本ファイルは `framework_profiles.md` の 1 エントリを **深掘りした詳細版**
- 他 FW(Flutter / RN+Expo / Ionic+Capacitor / KMP / .NET MAUI 等)についても同レベルの詳細プロファイルを作成するかは、本ファイルの深さを見て **必要・不要・中間の三択** で判断
- 業務アプリの選定判断時は、本詳細版 + `selection_criteria.md` の A〜G 観点 + `android_business_app_framework_comparison.md` の総合比較 の 3 点セットで評価

---

## 更新履歴

- **2026-04-23**: 初版作成。`framework_profiles.md` の Android ネイティブ項目を深掘りした詳細プロファイル。
