# 言語の活用範囲 と ネイティブ機能アクセス・代表ライブラリ

本ドキュメントは以下 2 つの視点を扱う:

1. **言語の活用範囲**: 各言語で Android/モバイル 以外に何ができるか(スキル投資の汎用性評価)
2. **ネイティブ機能別アクセス方法と代表ライブラリ**: 業務アプリで使う機能を各 FW でどう実現するか

- 情報は **2026 年 4 月時点**
- 対象 FW: ネイティブ Android / Flutter / React Native(+ Expo)/ Ionic + Capacitor / KMP / .NET MAUI / PWA

---

## Part 1. 言語の活用範囲(何が作れるか)

### 凡例

- ◎=主要用途(事実上の標準) / ○=実用可 / △=可能だが非主流 / ×=非対応または実質不可

### 1.1 対応範囲マトリクス

| 言語 | Androidモバイル | iOSモバイル | Webフロント | サーバサイド | デスクトップ | 組込/IoT | CLI/スクリプト | ゲーム | データサイエンス |
|---|---|---|---|---|---|---|---|---|---|
| **Kotlin** | ◎ | ○(KMP) | △(Kotlin/JS) | ◎(Ktor/Spring) | ○(Compose Desktop) | △(Kotlin Native) | ○(`kotlinc -script`) | △(libGDX) | △(KotlinDL) |
| **Java** | ◎(レガシ+併用) | × | △(GWT 旧) | ◎(Spring/Jakarta EE) | ○(JavaFX/Swing) | ○(Android Things 終了、Pi等) | ○ | △(jMonkeyEngine) | ○(JVM ML) |
| **Dart** | ◎(Flutter) | ◎(Flutter) | ○(Flutter Web) | △(dart:io/Dart Frog) | ○(Flutter Desktop) | ○(Flutter Embedded) | ○(pub bin) | × | × |
| **TypeScript** | ◎(RN/Ionic) | ◎(RN/Ionic) | ◎ | ◎(Node.js/Deno/Bun) | ◎(Electron/Tauri) | ○(MCU Node) | ◎ | ○(Phaser/PixiJS) | ○(TensorFlow.js) |
| **JavaScript** | ○(RN/Ionic) | ○(RN/Ionic) | ◎ | ◎(Node.js) | ◎(Electron) | ○ | ◎ | ○ | ○ |
| **C#** | ○(MAUI/Xamarin) | ○(MAUI) | ○(Blazor) | ◎(ASP.NET) | ◎(WPF/WinUI/MAUI) | ○ | ○(PowerShell/`dotnet-script`) | ◎(Unity) | ○(ML.NET) |
| **Swift** | × | ◎ | △(SwiftWasm) | ○(Vapor) | ◎(macOS) | △ | ○(Swift Scripts) | △ | × |
| **Python(参考)** | △(Kivy/BeeWare) | △ | ○(Django/Flask) | ◎ | ○(PyQt/Tk) | ◎(MicroPython) | ◎ | ○(Pygame) | ◎ |

### 1.2 主要言語の活用範囲 詳細

#### Kotlin

| 領域 | 主要フレームワーク / 位置付け |
|---|---|
| Android モバイル | **Jetpack Compose / Android SDK** — Google 公式推奨 |
| iOS モバイル | **Kotlin Multiplatform(KMP)** — ロジック共通化、UI は SwiftUI / Compose MP |
| サーバサイド | **Ktor**(JetBrains 公式、軽量) / **Spring Boot**(Spring の Kotlin サポート) / **http4k** / **Micronaut** |
| デスクトップ | **Compose Multiplatform Desktop**(Windows/macOS/Linux) / JavaFX + Kotlin |
| Web(フロント) | Kotlin/JS、**Kotlin/Wasm**(実験的)+ Compose HTML |
| スクリプト・ビルド | Gradle Kotlin DSL、`kotlinc -script`、Kotlin Notebook |
| CLI | **Clikt**、kotlinx-cli |
| データ・ML | **KotlinDL**、multik、Kotlin Jupyter |
| Android 以外の業務用途 | Android Things(終了)、kotlin-native による CLI 配布バイナリ |

**実務的な含意**: Kotlin 投資は **Android + サーバ + Desktop + Gradle ビルド** まで横展開可。Java/Spring 経験のあるチームは特に有利。

#### Dart

| 領域 | 主要フレームワーク / 位置付け |
|---|---|
| モバイル | **Flutter** が事実上の独占用途 |
| Web | **Flutter Web**(SPA 的に動作、SEO は制限あり) |
| デスクトップ | **Flutter Desktop**(Windows/macOS/Linux、2022 安定) |
| 組込 | **Flutter Embedded**(車載・POS・サイネージ) |
| サーバ | **Dart Frog**、`dart:io`、`shelf`(用途少) |
| CLI | `pub global activate`、Flutter ツール自作 |

**実務的な含意**: Dart は Flutter 外での活躍が限定的。スキル投資の汎用性は Kotlin/TS より狭い。

#### TypeScript

| 領域 | 主要フレームワーク / 位置付け |
|---|---|
| Web フロントエンド | **React / Vue / Angular / Svelte / Solid** — 事実上の標準 |
| モバイル(JS系) | **React Native / Expo** / **Ionic + Capacitor**(Angular/React/Vue) |
| サーバサイド | **Node.js / Deno / Bun** / NestJS / Express / Hono / Fastify |
| デスクトップ | **Electron**(VS Code/Slack/Discord 等) / **Tauri**(Rust + TS) |
| CLI | oclif、commander、yargs |
| ゲーム | Phaser.js、PixiJS、Babylon.js |
| データ・ML | **TensorFlow.js** |
| 組込 | **Espruino**、**Johnny-Five**、Node-RED(IoT) |

**実務的な含意**: TypeScript は **モバイル・Web・サーバ・デスクトップ・CLI を 1 言語でカバー**可能な最も汎用的な言語。Web 資産のあるチームは特に有利。

#### C#(.NET)

| 領域 | 主要フレームワーク / 位置付け |
|---|---|
| モバイル | **.NET MAUI**(Android/iOS/Win/Mac)/ 旧 Xamarin(EOL) |
| Web | **ASP.NET Core**(MVC/Razor/Blazor Server/Blazor WebAssembly) |
| デスクトップ | **WPF**(Win)、**WinUI 3**、**MAUI**、Avalonia(クロスプラットフォーム) |
| サーバ | **ASP.NET Core**(Web API、gRPC)、Minimal API |
| **ゲーム** | **Unity**(Android/iOS/PC/Console)、Godot(C# 対応) |
| CLI | `dotnet` CLI、System.CommandLine |
| データ・ML | **ML.NET**、ONNX Runtime |
| PowerShell | システム管理・自動化 |

**実務的な含意**: C# は **ゲーム(Unity)領域を押さえている**のが他言語と差別化される点。Microsoft エコシステム社内標準組織で極めて強い。

---

## Part 2. ネイティブ機能別 × FW 対応ライブラリ一覧

### 凡例

- **ネイティブ Android** = Kotlin + Android SDK + Jetpack
- **Flutter** = Flutter SDK + 公式 plugin(`flutter/packages` 等)/ サードパーティ plugin
- **RN / Expo** = React Native コア + `react-native-*` コミュニティ / Expo SDK `expo-*`
- **Ionic** = Web 側(Ionic Framework)+ Capacitor plugin(`@capacitor/*`)/ Cordova 互換プラグイン
- 凡例: ◎=公式・安定 / ○=コミュニティ / △=制約あり / ×=原理的に不可

### 2.1 カメラ・画像

| 機能 | ネイティブ Android | Flutter | RN / Expo | Ionic + Capacitor |
|---|---|---|---|---|
| カメラ撮影 | ◎ **CameraX**(Jetpack)/ Camera2 API | ◎ `camera`、`image_picker` | ◎ `expo-camera` / `react-native-vision-camera` | ◎ `@capacitor/camera` |
| 動画撮影 | ◎ CameraX + MediaRecorder | ◎ `camera` | ◎ Vision Camera、`expo-camera` | ○ capacitor-video-recorder |
| 画像ギャラリー | ◎ ActivityResultContracts.PickVisualMedia | ◎ `image_picker` | ◎ `expo-image-picker` | ◎ `@capacitor/camera`(Photos モード) |
| 画像クロップ | ○ `uCrop` | ○ `image_cropper` | ○ `react-native-image-crop-picker` | ○ Cordova crop |
| 画像表示・キャッシュ | ◎ **Coil**(Jetpack)/ Glide | ◎ `cached_network_image` | ◎ `expo-image`、`react-native-fast-image` | ◎ browser `<img>` + PWA cache |
| 画像圧縮・編集 | ○ BitmapFactory / Canvas | ○ `image`、`flutter_image_compress` | ○ `react-native-image-manipulator` | ○ browser Canvas API |

### 2.2 バーコード / QR

| 機能 | ネイティブ Android | Flutter | RN / Expo | Ionic + Capacitor |
|---|---|---|---|---|
| カメラでのQR/バーコード | ◎ **ML Kit Barcode Scanning**(オフライン) | ◎ `mobile_scanner`、`google_mlkit_barcode_scanning` | ◎ `expo-barcode-scanner`、Vision Camera + frame processors | ◎ `@capacitor-mlkit/barcode-scanning` |
| **Zebra DataWedge(Keystroke)** | ◎(キー入力として `<input>` で受信) | ◎ 同 | ◎ 同 | ◎ 同 |
| **Zebra DataWedge(Intent API)** | ◎ 公式 Intent Broadcast | ◎ `flutter_datawedge`、Zebra 公式サンプル | ○ `react-native-datawedge-intents`(非公式、Darryn Campbell) | ○ Capacitor Intent プラグイン自作 |
| **Zebra EMDK** | ◎ Zebra 公式 SDK | △ `zebra_scanner`(限定) | △ 3rd party ラッパ | × 非推奨 |
| **Honeywell** | ◎ Honeywell Mobility SDK | ○ `honeywell_scanner` | △ カスタム実装 | △ カスタム実装 |
| **Datalogic** | ◎ Datalogic SDK | ○ `datalogic_flutter` | △ カスタム実装 | △ カスタム実装 |
| Web(PWA)カメラ | — | — | — | △ **Barcode Detection API**(Chromium のみ)/ zxing-js |

### 2.3 NFC / RFID

| 機能 | ネイティブ Android | Flutter | RN / Expo | Ionic + Capacitor |
|---|---|---|---|---|
| NFC(NDEF 読み書き) | ◎ `android.nfc` パッケージ | ◎ `nfc_manager` | ◎ `react-native-nfc-manager` | ◎ `@capawesome-team/capacitor-nfc` |
| NFC Tag Host Card Emulation | ◎ HostApduService | △ | △ | △ |
| **UHF RFID**(業務用) | ◎ 各端末メーカー SDK(Zebra RFID SDK 等) | △ 端末別ラッパ | △ 端末別 | △ 端末別 |
| **FeliCa**(交通系)| ◎ `NfcF` | △ | △ | △ |

### 2.4 Bluetooth(Classic / BLE)

| 機能 | ネイティブ Android | Flutter | RN / Expo | Ionic + Capacitor |
|---|---|---|---|---|
| BLE Central | ◎ `android.bluetooth.BluetoothLeScanner` / **Nordic nRF Android Library** | ◎ `flutter_blue_plus`、`flutter_reactive_ble` | ◎ `react-native-ble-plx` | ◎ `@capacitor-community/bluetooth-le` |
| BLE Peripheral | ◎ `BluetoothGattServer` | ○ `flutter_ble_peripheral` | △ `react-native-ble-advertiser` | △ |
| Bluetooth Classic(SPP)| ◎ `BluetoothSocket` | ○ `flutter_bluetooth_serial` | ○ `react-native-bluetooth-classic` | ○ Cordova BT Serial |
| A2DP 音声 | ◎ | × | × | × |

### 2.5 USB(業務用HT周辺機器)

| 機能 | ネイティブ Android | Flutter | RN / Expo | Ionic + Capacitor |
|---|---|---|---|---|
| USB Host(汎用)| ◎ `android.hardware.usb` | ○ `usb_serial` | ○ `react-native-usb-serialport-for-android` | △ |
| USB Accessory | ◎ | △ | △ | △ |
| プリンタ USB 接続 | ◎ ベンダー SDK | ○ ベンダー別 | △ | △ |

### 2.6 位置情報(GPS・ジオフェンス)

| 機能 | ネイティブ Android | Flutter | RN / Expo | Ionic + Capacitor |
|---|---|---|---|---|
| GPS 座標 | ◎ **Fused Location Provider**(GMS)/ LocationManager | ◎ `geolocator`、`location` | ◎ `expo-location` / `react-native-geolocation-service` | ◎ `@capacitor/geolocation` |
| ジオフェンシング | ◎ `GeofencingClient`(GMS) | ○ `native_geofence` | ○ `react-native-geolocation-service` | ○ Capacitor Geofence |
| バックグラウンド位置 | ◎ Foreground Service | ○ `flutter_background_geolocation` | ○ Expo Location BG Task | ○ `@capacitor/background-task` |

### 2.7 センサー(加速度・ジャイロ・磁気・気圧)

| 機能 | ネイティブ Android | Flutter | RN / Expo | Ionic + Capacitor |
|---|---|---|---|---|
| 各種センサー | ◎ `SensorManager` | ◎ `sensors_plus` | ◎ `expo-sensors` | ○ `@capacitor-community/device-motion` |

### 2.8 生体認証

| 機能 | ネイティブ Android | Flutter | RN / Expo | Ionic + Capacitor |
|---|---|---|---|---|
| 指紋・顔認証 | ◎ **Biometric API**(AndroidX) | ◎ `local_auth` | ◎ `expo-local-authentication` | ◎ `@capacitor-community/biometric-auth` |
| CryptoObject 連携 | ◎ Keystore と統合 | ○ 制限あり | ○ 制限あり | △ |

### 2.9 プッシュ通知・ローカル通知

| 機能 | ネイティブ Android | Flutter | RN / Expo | Ionic + Capacitor |
|---|---|---|---|---|
| **FCM(Push)** | ◎ Firebase Messaging | ◎ `firebase_messaging` | ◎ `@react-native-firebase/messaging`、`expo-notifications` | ◎ `@capacitor/push-notifications` |
| APNS(iOS) | — | ◎ 同 | ◎ 同 | ◎ 同 |
| ローカル通知 | ◎ NotificationManager | ◎ `flutter_local_notifications` | ◎ `expo-notifications` | ◎ `@capacitor/local-notifications` |
| Notification Channel(Android 8+)| ◎ | ◎ | ◎ | ◎ |

**HT の GMS 非搭載端末**: Firebase Messaging 利用不可。MQTT / WebSocket で自社サーバからリアルタイム配信、または Amazon SNS 等で HMS(Huawei Mobile Services)代替に直送。

### 2.10 ストレージ・ファイル

| 機能 | ネイティブ Android | Flutter | RN / Expo | Ionic + Capacitor |
|---|---|---|---|---|
| 内部ストレージ | ◎ `Context.getFilesDir()` | ◎ `path_provider` | ◎ `expo-file-system` | ◎ `@capacitor/filesystem` |
| SAF(Storage Access Framework)| ◎ `ACTION_OPEN_DOCUMENT` | ○ `file_picker` | ○ `expo-document-picker` | ○ |
| MediaStore | ◎ | ○ `image_gallery_saver` | ○ | ○ |
| ファイル共有(FileProvider)| ◎ | ◎ `share_plus` | ◎ `expo-sharing` | ◎ `@capacitor/share` |

### 2.11 ローカルDB(SQLite・KVS)

| 機能 | ネイティブ Android | Flutter | RN / Expo | Ionic + Capacitor |
|---|---|---|---|---|
| SQLite | ◎ **Room**(型安全ORM) | ◎ **Drift**(型安全ORM)、`sqflite` | ◎ `expo-sqlite`、`react-native-sqlite-storage`、**WatermelonDB**(オフライン同期型) | ◎ `@capacitor-community/sqlite` |
| 軽量KVS | ◎ **DataStore**(Preferences/Proto) | ◎ `shared_preferences`、**Hive**、**Isar**(高速) | ◎ `@react-native-async-storage/async-storage`、MMKV | ◎ `@capacitor/preferences` |
| オブジェクト DB | ○ ObjectBox(Java/Kotlin) | ◎ **Isar**、ObjectBox | ○ Realm、ObjectBox | ○ PouchDB |
| 暗号化 DB | ◎ SQLCipher + Room | ○ `sqflite_sqlcipher` | ○ Realm encryption、Expo SQLite SQLCipher | ○ capacitor-sqlite(encrypted) |

### 2.12 暗号化・キーストア

| 機能 | ネイティブ Android | Flutter | RN / Expo | Ionic + Capacitor |
|---|---|---|---|---|
| ハードウェアキー保管 | ◎ **Android Keystore System**(TEE/StrongBox) | ○ `flutter_secure_storage` | ○ **`expo-secure-store`**、`react-native-keychain` | ○ `@capacitor-community/secure-storage` |
| 暗号化設定 | ◎ **EncryptedSharedPreferences**(Jetpack Security) | ○ `flutter_secure_storage`(Keystore バックエンド) | ○ 同上 | ○ 同上 |
| 汎用暗号 | ◎ `javax.crypto` / BouncyCastle | ◎ `crypto`、`cryptography` | ◎ `crypto-js`、`expo-crypto` | ◎ WebCrypto API |

### 2.13 バックグラウンド処理

| 機能 | ネイティブ Android | Flutter | RN / Expo | Ionic + Capacitor |
|---|---|---|---|---|
| **WorkManager 相当** | ◎ **WorkManager**(Jetpack、定期・制約・Expedited) | ○ `workmanager`(WorkManager ラッパ) | ○ `expo-background-fetch`、`expo-task-manager` | △ `@capacitor/background-task`(短時間のみ) |
| Foreground Service | ◎ ForegroundService + type 宣言(API 34+) | ○ `flutter_foreground_task` | ○ `react-native-foreground-service` | △ |
| AlarmManager | ◎ | ○ `android_alarm_manager_plus` | ○ Task Manager | △ |
| **長時間稼働(8h)** | ◎(WorkManager + Doze 耐性) | ○ | ○(制約あり) | **△**(WebView BG 制限) |

### 2.14 共有・Intent・ディープリンク

| 機能 | ネイティブ Android | Flutter | RN / Expo | Ionic + Capacitor |
|---|---|---|---|---|
| Share Sheet | ◎ ACTION_SEND | ◎ `share_plus` | ◎ `expo-sharing`、`react-native-share` | ◎ `@capacitor/share` |
| カスタム Intent 送受信 | ◎ | ◎ `intent_plus`、`receive_sharing_intent` | ○ `react-native-intent-launcher` | ○ `@capacitor-community/intent` |
| App Links / Universal Links | ◎ | ◎ `app_links`、`uni_links` | ◎ Expo Linking / `react-native-app-link` | ◎ `@capacitor/app`、Deep Linking |

### 2.15 HTTP / API 通信

| 機能 | ネイティブ Android | Flutter | RN / Expo | Ionic + Capacitor |
|---|---|---|---|---|
| HTTP クライアント | ◎ **Retrofit + OkHttp** / **Ktor Client** | ◎ `dio`、`http` | ◎ fetch / axios | ◎ fetch / axios / `@capacitor/http` |
| クライアント証明書 | ◎ Keychain + OkHttp SSLSocketFactory | ○ `dio` + `http_certificate_pinning` | ○ `react-native-ssl-pinning` | ○ Capacitor HTTP plugin |
| 証明書ピン留め | ◎ OkHttp CertificatePinner | ○ | ○ | ○ |

### 2.16 WebSocket / リアルタイム通信

| 機能 | ネイティブ Android | Flutter | RN / Expo | Ionic + Capacitor |
|---|---|---|---|---|
| WebSocket | ◎ OkHttp WebSocket / Ktor WebSockets | ◎ `web_socket_channel` | ◎ ブラウザ標準 / `socket.io-client` | ◎ ブラウザ標準 |
| Socket.IO | ◎ `socket.io-client-java` | ◎ `socket_io_client` | ◎ `socket.io-client` | ◎ 同 |
| MQTT | ◎ Paho / HiveMQ Client | ◎ `mqtt_client` | ◎ `react-native-mqtt` | ◎ `mqtt` npm |

### 2.17 gRPC

| 機能 | ネイティブ Android | Flutter | RN / Expo | Ionic + Capacitor |
|---|---|---|---|---|
| gRPC クライアント | ◎ `grpc-okhttp` + `grpc-kotlin-stub` | ◎ `grpc-dart` | ○ `grpc-web`(Web版、制約あり)/ `@improbable-eng/grpc-web` | ○ gRPC-Web |

**注**: gRPC は HTTP/2 Trailer headers を使うため、ブラウザ/WebView からは直接利用不可。Web 系は **gRPC-Web**(サーバ側 Envoy 等でプロトコル変換)が必須。

### 2.18 印刷(ZPL・PDF・Bluetooth プリンタ)

| 機能 | ネイティブ Android | Flutter | RN / Expo | Ionic + Capacitor |
|---|---|---|---|---|
| **Zebra ZPL プリンタ** | ◎ **Zebra Link-OS SDK** | ○ `zsdk_flutter` | △ コミュニティ | △ カスタム |
| Bluetooth プリンタ | ◎ 各ベンダー SDK | ○ 同 | △ | △ |
| PDF 生成 | ◎ `PdfDocument`(API 19+)/ iText | ◎ `pdf`、`printing` | ◎ `react-native-print`、`expo-print` | ◎ jsPDF、pdfmake |
| Android 印刷フレームワーク | ◎ PrintManager | ○ `printing` | ○ | ○ |
| AirPrint / Google Cloud Print(廃止) | ◎ | ○ | ○ | ○ |

### 2.19 MDM 連携

| 機能 | ネイティブ Android | Flutter | RN / Expo | Ionic + Capacitor |
|---|---|---|---|---|
| **App Config(Managed Configuration)** | ◎ **RestrictionsManager**(公式) | ○ `android_managed_config` | ○ `react-native-managed-app-config` | ○ `@capawesome-team/capacitor-managed-configurations` |
| Device Owner / Lock Task | ◎ DevicePolicyManager / `startLockTask()` | ○ プラグイン経由 | ○ プラグイン経由 | ○ プラグイン経由 |
| Android Enterprise 統合 | ◎ | ○ | ○ | ○ |

### 2.20 クラッシュレポート・アナリティクス

| 機能 | ネイティブ Android | Flutter | RN / Expo | Ionic + Capacitor |
|---|---|---|---|---|
| Firebase Crashlytics | ◎ | ◎ `firebase_crashlytics` | ◎ `@react-native-firebase/crashlytics` | ◎ `@capacitor-community/firebase-crashlytics` |
| Sentry | ◎ `sentry-android` | ◎ `sentry_flutter` | ◎ `@sentry/react-native` | ◎ `@sentry/capacitor` |
| Firebase Analytics | ◎ | ◎ | ◎ | ◎ |
| **GMS 非搭載端末**(Zebra 等) | → Sentry / 自社収集に寄せる | 同 | 同 | 同 |

### 2.21 ML・OCR(オフライン推論)

| 機能 | ネイティブ Android | Flutter | RN / Expo | Ionic + Capacitor |
|---|---|---|---|---|
| **ML Kit**(Google、オフライン含む)| ◎ | ◎ `google_mlkit_*` シリーズ | ○ `@react-native-ml-kit/*`、`@infinitered/react-native-mlkit-*` | ◎ `@capacitor-mlkit/*` |
| TensorFlow Lite | ◎ TFLite Android | ◎ `tflite_flutter` | ○ `react-native-fast-tflite` | △ |
| ONNX Runtime | ◎ onnxruntime-android | ○ `onnxruntime` | ○ | △ |
| **OCR**(ML Kit Text Recognition)| ◎ | ◎ | ○ | ◎ |

### 2.22 アクセシビリティ

| 機能 | ネイティブ Android | Flutter | RN / Expo | Ionic + Capacitor |
|---|---|---|---|---|
| TalkBack 対応 | ◎ `contentDescription` | ◎ Semantics ウィジェット | ◎ Accessibility Props | ◎(HTML 属性そのまま) |
| 音声認識 | ◎ `SpeechRecognizer` | ○ `speech_to_text` | ○ `@react-native-voice/voice`、`expo-speech-recognizer` | ○ Web Speech API |
| テキスト読み上げ | ◎ `TextToSpeech` | ○ `flutter_tts` | ◎ `expo-speech` | ◎ Web Speech API |

### 2.23 その他業務アプリ頻出

| 機能 | ネイティブ Android | Flutter | RN / Expo | Ionic + Capacitor |
|---|---|---|---|---|
| クリップボード | ◎ ClipboardManager | ◎ `clipboard`、`flutter/services` | ◎ `@react-native-clipboard/clipboard` | ◎ `@capacitor/clipboard` |
| デバイス情報 | ◎ Build、TelephonyManager | ◎ `device_info_plus` | ◎ `expo-device` | ◎ `@capacitor/device` |
| 電源・WakeLock | ◎ PowerManager | ◎ `wakelock_plus` | ◎ `expo-keep-awake` | ◎ `@capacitor-community/keep-awake` |
| Vibrator | ◎ Vibrator | ◎ `vibration` | ◎ `expo-haptics` | ◎ `@capacitor/haptics` |
| WebView 埋め込み | ◎ WebView + JavaScriptInterface | ◎ `webview_flutter`、`flutter_inappwebview` | ◎ `react-native-webview` | ◎ Capacitor 自体が WebView ベース |
| QR コード生成 | ○ ZXing | ◎ `qr_flutter` | ◎ `react-native-qrcode-svg` | ◎ `qrcode` npm |
| Widget(ホーム画面) | ◎ AppWidgetProvider + Glance | ○ `home_widget`(制約あり) | ○ `react-native-android-widget` | × |
| アプリ内課金(IAP) | ◎ Google Play Billing | ◎ `in_app_purchase` | ◎ `expo-in-app-purchases` (廃止予定)、RevenueCat | ◎ RevenueCat、Stripe |

---

## Part 3. HT 業務アプリで特に重要なネイティブ連携

### 3.1 HT スキャナ統合の決定木

```
Q: スキャン結果を EditText に入れるだけで OK か?
├─ YES → DataWedge Keyboard Wedge モードで全 FW 動作可(実装不要)
└─ NO  → 以下へ

Q: 詳細制御(MultiBarcode / プロファイル切替 / メタ情報)が必要か?
├─ YES → DataWedge Intent API
│        ├─ ネイティブ Android: ◎ 直接 BroadcastReceiver
│        ├─ Flutter: ◎ Zebra 公式デモ、flutter_datawedge
│        ├─ RN: ○ darryncampbell/DataWedgeIntents(Expo は Dev Client 必須)
│        └─ Ionic: ○ Capacitor Intent plugin 自作
└─ NO  → Keyboard Wedge で足りる(上の YES パス)

Q: 低レベル制御(EMDK 専用機能、RFID 連携等)が必要か?
└─ YES → ネイティブ Android 必須(他FWでは不可/非推奨)
```

### 3.2 典型 HT 業務アプリのネイティブ機能依存度

| 機能 | 必須度 | 影響 FW 選定 |
|---|---|---|
| バーコードスキャン(Wedge)| ★★★ | 影響なし |
| バーコードスキャン(Intent)| ★★☆ | Expo Managed 不可 |
| 物理ファンクションキー | ★☆☆(業務要件次第)| Ionic 不利 |
| オフライン DB(SQLite) | ★★★ | 影響なし |
| オフライン同期(WorkManager)| ★★★ | Ionic 不利、他は可 |
| MDM App Config | ★★☆ | プラグインで対応可、ネイティブが最確実 |
| Kiosk / Device Owner | ★☆☆ | プラグインで対応可 |
| プリンタ(ZPL) | ★☆☆〜★★☆ | ネイティブ or ベンダー SDK 同梱 FW |
| クライアント証明書認証 | ★★☆ | 影響なし |

---

## Part 4. 参照リンク

### 4.1 Android 公式(ネイティブ)

| 対象 | URL |
|---|---|
| Android Developers Guides | https://developer.android.com/guide |
| CameraX | https://developer.android.com/training/camerax |
| ML Kit | https://developers.google.com/ml-kit |
| Biometric API | https://developer.android.com/training/sign-in/biometric-auth |
| WorkManager | https://developer.android.com/topic/libraries/architecture/workmanager |
| Room | https://developer.android.com/training/data-storage/room |
| DataStore | https://developer.android.com/topic/libraries/architecture/datastore |
| Managed Configurations | https://developer.android.com/work/managed-configurations |
| Jetpack Security | https://developer.android.com/topic/security/data |

### 4.2 Flutter パッケージ

| 対象 | URL |
|---|---|
| pub.dev(公式レジストリ) | https://pub.dev/ |
| Flutter Favorites | https://docs.flutter.dev/packages-and-plugins/favorites |
| flutter_datawedge | https://pub.dev/packages/flutter_datawedge |
| Zebra Flutter 公式サンプル | https://github.com/ZebraDevs |

### 4.3 React Native / Expo

| 対象 | URL |
|---|---|
| Expo SDK | https://docs.expo.dev/versions/latest/ |
| React Native Directory | https://reactnative.directory/ |
| Vision Camera | https://react-native-vision-camera.com/ |
| @react-native-firebase | https://rnfirebase.io/ |
| React Native DataWedge Intents | https://github.com/darryncampbell/react-native-datawedge-intents |

### 4.4 Capacitor プラグイン

| 対象 | URL |
|---|---|
| Capacitor Plugins 公式 | https://capacitorjs.com/docs/plugins |
| Capacitor Community | https://github.com/capacitor-community |
| Capawesome Plugins | https://capawesome.io/plugins/ |

### 4.5 Zebra / HT

| 対象 | URL |
|---|---|
| Zebra TechDocs | https://techdocs.zebra.com/ |
| Zebra DataWedge | https://techdocs.zebra.com/datawedge/latest/guide/about/ |
| Zebra Link-OS Printer SDK | https://techdocs.zebra.com/link-os/latest/ |
| Honeywell Developer | https://developer.honeywell.com/ |
| Datalogic Developer Portal | https://developer.datalogic.com/ |

### 4.6 言語の活用範囲(横断)

| 対象 | URL |
|---|---|
| Ktor(Kotlin サーバ) | https://ktor.io/ |
| Kotlin Multiplatform | https://kotlinlang.org/docs/multiplatform.html |
| Compose Multiplatform | https://www.jetbrains.com/lp/compose-multiplatform/ |
| Dart Frog | https://dartfrog.vgv.dev/ |
| Flutter Web/Desktop/Embedded | https://flutter.dev/multi-platform |
| Node.js | https://nodejs.org/ |
| Deno | https://deno.com/ |
| Bun | https://bun.sh/ |
| Electron | https://www.electronjs.org/ |
| Tauri | https://tauri.app/ |
| ASP.NET Core | https://dotnet.microsoft.com/apps/aspnet |
| Unity | https://unity.com/ |

---

## 本ファイルの位置付け

- `framework_profiles.md` の各 FW プロファイルと組合せて読むことを想定
- 「**この機能は使いたい。どの FW でどんなライブラリで実装できるか**」を俯瞰する用途
- 業務アプリで頻出機能に絞っているため、ゲーム・AR/VR 等は対象外
- ライブラリの具体的実装パターンは、ネイティブ Android のみ `profile_android_native.md` で深掘り済み

---

## 更新履歴

- **2026-04-23**: 初版作成。言語活用範囲マトリクス + 23 カテゴリのネイティブ機能対応表を整備。
