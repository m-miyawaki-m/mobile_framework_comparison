# 詳細プロファイル: Flutter

`framework_profiles.md` の「Flutter」項目を **FW 単体の視点** から深掘りした詳細プロファイル。
`selection_criteria.md` の観点 A〜H を Flutter に投影し、本件前提(Android 業務アプリ / HT / jQuery+Java チーム)での評価を付記する。

- 情報は **2026 年 4 月時点**。バージョン・EOL は公式で採用時点再確認必須

---

## 0. 結論サマリ

| 項目 | 評価 |
|---|---|
| 総合推奨度(本件) | ★★★★☆(Android 永続なら ★★★★★ のネイティブ Kotlin に対する強い対抗馬) |
| 技術リスク | 低〜中(Google が単独後援、LTS 明示なし) |
| 保守リスク | 中(6ヶ月毎の追従コスト、パッケージ更新負荷) |
| 学習曲線形状 | **初手やや急(Widget Tree)→ 中盤スムーズ → エキスパート 6〜12ヶ月** |
| 立ち上げ期間(Java経験者) | Dart 基礎 2〜3 週、実務 2〜3 ヶ月 |
| 業務アプリ適性 | ◎(特に UI 一貫性・高負荷 UI・iOS 併用案件) |
| HT 業務アプリ | ◎(Zebra 公式 DataWedge Flutter デモあり) |
| Windows 開発 | ◎ Android は完全対応、iOS ビルドは Mac 必須 |
| AI/LLM 補完相性 | ◎(型情報豊富、Widget DSL 明確) |

---

## 1. 基本情報

### 1.1 開発元・ガバナンス・Bus Factor

| 項目 | 内容 |
|---|---|
| 開発元 | **Google**(Flutter Team、Dart Team と一体) |
| 発案 | 2015(Sky プロジェクト) |
| ガバナンス | **単一企業(Google)主導**、GitHub でオープン開発 |
| 言語 | **Dart**(Google 製、Flutter と一体設計) |
| エンジン | C++(`flutter/engine` リポジトリ、レンダリング + Dart VM 統合) |
| Bus Factor | **中**(Google 戦略依存、Google 内で複数回見直し報道あり) |
| コミュニティ参画 | ◎(GitHub で Issue/PR 活発、外部コミッタ多数) |

**本件での影響**: Google 戦略依存リスクは長期保守で考慮要。ただし 2026 年時点で採用企業数は増加継続、エコシステムは健全に拡大中。

### 1.2 ライセンス

| 対象 | ライセンス |
|---|---|
| Flutter SDK | **BSD-3-Clause** |
| Dart SDK | BSD-3-Clause |
| Flutter Engine | BSD-3-Clause(内部に Skia/Impeller/ICU など含む、各々ライセンス分離) |
| pub.dev パッケージ | 各作者依存(大半は BSD / MIT / Apache 2.0) |
| 商用制約 | なし |
| 商用サポート | **Google 直接の公式商用サポートなし**、Flutter Partner プログラム経由 |

### 1.3 バージョン履歴(主要マイルストーン)

| 年月 | バージョン | 主な変更 |
|---|---|---|
| 2017-05 | Beta 1 | Google I/O で発表 |
| 2018-12 | **1.0** | Stable 到達 |
| 2019 | 1.7 | AOT コンパイル改善 |
| 2020-03 | 1.12 | iOS 13 対応強化、Add-to-App 成熟 |
| 2021-03 | **2.0** | **Null safety 言語標準化**、Web Stable、Desktop Beta |
| 2022-05 | **3.0** | **Desktop Stable(Win/Mac/Linux)**、Material 3 対応開始 |
| 2023-01 | 3.7 | **Impeller iOS デフォルト化**(Skia 代替) |
| 2023-08 | 3.13 | Impeller Android Preview |
| 2024-02 | 3.19 | AI アシスト統合、Impeller Android 改善 |
| 2024-05 | 3.22 | WebAssembly Preview、Cupertino 大幅改善 |
| 2024-11 | 3.27 | **Impeller Android Stable** |
| 2025 前半 | 3.2x 系 | Impeller Vulkan 安定化、ホットリスタート最適化 |
| **2026-04 時点** | 3.3x 系(推定) | K2 相当の成熟、WebAssembly 本格、AI 統合深化 |

### 1.4 リリース・アップデートサイクル

| 対象 | 頻度 |
|---|---|
| Stable チャネル | **四半期ごと**(2〜3 ヶ月) |
| Beta チャネル | 月次 |
| Master / Dev チャネル | 随時(開発版) |
| Hotfix / patch | 随時(セキュリティ・クリティカルバグ) |
| Dart SDK | Flutter と同期 |

**業務アプリ観点**: 6ヶ月サイクルの Kotlin/Angular より **追従頻度が高い**。年 4 回のリリース追従を運用計画に織り込む必要。

### 1.5 LTS・サポート期限・EOL(F5)

| 項目 | 内容 |
|---|---|
| 明示 LTS | **なし**(Google 方針) |
| 古い Stable のサポート | 通常 **3〜6 ヶ月程度**、その後は非推奨 |
| EOL 通知 | Flutter リリースノート内で告知 |
| 採用指針 | 原則 **最新 2〜3 Stable 以内**で運用 |
| Dart | Flutter と同期 |

**業務アプリ観点**: 長期保守 5〜7 年では、少なくとも **年 4 回の追従アップデート**を運用プロセスに組み込む必要あり。これは .NET MAUI / Angular(18 ヶ月 LTS)比で負担大。

### 1.6 破壊的変更頻度・後方互換ポリシー(F6)

| 局面 | 変更影響度 |
|---|---|
| Dart 言語レベル | 低(Null safety 導入時は大改変、以降は穏やか) |
| Flutter Widget API | **中**(非推奨→削除のサイクルがやや短い印象) |
| Material 2 → Material 3 | **大**(UI 層の書き直しが発生した事例あり) |
| Navigator 1 → Navigator 2 / GoRouter | 中(ルーティング移行は手動コード修正要) |
| Impeller 切替 | 設計面は透過だが特殊描画コードの再検証要 |

**Migration Guide**: 公式が各バージョンで提供、`flutter analyze` で API 差分検出可。

### 1.7 脆弱性対応・CVE 体制(F7)

| 項目 | 内容 |
|---|---|
| 窓口 | `security@flutter.dev`(Google セキュリティチーム) |
| 公開フロー | GitHub Security Advisories + リリースノート |
| 対応速度 | 迅速(Google バックアップ) |
| 脆弱性履歴 | SDK 本体の重大 CVE は限定的、サードパーティパッケージが多い |
| サードパーティ脆弱性 | pub.dev パッケージは **作者依存**、Dependabot で補完必要 |

### 1.8 商用サポート(F8)

| 手段 | 内容 |
|---|---|
| **Google 直接の SLA 契約** | なし(2026 時点) |
| Flutter Partner | Flutter 専業コンサル企業(Codemagic、Very Good Ventures、nubank系 等) |
| JetBrains 商用 IDE | IntelliJ IDEA Ultimate(Flutter サポート含む) |
| Firebase 商用契約 | 間接的に関連(Crashlytics、Remote Config 等) |

---

## 2. フレームワーク特性

### 2.1 アーキテクチャ

```
┌────────────────────────────────────┐
│  Flutter Framework(Dart)           │
│  - Material / Cupertino Widgets    │
│  - Rendering Tree                  │
│  - Animation                       │
├────────────────────────────────────┤
│  Flutter Engine(C++)                │
│  - Skia / Impeller(レンダリング)    │
│  - Dart VM / AOT ランタイム         │
│  - Platform Channels               │
├────────────────────────────────────┤
│  Embedder(プラットフォーム別)       │
│  - Android: JNI + Surface          │
│  - iOS: Objective-C/Swift + Metal  │
│  - Windows / macOS / Linux         │
└────────────────────────────────────┘
```

| レイヤ | 言語 | 役割 |
|---|---|---|
| Framework | Dart | Widget、レイアウト、アニメーション、ルーティング |
| Engine | C++ | レンダリング、Dart VM、プラットフォーム抽象化 |
| Embedder | プラットフォーム別 | OS 連携(サーフェス取得、入力イベント) |

### 2.2 UI 描画方式(A3 / 性能)

| エンジン | 特徴 | 状況(2026-04) |
|---|---|---|
| **Skia**(旧) | Google 製 2D グラフィックス、Chrome 等と共通 | 非デフォルト(レガシー互換用) |
| **Impeller** | Flutter 専用、**プリコンパイル済みシェーダ**、ジャンクの根絶 | **iOS/Android 両方で Stable** |
| バックエンド | Metal(iOS)、Vulkan(Android 新)、OpenGL(fallback) | Vulkan が Android デフォルトに移行中 |

**ピクセル単位の自前描画**: OS 純正 UI を使わず、Flutter が画面全体をフレームごとに独自描画する。これにより **全プラットフォームでピクセルまで一致した UI** を実現。トレードオフは OS ネイティブ感の低下。

### 2.3 Widget ツリー

| 概念 | 内容 |
|---|---|
| **Widget = 全て** | UI の最小単位、レイアウト・装飾・状態すべてが Widget |
| 不変 | Widget は immutable、差分計算で変化を反映 |
| 3 層ツリー | Widget(不変宣言)→ Element(inflation)→ RenderObject(実描画) |
| State | StatefulWidget に紐づく可変状態 |
| BuildContext | Widget ツリー内の位置情報(祖先参照に使用) |

**パラダイム**: 宣言的 UI(`UI = f(state)`)。React / SwiftUI / Jetpack Compose と同系統。

### 2.4 言語(Dart)

| 特性 | 内容 |
|---|---|
| 静的型付け | ◎ 型推論強い |
| **null safety** | Dart 3 で強制化(2023) |
| コンパイル | **AOT(本番)/ JIT(開発)** 両対応 |
| 並行 | Future / async-await、Stream、Isolate(プロセス相当の並行) |
| GC | 世代別 GC(Dart VM 内蔵) |
| クラス | class / mixin / extension |
| パターンマッチ | `switch` 式(Dart 3+)、record、パターン分解 |
| 構文親和性 | Java/JS 経験者に親しみやすい |

### 2.5 並行・非同期モデル

| 機構 | 用途 |
|---|---|
| `Future<T>` + `async` / `await` | 単発の非同期処理 |
| `Stream<T>` | 連続イベント(UI イベント、データ変更) |
| **`Isolate`** | **メモリ共有しない並行実行単位**(プロセスに近い、Dart の特徴) |
| `compute()` | ワンショットで Isolate を起動する簡易 API |

**Kotlin coroutine との違い**: Dart Isolate は **メモリ共有しない**。スレッド間のデータ競合が原理的に発生しない一方で、Isolate 間でのオブジェクト渡しはシリアライズコストあり。

### 2.6 エラーハンドリング哲学

| 機構 | 使い所 |
|---|---|
| `try/catch` / `throw` | 主流 |
| **検査例外なし** | Dart は Kotlin と同様、検査例外を廃止 |
| `Result<T>`(サードパーティ) | 関数型スタイル |
| sealed class(Dart 3+) | パターンマッチで網羅 |
| `FlutterError.onError` | グローバルエラーフック |

### 2.7 メタプログラミング・コード生成(H5)

| 技術 | 内容 |
|---|---|
| `build_runner` + `source_gen` | Dart の **コード生成基盤**(Java の APT 相当) |
| `json_serializable` / `freezed` | JSON マッピング、immutable data class 自動生成の定番 |
| Reflection | **Flutter では実質禁止**(`dart:mirrors` は AOT で動かない) |
| Codegen 依存度 | 業務アプリでは高い(`freezed` / `riverpod_generator` / `retrofit_generator` 等) |

**業務アプリ観点**: `dart:mirrors` が使えない分、コード生成で補う設計。`build_runner` の実行時間は案件によっては長くなるので CI 配慮要。

### 2.8 FFI・ネイティブ連携(H6)

| 連携手段 | 内容 |
|---|---|
| **Platform Channels**(`MethodChannel` / `EventChannel`) | Dart↔ネイティブの非同期メッセージング、Flutter の基本 |
| **Pigeon** | **型安全な Platform Channel コード生成**(公式) |
| **`dart:ffi`** | C 言語ライブラリを直接呼出(リアルタイム性能要件で使用) |
| プラットフォーム例外 | `PlatformException` で受取 |

**業務アプリ観点**: HT の DataWedge は **Intent Broadcast + MethodChannel** で受信。Pigeon でコード生成すれば Kotlin/Swift/Dart 三者で型整合。

### 2.9 設計哲学

| 原則 | 意味 |
|---|---|
| **Everything is a Widget** | UI・レイアウト・装飾・アニメーション全てを Widget で表現 |
| ピクセル単位の制御 | OS テーマに依存せず意図した見た目を保証 |
| 宣言的 | UI = f(state)、状態変更で UI が自動再構築 |
| 開発者体験重視 | Hot Reload、DevTools、エラーメッセージがすべて一級 |
| パフォーマンス | AOT + Impeller で 120FPS 対応 |

---

## 3. 開発環境・ツーリング

### 3.1 CLI・SDK

| コマンド | 用途 |
|---|---|
| `flutter create` | プロジェクト生成 |
| `flutter pub get` | 依存解決 |
| `flutter run` | 開発実行(Hot Reload 付) |
| `flutter build apk/appbundle/ios/web/windows` | ビルド |
| `flutter test` | テスト |
| `flutter analyze` | 静的解析 |
| `flutter doctor` | 環境診断(必須) |
| `dart format` | Formatter |
| `dart fix` | 自動修正 |

### 3.2 IDE

| IDE | 評価 | 備考 |
|---|---|---|
| **VS Code + Flutter extension** | ◎ 公式、最も人気 | 軽量、Dart/Flutter DevTools 統合 |
| **Android Studio + Flutter plugin** | ◎ 公式 | Android ビルド・デバッグ込 |
| IntelliJ IDEA Ultimate | ◎ | Flutter プラグイン同等 |
| JetBrains Fleet | ○ | 実装成熟中 |

### 3.3 LSP・補完品質(H8)

- **Dart Analysis Server**(公式 LSP)
- 補完・Navigate to definition・リネーム・Extract Widget・Wrap with… 強力
- **Dart DevTools** がブラウザ上で統合デバッグ・プロファイルを提供

### 3.4 ビルド・依存管理

| 項目 | 内容 |
|---|---|
| ビルドシステム | **`flutter build`** が内部で Gradle(Android)/ Xcode(iOS)/ CMake(Desktop) を叩く |
| パッケージマネージャ | **`pub`**(Dart 公式) |
| レジストリ | **pub.dev**(JetBrains なし、Google 運営) |
| バージョン指定 | `pubspec.yaml` + `pubspec.lock` |
| Version Catalog 的機能 | なし(pubspec に直接記述) |
| SDK バージョン管理 | **`fvm`**(Flutter Version Management) / asdf |

### 3.5 Lint / Formatter

| ツール | 役割 |
|---|---|
| **`flutter_lints`** | 公式推奨 Lint ルール |
| **`very_good_analysis`** | Very Good Ventures 製、より厳格 |
| `dart analyze` | 静的解析 |
| `dart format` | 公式 Formatter |
| custom_lint | サードパーティで Lint ルール追加 |

### 3.6 デバッガ・プロファイラ

| ツール | 用途 |
|---|---|
| **Flutter DevTools**(ブラウザ統合、公式) | Performance / Memory / Network / Widget Inspector / CPU Sampler / Logger |
| IDE Debugger(VS Code / Android Studio) | ブレークポイント、Hot Reload 統合 |
| Observatory | Dart VM レベルの低レベル解析(DevTools 内) |
| Firebase Performance Monitoring | リモート実機パフォーマンス |
| Sentry | クラッシュ + パフォーマンス |

**スタックトレース可読性(H7)**: Dart スタックトレースは Java/Kotlin と同程度に読みやすい。`StackTrace.current` / `Error` の扱いも標準的。

### 3.7 Hot Reload / Hot Restart

| 機能 | 内容 | 所要時間 |
|---|---|---|
| **Hot Reload** | Widget ツリー + Dart コードの差分注入、**状態保持** | **< 1 秒(最速)** |
| Hot Restart | アプリ再起動、状態リセット | 数秒 |
| Full Run | クリーン再ビルド | 数十秒〜数分 |

**業界で最高水準の開発フィードバックループ**。React Native / Jetpack Compose の Hot Reload / Fast Refresh より速度・安定性で上回る。

### 3.8 コンパイル速度(H13)

| 局面 | 体感 |
|---|---|
| インクリメンタル(Hot Reload) | < 1 秒 |
| Hot Restart | 数秒 |
| フルビルド(Android APK) | 数十秒〜数分 |
| フルビルド(iOS、Mac 前提) | 数十秒〜数分 |
| `build_runner` 実行(codegen 多用時) | **数十秒〜数分**(案件による) |

### 3.9 Windows 開発環境

| 項目 | 内容 |
|---|---|
| Flutter SDK | Windows 対応 ◎ |
| Android ビルド | 完全対応 |
| iOS ビルド | **Mac 必須**(または Codemagic / GitHub Actions macOS runner) |
| Windows Desktop ビルド | **Windows 必須**(クロスコンパイル不可) |
| ディスク占有 | Flutter SDK + Android SDK ≈ 20〜40 GB、+ Xcode で 50〜80 GB |

---

## 4. パフォーマンス特性

### 4.1 起動時間

| 局面 | 体感 |
|---|---|
| 初回起動(cold start) | AOT のため高速、Java/Kotlin より若干重い場合あり |
| Warm start | 即時 |
| Impeller 効果 | 初回描画のジャンクを抑制 |

### 4.2 スループット

| 指標 | 値 |
|---|---|
| UI fps | **120 FPS 対応**(対応デバイスで) |
| レンダリング | Impeller でシェーダーコンパイルの待ちなし |
| スクロール | OS ネイティブ同等以上 |
| アニメーション | 独自描画ゆえ滑らか |

### 4.3 メモリフットプリント

| 項目 | 値 |
|---|---|
| Dart VM オーバーヘッド | 数 MB |
| Widget ツリー | アプリ複雑度に依存 |
| Isolate ごとのメモリ | 独立ヒープ(共有なし、コスト中) |

### 4.4 APK / アプリサイズ(H14)

| 項目 | サイズ |
|---|---|
| Flutter runtime(Engine) | **8〜12 MB**(最小 APK) |
| 典型的アプリ(APK) | 15〜25 MB |
| App Bundle 配信(Play) | ABI 別に分割、実インストール 10〜15 MB |
| ビルド最適化 | `--split-per-abi`、`--obfuscate --split-debug-info` |

**業務アプリ観点**: Kotlin ネイティブより **アプリサイズは大きめ**。MDM 配信の帯域が細い現場では考慮要。

### 4.5 ベンチマーク参照

- Flutter Performance Best Practices: https://docs.flutter.dev/perf/best-practices
- Impeller 性能レポート: https://docs.flutter.dev/perf/impeller

---

## 5. エコシステム・コミュニティ

### 5.1 パッケージ規模

| レジストリ | 規模 |
|---|---|
| **pub.dev** | **50,000+ パッケージ**(2026 時点概算) |
| Flutter Favorites | 公式認定の高品質パッケージ集 |
| Flutter Community | コミュニティパッケージ集合体 |

### 5.2 GitHub 定量指標(H11、2026-04 時点目安)

| リポジトリ | Stars(概算) |
|---|---|
| `flutter/flutter` | **約 170k**(2026-04) |
| `flutter/engine` | 約 10k |
| `dart-lang/sdk` | 約 10k |
| `flutter/packages` | 約 1k |

### 5.3 年次調査での位置

| 調査 | 位置 |
|---|---|
| Stack Overflow Developer Survey — Most Admired(Frameworks) | **TOP 10 圏内を継続** |
| GitHub Octoverse | 成長 Framework 上位 |
| Google Trends | 2020 年以降 React Native を上回る傾向 |

### 5.4 日本語リソース(H12)

| 種別 | 代表 |
|---|---|
| **FlutterKaigi**(日本のFlutter公式カンファレンス) | 2022 年〜、年 1 回 |
| flutter.jp(日本コミュニティ) | 勉強会、情報集約 |
| 書籍(日本語) | 『Flutter モバイルアプリ開発』『現場で使える Flutter 開発入門』他、増加中 |
| 日本語ブログ | Zenn / Qiita に豊富、技術記事多数 |
| 国内主要採用 | ニコン、JR 東日本アプリ、一部銀行モバイル |

### 5.5 認定・トレーニング

- **Google Developers Flutter Training**(Codelabs)
- **Udemy / Coursera Flutter コース**(豊富)
- Flutter Bootcamp(Academind、Angela Yu 等、YouTube で無償も多数)
- 公式認定資格は存在しない

### 5.6 コミュニティガバナンス

- Design proposals: https://github.com/flutter/flutter/wiki/Design-Documents
- RFC プロセス: オープンコントリビューション

---

## 6. 業務アプリ視点(HT 特化含む)

### 6.1 HT スキャナ連携

| 機能 | Flutter での実装 |
|---|---|
| **DataWedge Keyboard Wedge** | 通常の TextField / TextInput にフォーカスがあれば受信。実装不要 |
| **DataWedge Intent API** | **Zebra 公式サンプル `DataWedge-Flutter-Demo` あり** / `flutter_datawedge` パッケージ |
| Zebra EMDK | Method Channel 経由でのラッパ実装、公式サンプル限定的 |
| Honeywell / Datalogic | `honeywell_scanner`、`datalogic_flutter` 等(コミュニティ) |
| 物理キー | `RawKeyboardListener` / `Focus` Widget、取りこぼし限定的 |

### 6.2 MDM 連携

| 機能 | 代表実装 |
|---|---|
| App Config | `android_managed_config`、`flutter_managed_app_config` |
| Kiosk / Lock Task | プラットフォーム別 Platform Channel(ネイティブコード小量併記) |
| Device Owner | 同上 |

### 6.3 オフライン・同期

| 機能 | 代表ライブラリ |
|---|---|
| SQLite ORM | **Drift**(型安全、コード生成、推奨) / `sqflite`(基本) |
| NoSQL | **Isar**(高速、DB自作)、**Hive**(軽量 KVS) |
| バックグラウンドタスク | **`workmanager`**(Android WorkManager ラッパ) |
| 通信 | **Dio**(高機能)、`http`(軽量)、`retrofit`(コード生成) |
| WebSocket | `web_socket_channel` |
| gRPC | `grpc-dart`(公式) |
| JSON | `freezed` + `json_serializable` |

### 6.4 セキュリティ

| 機能 | 代表ライブラリ |
|---|---|
| セキュアストレージ | `flutter_secure_storage`(Android Keystore バックエンド) |
| 生体認証 | `local_auth`(Android Biometric / iOS FaceID-TouchID) |
| 証明書ピン留め | `dio_certificate_pinning` / `http_certificate_pinning` |
| 暗号化 | `cryptography`、`pointycastle` |
| OSS 検査 | pana、`flutter pub outdated --mode=security-only` |

### 6.5 カメラ・スキャン

| 機能 | 代表ライブラリ |
|---|---|
| カメラ | `camera`(公式) |
| QR/バーコード | **`mobile_scanner`**、`google_mlkit_barcode_scanning` |
| OCR | `google_mlkit_text_recognition`(ML Kit ラッパ) |
| 顔認識 | `google_mlkit_face_detection` |

### 6.6 印刷(ZPL・Bluetooth プリンタ)

| 機能 | 代表ライブラリ |
|---|---|
| Zebra ZPL | `zsdk_flutter`(コミュニティ) |
| Bluetooth プリンタ | 各ベンダ SDK ラッパ |
| PDF 生成 | `pdf`、`printing` |
| Android 印刷 | `printing` が統合 |

### 6.7 ネイティブ機能へのアクセスパターン

| 層 | 使用機能 |
|---|---|
| **高レベル(公式プラグイン)** | camera / geolocator / shared_preferences / path_provider 等 |
| **中レベル(Method Channel)** | カスタムネイティブ機能、Zebra Intent 受信等 |
| **中レベル(Pigeon コード生成)** | 型安全な Channel コード自動生成 |
| **低レベル(dart:ffi)** | C/C++ ライブラリ直接連携(RFID SDK、OpenCV 等) |

---

## 7. 主要採用事例(H9)

### 7.1 グローバル

| 企業 | 用途 |
|---|---|
| **Google** | Google Pay(Android/iOS 統合)、Google Ads Mobile、Google Classroom |
| **Alibaba** | Xianyu(2 億 MAU) |
| **BMW** | My BMW |
| **Toyota** | 車載インフォテインメント(Flutter Embedded) |
| **Nubank**(ブラジル銀行) | モバイルアプリ全面 |
| **eBay Motors** | 中古車アプリ |
| **Reflectly** | メンタルヘルス |
| **PUBG Mobile** | ゲーム内 UI(Flutter を UI 層で採用) |
| ByteDance / TikTok | 一部機能 |
| Etsy、Philips Hue、GeekBrains | モバイル |

### 7.2 国内

| 企業 | 用途 |
|---|---|
| ニコン | モバイルアプリ |
| JR 東日本 | アプリ(部分) |
| トヨタ系、他地方銀行 | 社内・顧客向けアプリ |
| 日立製作所、富士通 | 業務アプリ試行 |

### 7.3 ドメイン別

| ドメイン | 採用実績 |
|---|---|
| 金融・Fintech | Nubank、各国銀行アプリ |
| 物流・HT | 増加中(Zebra 公式デモ影響) |
| 車載 | Toyota 旗艦採用 |
| 産業用組込 | Flutter Embedded |
| SaaS モバイル | 中小スタートアップ中心 |
| 医療 | Philips(UI 層部分) |
| ゲーム UI | PUBG Mobile(ゲーム本体ではなく UI) |
| 官公庁(国内) | 限定的 |

---

## 8. AI/LLM 補完相性(H10)

| 観点 | Flutter |
|---|---|
| 学習データ量 | ○〜◎ Android/iOS 関連より少ないが急増中 |
| 型情報活用 | ◎ Dart 型推論強、補完精度高 |
| **Widget DSL 補完** | ◎ 宣言的・構造明快で AI が提案しやすい |
| GitHub Copilot | ◎ Widget Tree 構築、state 管理提案が精度高い |
| ChatGPT / Claude | ◎ Material/Cupertino Widget の質問に強い |
| 誤補完傾向 | 旧 API(Navigator 1、初期 Material 2)を提案することあり |
| Flutter Suggest / Gemini AI | Google 直営の Flutter 特化 AI アシスト(2024〜) |

---

## 9. 学習パス(H18)

### 9.1 学習曲線の形状

```
難易度
  │               ___________  エキスパート(6〜12ヶ月)
  │              /             Impeller / 大規模アーキ / Platform Channel 深掘り
  │          ___/
  │         /                   実務(2〜3ヶ月)
  │     ___/                    状態管理 / GoRouter / Drift
  │    /                        
  │   /                         基礎(2〜3週)
  │  /                          Dart + Flutter Widget
  │ /                           
  └──────────────────────→ 時間
```

**形状の特徴**:

- **初手やや急**(Widget Tree、BuildContext、setState の概念)
- **中盤スムーズ**(Dart は Java 類似で書ける、ライブラリ豊富)
- **長期は状態管理選定**で差が出る(Provider / Riverpod / Bloc / GetX)

### 9.2 基礎習得(2〜3 週、Java/JS 経験者前提)

| 週 | テーマ |
|---|---|
| 1 | Dart 文法(Java との差分、null safety、Future / async-await)、Flutter 初期セットアップ |
| 2 | Widget 基礎(Container / Row / Column / Text / Button / TextField)、Material/Cupertino、Navigator 1 |
| 3 | StatefulWidget / setState、ListView、非同期 UI、簡易 API 連携 |

### 9.3 実務レベル(2〜3 ヶ月)

| 項目 | 内容 |
|---|---|
| 状態管理 | **Riverpod**(推奨)または Provider / Bloc |
| ルーティング | **GoRouter**(公式推奨) |
| Theme / Design System | Material 3、カスタム Theme |
| フォーム | flutter_form_builder、reactive_forms |
| ローカル DB | **Drift** |
| 通信 | **Dio** + freezed + json_serializable |
| テスト | flutter_test、integration_test、mocktail |
| CI/CD | Codemagic または GitHub Actions |

### 9.4 エキスパート(6〜12 ヶ月)

| 項目 | 内容 |
|---|---|
| Impeller / レンダリング仕組み | カスタム描画(CustomPainter)、Shader 活用 |
| Platform Channel / Pigeon | ネイティブ機能追加 |
| dart:ffi | C ライブラリ直接呼出 |
| パフォーマンス最適化 | `const` Widget、RepaintBoundary、Profiler 解析 |
| Modular アーキテクチャ | Multi-package workspace、melos |
| Flutter Engine カスタマイズ | Embedded / 車載向け |

### 9.5 推奨教材

| カテゴリ | 教材 |
|---|---|
| 公式 | [Flutter Docs](https://docs.flutter.dev/)、[Flutter Codelabs](https://docs.flutter.dev/codelabs) |
| 書籍(日本語) | 『Flutter モバイルアプリ開発』『現場で使える Flutter 開発入門』 |
| 書籍(英語) | "Flutter in Action"(Manning)、"Flutter Complete Reference" |
| 動画 | Flutter 公式 YouTube、Flutter Forward セッション、Angela Yu コース |
| オンライン | Flutter Codelabs、App Brewery、Very Good Ventures Blog |
| 国内 | FlutterKaigi 動画、Zenn、Qiita |

---

## 10. 落とし穴・バッドパターン

| # | 落とし穴 | 対策 |
|---|---|---|
| 1 | setState 濫用で `build` が大量に走る | 状態を細分化、`const` Widget 活用、`Selector` で最小化 |
| 2 | BuildContext のライフサイクル誤解(async で context を使いリーク) | `mounted` チェック、`WidgetsBinding.instance.addPostFrameCallback` |
| 3 | ListView で `key` の欠落によるアイテム再利用バグ | `ValueKey` / `ObjectKey` 使用 |
| 4 | **Navigator 1 と Navigator 2 の混在**で画面遷移が破綻 | GoRouter に統一 |
| 5 | 状態管理ライブラリを途中で変えて全面書換 | **Riverpod を最初から採用**(推奨進化形) |
| 6 | `build_runner` 実行時間の爆増 | 生成対象を絞る、CI キャッシュ活用 |
| 7 | プラットフォーム差(iOS 特有の物理パディング等)を見落とす | `SafeArea` / `MediaQuery` 標準使用 |
| 8 | Impeller 特有の描画問題(初期バージョンでまれに発生) | 最新 Flutter 維持、フォールバックで Skia 切替 |
| 9 | アプリサイズ肥大化(不要アセット・フォント) | `--split-per-abi`、アセット精査、WebP |
| 10 | Plugin の Platform 互換性差(Android / iOS のどちらかだけ動く)| Federated plugin 構成の plugin を選定、公式 plugin 優先 |
| 11 | Dart Null Safety 移行時の混乱 | Dart 3 強制化済み、新規プロジェクトは問題なし |
| 12 | Isolate 間通信のシリアライズコスト軽視 | compute() で単純化、大量データなら shared memory(TransferableTypedData) |

---

## 11. 業務アプリでの適性評価

### 11.1 本件(Android HT + 20 画面 + Java チーム + Android 永続)

| 観点 | 評価 | コメント |
|---|---|---|
| 言語習熟難易度 | ★★★★☆ | Dart は Java 類似、学習コスト小 |
| UI 一貫性 | ★★★★★ | Flutter の最大強み |
| HT スキャナ連携 | ★★★★★ | Zebra 公式デモあり |
| オフライン耐性 | ★★★★★ | Drift / Isar で堅牢 |
| 8h 連続稼働 | ★★★★☆ | AOT のため安定、ネイティブ比で少し重い程度 |
| 長期保守 | ★★★★☆ | 四半期ごとの追従必要、工数織り込み要 |
| iOS 併用の将来性 | ★★★★★ | 1 コードで iOS 同時展開 |
| 国内人材プール | ★★★☆☆ | 増加中だが React 系より薄い |
| **Android 永続前提** | ★★★★☆ | ネイティブ Kotlin との明確な差別化が弱い |
| **総合** | ★★★★☆ | **Android 永続なら 4 ★、iOS 併用可能性があれば 5 ★** |

### 11.2 業務アプリ一般での評価

| ドメイン | 推奨度 |
|---|---|
| Android モバイル | ★★★★☆(Kotlin と競合) |
| iOS モバイル | ★★★★★ |
| Android + iOS 両対応 | ★★★★★(競合なし候補) |
| Web(PWA として) | ★★★☆☆(SEO 制限あり) |
| Desktop(Win/Mac/Linux) | ★★★★☆ |
| HT 業務 | ★★★★★(Zebra 公式) |
| 車載 / 産業 Embedded | ★★★★★(Toyota 採用) |
| 高負荷 UI / アニメ / ダッシュボード | ★★★★★ |
| ブランド統一必須 | ★★★★★(ピクセル単位一致) |

---

## 12. ロードマップ(H15)

公式ロードマップ: https://github.com/flutter/flutter/wiki/Roadmap

| 中期テーマ | 内容 |
|---|---|
| **Impeller Vulkan 完全移行**(Android) | OpenGL フォールバック削除 |
| **Wasm + Compose HTML** | Web 向け高速化 |
| **AI 統合**(Flutter Suggest / Gemini) | コード補完・Widget 提案 |
| Cupertino 忠実度向上 | iOS ネイティブ UX の違和感解消 |
| DevTools 機能強化 | AI 統合、プロファイル深化 |
| Material 3 Expressive 完全対応 | Google の新デザインシステム |
| Embedded ターゲット拡張 | 車載・POS・サイネージ |

---

## 13. 参照リンク

### 13.1 公式

| 対象 | URL |
|---|---|
| Flutter 公式 | https://flutter.dev/ |
| ドキュメント | https://docs.flutter.dev/ |
| リリース履歴 | https://docs.flutter.dev/release/archive |
| ロードマップ | https://github.com/flutter/flutter/wiki/Roadmap |
| Flutter Codelabs | https://docs.flutter.dev/codelabs |
| Flutter Samples | https://github.com/flutter/samples |
| Flutter Engine | https://github.com/flutter/engine |
| Dart 公式 | https://dart.dev/ |
| pub.dev | https://pub.dev/ |
| Flutter Favorites | https://docs.flutter.dev/packages-and-plugins/favorites |

### 13.2 業務・HT 向け

| 対象 | URL |
|---|---|
| Zebra DataWedge Flutter Demo | https://github.com/ZebraDevs/DataWedge-Flutter-Demo |
| flutter_datawedge | https://pub.dev/packages/flutter_datawedge |
| Drift(SQLite ORM) | https://drift.simonbinder.eu/ |
| Isar(NoSQL DB) | https://isar.dev/ |
| Dio(HTTP)| https://pub.dev/packages/dio |
| Riverpod(状態管理) | https://riverpod.dev/ |
| GoRouter(公式ルーティング) | https://pub.dev/packages/go_router |
| freezed / json_serializable | https://pub.dev/packages/freezed |
| mobile_scanner(QR/バーコード) | https://pub.dev/packages/mobile_scanner |
| Google ML Kit(Flutter ラッパ) | https://pub.dev/publishers/flutter-ml.dev/packages |

### 13.3 開発環境

| 対象 | URL |
|---|---|
| VS Code Flutter Extension | https://marketplace.visualstudio.com/items?itemName=Dart-Code.flutter |
| Android Studio Flutter Plugin | https://plugins.jetbrains.com/plugin/9212-flutter |
| Flutter DevTools | https://docs.flutter.dev/tools/devtools |
| fvm(Flutter Version Management) | https://fvm.app/ |
| Codemagic(Flutter特化 CI/CD) | https://codemagic.io/ |

### 13.4 コミュニティ・学習

| 対象 | URL |
|---|---|
| Flutter Community(GitHub) | https://github.com/fluttercommunity |
| Flutter Japan User Group | https://flutter-jp.connpass.com/ |
| FlutterKaigi | https://flutterkaigi.jp/ |
| Very Good Ventures Blog | https://verygood.ventures/blog |
| Flutter Weekly | https://flutterweekly.net/ |

### 13.5 セキュリティ

| 対象 | URL |
|---|---|
| Flutter Security | security@flutter.dev |
| GitHub Security Advisories | https://github.com/flutter/flutter/security/advisories |

---

## 14. 本ファイルの位置付け

- FW 詳細プロファイル雛形(profile_kotlin.md と同粒度)
- 観点 A〜H(selection_criteria.md)を Flutter に投影
- 次は `profile_react_native.md`、`profile_ionic_capacitor.md`、`profile_kmp.md`、`profile_maui.md` を予定

---

## 更新履歴

- **2026-04-23**: 初版作成。FW 詳細プロファイル(Flutter)。
