# モバイルフレームワーク比較表 IT用語集

本用語集は「モバイルフレームワーク比較表(Excel)」内に登場するIT用語をジャンル別に整理したものです。
各項目は「用語 | 説明 | ジャンル」の形式で記載しています。情報は2026年4月時点のものです。

---

## 1. モバイル系フレームワーク(クロスプラットフォーム・ハイブリッド)

| 用語 | 説明 | ジャンル |
|---|---|---|
| Flutter | Google製のクロスプラットフォームUIフレームワーク。Dart言語で記述し、独自描画エンジンImpellerで全プラットフォームをピクセル単位で自前描画する。 | モバイルFW |
| React Native | Meta(Facebook)製のクロスプラットフォームフレームワーク。JavaScript/TypeScriptで記述し、OS純正のネイティブUIコンポーネントを利用する。 | モバイルFW |
| Expo | React Native上に構築された統合開発プラットフォーム。EAS Build/UpdateによりクラウドビルドやOTA配信が可能。 | モバイルFW |
| Ionic Framework | Webベースのクロスプラットフォーム用UIコンポーネント集。Angular/React/Vueなど複数のJSフレームワークと組み合わせて使える。 | モバイルFW |
| Capacitor | Ionic社製のネイティブランタイム。Webアプリをネイティブアプリとしてパッケージングし、プラグインで各種ネイティブAPIへアクセス可能。 | モバイルFW |
| NativeScript | JS/TSからWebViewを介さず直接ネイティブUIを呼び出すフレームワーク。Angular/Vue等と統合可能。 | モバイルFW |
| Kotlin Multiplatform(KMP) | JetBrains製。Kotlinで書いたビジネスロジックをAndroid/iOS/Desktop/Serverで共有できるSDK。UIは各プラットフォームネイティブを併用。 | モバイルFW |
| Compose Multiplatform | JetBrains製。Jetpack Composeをマルチプラットフォーム化した宣言的UIフレームワーク。 | モバイルFW |
| Jetpack Compose | Android公式の宣言的UIツールキット。Kotlinで記述する。 | モバイルUI |
| SwiftUI | Apple純正のiOS/macOS向け宣言的UIフレームワーク。 | モバイルUI |
| UIKit | 従来のAppleネイティブiOS UIフレームワーク(命令的)。 | モバイルUI |
| .NET MAUI | Microsoft製のクロスプラットフォームUIフレームワーク。C#/XAMLで記述。Xamarin.Formsの後継。 | モバイルFW |
| Xamarin | Microsoft製のクロスプラットフォームフレームワーク(C#)。.NET MAUIへ移行。 | モバイルFW(レガシー) |
| Cordova | Apache製のWebベースハイブリッドアプリフレームワーク。PhoneGapの後継。 | モバイルFW(レガシー) |
| PhoneGap | Adobe製(2020年終了)のハイブリッドアプリフレームワーク。Cordovaのディストリビューション版。 | モバイルFW(レガシー) |
| TWA(Trusted Web Activity) | PWAをAndroidアプリとして配信可能にする仕組み。Chrome Custom Tabsベース。 | モバイルFW |
| Unity | C#ベースのゲームエンジン。2D/3Dゲーム・AR/VR開発で事実上の標準。 | ゲームエンジン |
| Unreal Engine | Epic Games製のゲームエンジン。C++/Blueprintでの記述が可能。 | ゲームエンジン |

---

## 2. Webフロントエンドフレームワーク

| 用語 | 説明 | ジャンル |
|---|---|---|
| React | Meta製のUIライブラリ。JSXと関数コンポーネント/Hooksを中心に、npmの巨大エコシステムを持つ。 | Web FW |
| Vue | 段階的に採用できる進歩的JSフレームワーク。テンプレート構文がHTMLに近く学習コストが低い。 | Web FW |
| Angular | Google製の完全Webアプリケーションフレームワーク。TypeScript標準、DI・モジュール・RxJSが統合されている。 | Web FW |
| jQuery | 古典的JavaScriptライブラリ。DOM操作・イベント・ajax通信を抽象化、命令的スタイル。 | Web FW(レガシー) |

---

## 3. プログラミング言語・ランタイム

| 用語 | 説明 | ジャンル |
|---|---|---|
| Dart | Google製のFlutter向け言語。Javaライクな構文、AOT/JIT両対応、null safety標準。 | 言語 |
| Kotlin | JetBrains製のJVM言語。Androidの推奨言語、Javaと完全相互運用可能。 | 言語 |
| Swift | Apple製のiOS/macOSネイティブ開発言語。 | 言語 |
| JavaScript(JS) | ブラウザとNode.js等で動作するスクリプト言語。 | 言語 |
| TypeScript(TS) | Microsoft製の静的型付きJavaScript。 | 言語 |
| Java | JVM上で動作するオブジェクト指向言語。Androidネイティブ開発で長年使用された。 | 言語 |
| C# | Microsoft製の.NET向けOOP言語。Javaに構文が類似。 | 言語 |
| AOT(Ahead-Of-Time)コンパイル | 実行前にネイティブコードへ変換する方式。起動が速い。 | 言語基盤 |
| JIT(Just-In-Time)コンパイル | 実行時にバイトコードをネイティブコードへ変換する方式。 | 言語基盤 |
| JVM(Java Virtual Machine) | Java/Kotlin等のバイトコードを実行する仮想マシン。 | 言語基盤 |
| JSI(JavaScript Interface) | React Native新アーキテクチャで、JS-ネイティブ間の同期呼び出しを実現する仕組み。 | 言語基盤 |
| Fabric | React Native新アーキテクチャのレンダラー。同期UI更新を実現。 | 言語基盤 |
| TurboModules | React Native新アーキテクチャのネイティブモジュール遅延読込機構。 | 言語基盤 |
| Impeller | Flutterの新描画エンジン。Skiaの後継としてiOS/Androidで安定稼働。 | 言語基盤 |
| Hermes | Meta製の軽量JSエンジン。React Nativeで標準採用。 | 言語基盤 |
| WebAssembly(Wasm) | ブラウザで動作するバイナリ命令フォーマット。 | 言語基盤 |

---

## 4. UI/UXパラダイム

| 用語 | 説明 | ジャンル |
|---|---|---|
| SPA(Single Page Application) | 単一HTMLでJSが画面遷移を担うWebアプリ構成。 | UIパラダイム |
| 宣言的UI | 「UI = f(状態)」の考え方。状態変更でUIが自動追従する。Compose/SwiftUI/React/Vue等。 | UIパラダイム |
| 命令的UI | DOMを直接操作してUIを書き換える方式。jQuery等。 | UIパラダイム |
| コンポーネント指向 | UIを再利用可能な部品(コンポーネント)の組合せで構築する設計思想。 | UIパラダイム |
| JSX | JavaScript内にHTMLライクなマークアップを書くReactの記法。 | 構文 |
| SFC(Single File Component) | .vueファイルにテンプレート/スクリプト/スタイルを一体化して記述するVueの構成。 | 構文 |
| Hooks | Reactの関数コンポーネントで状態・副作用・コンテキストを扱うAPI群。 | 構文 |
| Composition API | Vue 3の関数ベースのコンポーネント記述方式。 | 構文 |
| Options API | Vue 2以来のオブジェクトベースのコンポーネント記述方式。 | 構文 |
| Widget(Flutter) | Flutterにおける全てのUI構成単位。親子のTree構造でUIを記述する。 | 構文 |
| Material Design | Google製のデザインシステム。 | デザインシステム |
| Human Interface Guidelines(HIG) | Apple製のデザインガイドライン。 | デザインシステム |

---

## 5. 状態管理・ルーティング

| 用語 | 説明 | ジャンル |
|---|---|---|
| Redux | React向けの代表的状態管理ライブラリ。単一ストア+アクション+リデューサ。 | 状態管理 |
| Zustand | React向けの軽量状態管理ライブラリ。 | 状態管理 |
| Jotai | React向けのアトミック状態管理ライブラリ。 | 状態管理 |
| Recoil | Meta製のReact向け状態管理ライブラリ。 | 状態管理 |
| Pinia | Vue 3の公式推奨状態管理ライブラリ(Vuex後継)。 | 状態管理 |
| Vuex | Vue 2時代の公式状態管理ライブラリ。 | 状態管理(レガシー) |
| TanStack Query | サーバ状態(API取得結果)のキャッシュ・同期管理ライブラリ。 | 状態管理 |
| Riverpod | Flutter向けの状態管理ライブラリ。 | 状態管理 |
| Provider(Flutter) | Flutter向けの状態管理ライブラリ。 | 状態管理 |
| Bloc | Flutter向けのアーキテクチャパターン/ライブラリ。Businessロジックを分離。 | 状態管理 |
| RxJS | Reactive Extensions for JavaScript。Angularで標準採用される非同期ストリームライブラリ。 | 状態管理 |
| Observable | RxJSの中核概念。時系列のデータストリーム。 | 状態管理 |
| React Navigation | React Native向けのルーティングライブラリ。 | ルーティング |
| Vue Router | Vue公式のルーティングライブラリ。 | ルーティング |
| GoRouter | Flutter向けのルーティングライブラリ。 | ルーティング |
| Expo Router | Expoのファイルベースルーティングライブラリ。 | ルーティング |

---

## 6. DI・アーキテクチャ

| 用語 | 説明 | ジャンル |
|---|---|---|
| DI(Dependency Injection) | 依存性注入。オブジェクト同士の依存を外部から注入する設計パターン。 | 設計パターン |
| Hilt | Android向けDIフレームワーク。Daggerの上位ラッパ。 | DIライブラリ |
| Koin | Kotlin向けの軽量DIフレームワーク。 | DIライブラリ |
| Spring | Java製のサーバサイドフレームワーク。DIとAOPが中核。 | サーバFW |
| Struts | 古くからあるJava Webフレームワーク。 | サーバFW(レガシー) |
| Seasar | 日本発のJava DIコンテナ(保守終了)。 | サーバFW(レガシー) |
| Reactive Forms | Angularのフォーム管理機構。プログラムで制御可能な厳密フォーム。 | Angular機能 |
| Angular Material | Angular公式のMaterial Design UIコンポーネント集。 | UI部品 |
| MVVM | Model-View-ViewModelアーキテクチャパターン。 | 設計パターン |
| Reactive Programming | データフローと変化の伝播を中心に据えるプログラミングスタイル。 | 設計パターン |

---

## 7. ネットワーク・APIクライアント・認証

| 用語 | 説明 | ジャンル |
|---|---|---|
| REST | Representational State Transfer。HTTPベースのAPI設計スタイル。 | API |
| gRPC | Google製のRPCフレームワーク。Protocol Buffersを使用。 | API |
| Retrofit | Android向けのHTTPクライアント(Square製)。 | HTTPクライアント |
| OkHttp | Android向けのHTTPクライアント(Square製)。 | HTTPクライアント |
| Ktor | JetBrains製のKotlin HTTPクライアント/サーバFW。 | HTTPクライアント |
| Dio | Flutter向けのHTTPクライアント。 | HTTPクライアント |
| axios | JavaScript向けの代表的HTTPクライアント。 | HTTPクライアント |
| fetch API | ブラウザ標準の非同期HTTP API。 | HTTPクライアント |
| OAuth | 認可プロトコル。サードパーティアプリへのアクセス権委譲。 | 認証認可 |
| JWT(JSON Web Token) | JSON形式のトークンによる認証方式。 | 認証認可 |
| WebAuthn | Web認証API。生体認証や物理キーによる認証を標準化。 | 認証認可 |
| HTTPS | HTTP over TLS。PWAでは必須。 | 通信 |
| VPN(Virtual Private Network) | 仮想私設網。社内リソースへの安全な接続。 | 通信 |
| クライアント証明書認証 | 証明書による双方向認証方式。 | 認証認可 |

---

## 8. データ永続化

| 用語 | 説明 | ジャンル |
|---|---|---|
| SQLite | 組込みRDBMS。モバイルで広く使われる。 | DB |
| Room | Android向けSQLiteラッパ(Jetpack)。 | ORM |
| Drift | Flutter向けSQLite/型安全ORM。 | ORM |
| Isar | Flutter向けの高速NoSQL DB。 | DB |
| Hive | Flutter向けの軽量キー値DB。 | DB |
| SQLDelight | Kotlin向け型安全SQLライブラリ(KMPで使用)。 | ORM |
| WatermelonDB | React Native向けのオフライン対応DB。 | DB |
| IndexedDB | ブラウザ標準の構造化データDB。PWAのオフライン基盤。 | DB |
| LocalStorage | ブラウザ標準のキー値ストレージ。 | ストレージ |
| SessionStorage | ブラウザ標準のセッション単位ストレージ。 | ストレージ |
| JPA(Java Persistence API) | Java標準のORMインタフェース。 | ORM |
| MyBatis | Java向けのSQLマッパ。 | ORM |
| JDBC | Java Database Connectivity。JavaのDB接続API。 | DB接続 |

---

## 9. Androidプラットフォーム

| 用語 | 説明 | ジャンル |
|---|---|---|
| Activity | Androidの画面単位コンポーネント。 | Android基本 |
| Fragment | Activity内で再利用可能なUI単位。 | Android基本 |
| Intent | Androidのコンポーネント間メッセージング機構。 | Android基本 |
| Broadcast Intent | システム/アプリ間のブロードキャスト通知方式。 | Android基本 |
| MethodChannel | Flutter-ネイティブ間の双方向メッセージング機構。 | FFI |
| EventChannel | Flutter-ネイティブ間のストリーミングイベント機構。 | FFI |
| Target SDK | アプリがターゲットとするAndroid APIレベル。 | ビルド設定 |
| Min SDK | アプリが動作する最小のAndroid APIレベル。 | ビルド設定 |
| NDK(Native Development Kit) | AndroidでC/C++を使うためのツールキット。 | Android基本 |
| ABI(Application Binary Interface) | ARMv7/arm64-v8a等のCPU命令セット規格。 | Android基本 |
| APK(Android Package) | Androidアプリの配布形式。 | 配布形式 |
| AAB(Android App Bundle) | Google Play用の新配布形式。 | 配布形式 |
| WorkManager | Androidのバックグラウンド処理ライブラリ(Jetpack)。 | Jetpack |
| AlarmManager | Androidの時刻指定処理API。 | Android基本 |
| Foreground Service | 前面常駐するAndroidサービス。 | Android基本 |
| WakeLock | 画面/CPUスリープを抑止する機構。 | Android基本 |
| ViewModel(Jetpack) | 画面ライフサイクルに対応するデータホルダ。 | Jetpack |
| LiveData | Jetpackの観測可能データホルダ(旧世代)。 | Jetpack |
| StateFlow | Kotlin Coroutinesの状態ホルダ(新世代)。 | Kotlin |
| Jetpack | Android開発向けライブラリ・ツール集の総称。 | ライブラリ群 |
| KeyEvent | Androidのキー入力イベント。 | Android基本 |
| Manifest(AndroidManifest.xml) | アプリの権限・コンポーネント宣言ファイル。 | Android基本 |

---

## 10. iOS プラットフォーム(参考)

| 用語 | 説明 | ジャンル |
|---|---|---|
| Xcode | iOS/macOS公式IDE。 | IDE |
| CocoaPods | iOS向けの依存性管理ツール。 | ビルド |
| SPM(Swift Package Manager) | Apple純正の依存性管理ツール。 | ビルド |

---

## 11. HT(ハンディターミナル)・業務デバイス関連

| 用語 | 説明 | ジャンル |
|---|---|---|
| HT(Handheld Terminal) | 業務用ハンディターミナル。バーコードスキャナ内蔵の業務用Android/Windows端末。 | ハードウェア |
| Zebra Technologies | HTデバイス最大手。旧Symbol/Motorola Solutions系譜。 | ベンダー |
| Honeywell | HTデバイス大手。 | ベンダー |
| Datalogic | HTデバイスメーカー(欧州系)。 | ベンダー |
| DataWedge | Zebra端末標準のデータキャプチャサービス。スキャン結果をアプリに届ける。 | ソフトウェア |
| Keyboard Wedge / Keystroke Output | DataWedgeの出力モード。スキャン結果をキー入力として送信する。 | ソフトウェア |
| DataWedge Intent API | DataWedgeのAndroid Intentベース制御API。詳細なスキャナ制御が可能。 | ソフトウェア |
| EMDK(Enterprise Mobility Development Kit) | Zebra製の低レベルSDK。DataWedgeより細かい制御が可能。 | SDK |
| MultiBarcode | 複数バーコードを1スキャンで同時読取する機能。 | 機能 |
| OCR(Optical Character Recognition) | 光学文字認識。 | 機能 |
| ZPL(Zebra Programming Language) | Zebraプリンタ向けのラベル記述言語。 | プリンタ |
| NFC(Near Field Communication) | 近距離無線通信。ICカード・タグ読取等で使用。 | ハードウェア |
| RFID(Radio Frequency Identification) | 電波による個体識別技術。UHF/HF帯等の帯域を使用。 | ハードウェア |
| 物理トリガーキー | HTの物理ボタンで押すとスキャナが起動するキー。 | ハードウェア |
| MSR(Magnetic Stripe Reader) | 磁気ストライプ読取機。クレジットカード等の磁気カード対応。 | ハードウェア |

---

## 12. MDM・エンタープライズ運用

| 用語 | 説明 | ジャンル |
|---|---|---|
| MDM(Mobile Device Management) | モバイル端末の一元管理。構成配信・アプリ配布・ロック等を行う。 | 運用 |
| EMM(Enterprise Mobility Management) | MDMを包含する企業モバイル総合管理。 | 運用 |
| SOTI MobiControl | 代表的なMDM製品(SOTI社)。 | 製品 |
| VMware Workspace ONE | 代表的なMDM製品(旧AirWatch)。 | 製品 |
| Microsoft Intune | Microsoft製のMDM製品。 | 製品 |
| Zebra StageNow | Zebra端末向けの一括構成ツール。 | 製品 |
| App Config(Managed Configuration) | MDMからアプリへ設定を動的注入する標準機構。 | 運用 |
| Kiosk Mode | 特定アプリのみ操作可能にするモード。 | 運用 |
| Single-App Mode | iOS/Androidの単一アプリ専用モード。 | 運用 |
| Lock Task Mode | Androidの画面固定モード(公式API)。 | 運用 |
| Device Owner | Androidのデバイス全体管理者権限モード。 | 運用 |
| Android Enterprise | Googleが提供する企業向けAndroid管理基盤。 | 運用 |

---

## 13. PWA・Web API

| 用語 | 説明 | ジャンル |
|---|---|---|
| PWA(Progressive Web App) | インストール可能・オフライン動作可能なWebアプリ。 | Web技術 |
| Service Worker | バックグラウンドで動作するブラウザ常駐スクリプト。キャッシュ・プッシュ等を担う。 | Web API |
| Web App Manifest | PWAの設定ファイル(アイコン・表示モード等)。 | Web技術 |
| A2HS(Add to Home Screen) | PWAをホーム画面に追加する仕組み。 | Web技術 |
| Background Sync | 通信断復帰時にサーバへデータ自動送信するAPI。 | Web API |
| Periodic Sync | 定期的なバックグラウンド同期API。 | Web API |
| Web Push | Web向けプッシュ通知API。 | Web API |
| getUserMedia | カメラ・マイクにアクセスするブラウザAPI。 | Web API |
| Barcode Detection API | ブラウザ標準のバーコード検出API(Chromiumベース)。 | Web API |
| Web Bluetooth | ブラウザからBluetoothデバイスと通信するAPI(Chromium系)。 | Web API |
| Web USB | ブラウザからUSBデバイスにアクセスするAPI(Chromium系)。 | Web API |
| Web NFC | ブラウザからNFCにアクセスするAPI(Chromium Android)。 | Web API |
| WebView | ネイティブアプリ内でWebページを表示するコンポーネント。 | Web技術 |
| Chrome Custom Tabs | Chromeをアプリ内で使う仕組み。TWAの基盤。 | Web技術 |
| Digital Asset Links | サイトとアプリの所有関係を証明する仕組み。TWAで使用。 | Web技術 |

---

## 14. Expo 関連

| 用語 | 説明 | ジャンル |
|---|---|---|
| EAS Build | Expo Application Services Build。クラウドビルドサービス。 | Expoサービス |
| EAS Update | Expo のOTA配信サービス。 | Expoサービス |
| OTA(Over-The-Air)update | ストア経由せずにアプリ更新を配信する方式。 | 配信 |
| Managed Workflow | Expo標準のワークフロー。ネイティブコード編集不可。 | Expo概念 |
| Bare Workflow | Expoでネイティブコード編集可能なワークフロー。 | Expo概念 |
| Dev Client | Expoのカスタム開発ビルド。 | Expo概念 |
| Prebuild | Expoがネイティブプロジェクト(iOS/Androidフォルダ)を生成するコマンド。 | Expo概念 |

---

## 15. 開発ツール・ビルド

| 用語 | 説明 | ジャンル |
|---|---|---|
| npm(Node Package Manager) | Node.js/JavaScript向けパッケージ管理ツール。 | パッケージ管理 |
| yarn | JavaScript向けパッケージ管理ツール(Meta製)。 | パッケージ管理 |
| pnpm | JavaScript向けパッケージ管理ツール(省容量)。 | パッケージ管理 |
| webpack | JavaScriptモジュールバンドラ(従来標準)。 | ビルドツール |
| vite | 高速なモダンビルドツール(Vue作者Evan You製)。 | ビルドツール |
| Maven | Java向け依存性管理・ビルドツール。 | ビルドツール |
| Gradle | Java/Kotlin/Android向けビルドツール。 | ビルドツール |
| pub.dev | Dart/Flutter公式パッケージレジストリ。 | パッケージ管理 |
| ESLint | JavaScript/TypeScriptリンター。 | コード品質 |
| Prettier | コードフォーマッタ。 | コード品質 |
| IDE(Integrated Development Environment) | 統合開発環境。 | ツール |
| Android Studio | Android公式IDE。IntelliJ IDEA ベース。 | IDE |
| IntelliJ IDEA | JetBrains製のIDE。 | IDE |
| Visual Studio Code(VS Code) | Microsoft製のコードエディタ。 | IDE |
| Git | 分散バージョン管理システム。 | バージョン管理 |
| CI/CD(Continuous Integration / Continuous Delivery) | 継続的統合/継続的配信。自動ビルド・テスト・デプロイ。 | 開発プロセス |
| Dependabot / Renovate | 依存関係自動更新ツール。 | コード品質 |

---

## 16. テスト

| 用語 | 説明 | ジャンル |
|---|---|---|
| JUnit | Java向け標準単体テストフレームワーク。 | テストFW |
| flutter_test | Flutter公式のテストフレームワーク。 | テストFW |
| Jest | Meta製のJavaScriptテストフレームワーク。 | テストFW |
| Vitest | Vite向けの高速テストフレームワーク。 | テストFW |
| React Testing Library(RTL) | React向けテストユーティリティ。 | テストFW |
| Vue Test Utils | Vue公式のテストユーティリティ。 | テストFW |
| Jasmine | BDDスタイルのJSテストフレームワーク。Angular標準。 | テストFW |
| Karma | Angular標準のテストランナー(非推奨化)。 | テストFW |
| TestBed | Angular公式のテスト基盤。DIされたコンポーネントをモックしやすい。 | テストFW |

---

## 17. 非同期処理

| 用語 | 説明 | ジャンル |
|---|---|---|
| Promise | JavaScriptの非同期処理オブジェクト。 | 非同期 |
| async / await | 非同期処理を同期的に記述する構文。 | 非同期 |
| Coroutine(Kotlin) | Kotlinの軽量並行処理機構。 | 非同期 |
| suspend function | Kotlin coroutineの中断可能関数。 | 非同期 |
| Future(Dart) | Dartの非同期結果を表すオブジェクト。 | 非同期 |
| Stream(Dart) | Dartの非同期イベント列。 | 非同期 |
| Flow(Kotlin) | Kotlinのコールドストリーム(Reactive)。 | 非同期 |
| ajax(Asynchronous JavaScript and XML) | 非同期HTTP通信の総称。jQueryで広まった。 | 非同期 |

---

## 18. 言語機能・型システム

| 用語 | 説明 | ジャンル |
|---|---|---|
| OOP(Object-Oriented Programming) | オブジェクト指向プログラミング。 | パラダイム |
| Generics | 型パラメータ化。 | 言語機能 |
| null safety | null参照エラーを型システムで防ぐ機構。Kotlin/Dart/Swift/TS等。 | 言語機能 |
| Extension function | 既存クラスに関数を追加できるKotlinの機能。 | 言語機能 |
| Data class | 構造化データ用の軽量クラス(Kotlin/Dart等)。 | 言語機能 |
| Scope functions | Kotlinのlet/apply/run/with/also等のスコープ変換関数。 | 言語機能 |
| Decorator | TypeScript/Python等のクラス/関数装飾機構。 | 言語機能 |
| Annotation | Javaのメタデータ記述。 | 言語機能 |
| ES2020+ | ECMAScript 2020以降のモダンJavaScript仕様。 | 言語機能 |
| DOM(Document Object Model) | HTMLを構造化して表現する標準モデル。 | Web基盤 |

---

## 19. 開発プロセス・ビジネス用語

| 用語 | 説明 | ジャンル |
|---|---|---|
| MVP(Minimum Viable Product) | 実用最小限の製品。仮説検証用の初期版。 | 開発プロセス |
| PoC(Proof of Concept) | 概念実証。技術や構想の実現可能性検証。 | 開発プロセス |
| SIer | System Integrator。日本特有のIT受託開発業態。 | 業界 |
| SI(System Integration) | システム構築。SIerの主業務。 | 業界 |
| LTS(Long-Term Support) | 長期サポート版。セキュリティ修正等を長期提供。 | ソフトウェア運用 |
| TCO(Total Cost of Ownership) | 総保有コスト。開発だけでなく運用・保守まで含む。 | ビジネス |
| ROI(Return On Investment) | 投資収益率。 | ビジネス |
| CRUD | Create/Read/Update/Delete。基本データ操作の4種。 | 設計 |
| RBAC(Role-Based Access Control) | ロールベースアクセス制御。 | セキュリティ |
| XSS(Cross-Site Scripting) | 悪意あるスクリプト注入脆弱性。 | セキュリティ |
| CSRF(Cross-Site Request Forgery) | クロスサイトリクエストフォージェリ攻撃。 | セキュリティ |
| RxJSのオペレータ | map/filter/mergeMap等のObservable変換関数。 | 関数型 |

---

## 20. その他のエコシステム・用語

| 用語 | 説明 | ジャンル |
|---|---|---|
| FCM(Firebase Cloud Messaging) | Google提供のAndroid/iOS向けプッシュ通知基盤。 | 通知 |
| APNS(Apple Push Notification service) | Apple提供のiOS向けプッシュ通知基盤。 | 通知 |
| Firebase Crashlytics | Google提供のクラッシュレポート収集サービス。 | 監視 |
| SDK(Software Development Kit) | 特定プラットフォーム・機能向けの開発キット。 | 汎用 |
| API(Application Programming Interface) | アプリケーション間のインタフェース。 | 汎用 |
| Playgrounds / DartPad / TypeScript Playground | オンラインの言語お試し環境。 | 学習ツール |
| Google Play | Androidアプリ公式ストア。 | 配布 |
| App Store | iOSアプリ公式ストア。 | 配布 |
| Stack Overflow | プログラミングQ&Aサイト。 | コミュニティ |
| Hot Reload | コード変更を状態を保ったままアプリに即反映する開発体験。Flutter/Ionic/RN/Expo等で提供。 | 開発体験 |
| Fast Refresh | React Nativeにおける Hot Reloadに相当する機能。 | 開発体験 |

---

## 付録:略語索引

主要な略語のアルファベット順インデックス。

| 略語 | 正式名称 |
|---|---|
| A2HS | Add to Home Screen |
| AAB | Android App Bundle |
| ABI | Application Binary Interface |
| AOT | Ahead-Of-Time(コンパイル) |
| API | Application Programming Interface |
| APK | Android Package |
| APNS | Apple Push Notification service |
| CI/CD | Continuous Integration / Continuous Delivery |
| CRUD | Create / Read / Update / Delete |
| CSRF | Cross-Site Request Forgery |
| DI | Dependency Injection |
| DOM | Document Object Model |
| EAS | Expo Application Services |
| EMDK | Enterprise Mobility Development Kit |
| EMM | Enterprise Mobility Management |
| FCM | Firebase Cloud Messaging |
| HIG | Human Interface Guidelines |
| HT | Handheld Terminal |
| HTTPS | HTTP over TLS |
| IDE | Integrated Development Environment |
| JDBC | Java Database Connectivity |
| JIT | Just-In-Time(コンパイル) |
| JPA | Java Persistence API |
| JS | JavaScript |
| JSI | JavaScript Interface |
| JSX | JavaScript XML |
| JVM | Java Virtual Machine |
| JWT | JSON Web Token |
| KMP | Kotlin Multiplatform |
| LTS | Long-Term Support |
| MDM | Mobile Device Management |
| MVP | Minimum Viable Product |
| MVVM | Model-View-ViewModel |
| NDK | Native Development Kit |
| NFC | Near Field Communication |
| OCR | Optical Character Recognition |
| OOP | Object-Oriented Programming |
| ORM | Object-Relational Mapping |
| OTA | Over-The-Air(update) |
| PoC | Proof of Concept |
| PWA | Progressive Web App |
| RBAC | Role-Based Access Control |
| REST | Representational State Transfer |
| RFID | Radio Frequency Identification |
| RN | React Native |
| ROI | Return On Investment |
| RTL | React Testing Library |
| SDK | Software Development Kit |
| SFC | Single File Component |
| SI | System Integration |
| SPA | Single Page Application |
| SPM | Swift Package Manager |
| SSR | Server-Side Rendering |
| TCO | Total Cost of Ownership |
| TS | TypeScript |
| TWA | Trusted Web Activity |
| Wasm | WebAssembly |
| XSS | Cross-Site Scripting |
| ZPL | Zebra Programming Language |

---

## 更新履歴

- 2026-04-23: 初版作成。モバイルフレームワーク比較表 全7シートから抽出。

