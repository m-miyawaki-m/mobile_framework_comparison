# PWA / Capacitor / ネイティブ のレイヤ境界と SDK アクセス可能性

「**AAR を Capacitor プラグイン化して PWA で動かしたい**」という構成案が技術的に成立しない理由を、配信形態・実行ランタイム・レイヤ境界の観点から原理的に整理する。具体例として **外付け BT 型 UHF RFID/バーコードスキャナ**(セパレート型・ベンダー SDK 提供型、Android/iOS/Windows の各 SDK をベンダーが配布する一般的形態)を用いる。

- 情報基準日: 2026 年 5 月時点
- 関連資料:
  - レイヤ構造図: [`architecture_pwa.puml`](./architecture_pwa.puml) / [`architecture_hybrid.puml`](./architecture_hybrid.puml) / [`architecture_native.puml`](./architecture_native.puml)
  - 項目別評価: [`pwa_vs_native_criteria.md`](./pwa_vs_native_criteria.md)(IV 章 HW 機能・XV 章 ハイブリッド/中間形態)
  - Ionic + Capacitor の詳細: [`../framework_comparison/profile_ionic_capacitor.md`](../framework_comparison/profile_ionic_capacitor.md)

---

## 0. 結論(TL;DR)

| 構成 | AAR をロードできるか | ベンダー SDK 経由で外部端末を操作できるか |
|---|---|---|
| **純 PWA**(HTTPS 配信、ブラウザ実行)| **× 不可** | **× 不可**(Web API の範囲のみ)|
| **Capacitor APK**(ストア/MDM 配信、APK ホスト実行)| **◎ 可**(Gradle で組込)| **◎ 可**(カスタムプラグイン経由)|
| **Capacitor の "Web ビルド" を PWA として配信** | **× 不可**(Bridge 不在)| **× 不可**(Plugin 層に到達せず)|

**核心**: 「Capacitor を採用する」=「ハイブリッド対応コードベースになる」だが、**実際に AAR/SDK が動くのは APK としてビルド・配信した時だけ**。同じコードを PWA として配信した瞬間に、Plugin 層と Bridge は消える。

---

## 1. 用語の整理 — 配信形態 ≠ 実行ランタイム

混同されやすい 2 つの軸を分けて考える。

### 1.1 配信形態(どこから取得するか)

| 配信形態 | 取得経路 | 成果物 |
|---|---|---|
| **PWA** | HTTPS URL アクセス | HTML/JS/CSS/manifest.json/Service Worker |
| **APK / IPA**(ストア)| Google Play / App Store | 署名付き APK / IPA |
| **APK 直配布**(MDM 等)| Intune / SOTI / 自前 MDM | 同上 |
| **TWA**(Trusted Web Activity)| Play | APK 殻 + URL ロード |

### 1.2 実行ランタイム(何が実行するか)

| 実行ランタイム | エンジン | 実行可能なコード |
|---|---|---|
| **ブラウザ**(Chrome / Safari / Edge)| V8 / JavaScriptCore + Blink/WebKit | JavaScript / WebAssembly / HTML / CSS |
| **WebView**(Android System WebView / WKWebView)| 同上 | 同上 |
| **Capacitor APK ホスト** | Android Runtime(ART)+ WebView | Java / Kotlin / **AAR 内のクラス** + JS |
| **純ネイティブ APK** | ART | Java / Kotlin + AAR + JNI 経由の C/C++ |

### 1.3 Capacitor が出力する 3 種の成果物

`npx cap` コマンドが生成するもの:

```
ionic build / vite build
        │
        ▼
   dist/ (Web アセット = HTML/JS/CSS/manifest)
        │
        ├──→ そのまま HTTPS で配信  → 【PWA】 (ブラウザ実行)
        │
        ├── npx cap sync android
        │       │
        │       ▼
        │   android/ (Gradle プロジェクト、dist/ を assets/ に同梱)
        │       │
        │       ▼
        │   ./gradlew assembleRelease → APK / AAB
        │       │
        │       ▼
        │   Play / MDM 配信  → 【Capacitor APK】 (APK ホスト + WebView 実行)
        │
        └── npx cap sync ios → ios/ (Xcode) → IPA → App Store
```

**重要**: dist/ を PWA として配信するルートと、APK として配信するルートは **別物の成果物・別物のランタイム**。同じ TypeScript ソースから派生していても、**PWA 側には Capacitor Bridge も Plugin 層も含まれない**。

---

## 2. レイヤ構造の比較

`architecture_pwa.puml` / `architecture_hybrid.puml` の図を文章で表現する。

### 2.1 純 PWA(ブラウザ実行)

```
┌─────────────────────────────────────────────┐
│ UI Layer        Web Components / SFC / JSX  │
├─────────────────────────────────────────────┤
│ Domain Layer    純 TS UseCase               │
├─────────────────────────────────────────────┤
│ Data Layer      Repository + DataSources    │
│                  ├── fetch / axios          │
│                  ├── IndexedDB              │
│                  └── Cache API (SW)         │
├═════════════════════════════════════════════┤  ← サンドボックス境界(超えられない)
│ Platform Runtime(ブラウザ)                │
│   - V8 / JavaScriptCore                     │
│   - Blink / WebKit                          │
│   - Service Worker                          │
├─────────────────────────────────────────────┤
│ Web APIs(ブラウザが許す範囲のみ)          │
│   getUserMedia, Geolocation, WebAuthn,      │
│   Web Bluetooth(BLE GATT)*,                │
│   Web NFC*, Web USB*, Web Serial*,          │
│   BarcodeDetector* …                        │
│   *: Chromium のみ、Safari 不可             │
└─────────────────────────────────────────────┘
        ↓
   HW(カメラ・GPS・ブラウザが標準化した BT/USB/NFC)
```

**到達不能**: ベンダー提供の AAR、Android Intent、BroadcastReceiver、Foreground Service、Android Keystore、WorkManager、JNI、ネイティブ共有ライブラリ(.so)。

### 2.2 Capacitor APK(APK ホスト + WebView 実行)

```
┌─────────────────────────────────────────────┐
│ UI Layer        (純 PWA と同じ JS/TS)       │
├─────────────────────────────────────────────┤
│ Domain Layer    (同上)                      │
├─────────────────────────────────────────────┤
│ Data Layer      (同上、ただし DataSource の │
│                  一部が Bridge 経由)        │
├─────────────────────────────────────────────┤
│ Capacitor Bridge (JS ↔ Native の非同期 IPC) │
│   JS側: window.Capacitor.Plugins.*          │
│   Native側: BridgeWebViewClient(Java)       │
├─────────────────────────────────────────────┤
│ Plugin 層 (Kotlin / Swift)                  │
│   - 公式 @capacitor/* (Camera, Geolocation) │
│   - コミュニティ @capacitor-community/*     │
│   - **カスタムプラグイン**(自社実装)      │
├─────────────────────────────────────────────┤
│ ベンダー AAR / SDK (Java/Kotlin/JNI)        │
│   - 外付けスキャナ AAR(ベンダー提供)      │
│   - Zebra DataWedge / EMDK                  │
│   - Honeywell SDK                           │
├─────────────────────────────────────────────┤
│ Android Runtime (ART) + Android SDK         │
│   - Intent / BroadcastReceiver              │
│   - BluetoothAdapter / GATT                 │
│   - UsbManager / NfcAdapter                 │
│   - WorkManager / Foreground Service        │
│   - Android Keystore                        │
└─────────────────────────────────────────────┘
        ↓
   HW(全機能 + 外付け端末 SDK 経由)
```

**到達範囲**: PWA で到達不能なすべての Android SDK 機能。ベンダー AAR は Gradle 依存として APK に同梱され、ART のクラスローダにロードされる。

### 2.3 純ネイティブ APK

```
┌─────────────────────────────────────────────┐
│ UI Layer        Jetpack Compose             │
├─────────────────────────────────────────────┤
│ Domain Layer    Kotlin UseCase              │
├─────────────────────────────────────────────┤
│ Data Layer      Repository + DataSources    │
├─────────────────────────────────────────────┤
│ ベンダー AAR / SDK (Java/Kotlin/JNI)        │
├─────────────────────────────────────────────┤
│ Android Runtime + Android SDK               │
└─────────────────────────────────────────────┘
```

Capacitor APK との違いは **Bridge 層と Plugin 層がないこと**(中間レイヤなし、直接 SDK 呼び出し)。AAR/SDK へのアクセスは Capacitor APK と同じ。

### 2.4 本書のレイヤ図と Web 業界の慣習との差(重要な注記)

#### 2.4.1 本書のレイヤ図は Android 公式アーキテクチャを Web 側に投影したもの

[`architecture_pwa.puml`](./architecture_pwa.puml) の冒頭コメントにも明記されている通り、本書および各 PUML が採用する **UI Layer / Domain Layer / Data Layer** という 3 層構造は、[Android 公式『推奨アーキテクチャ』](https://developer.android.com/topic/architecture)を Web 側に投影したもの。3 構成(PWA / Capacitor APK / Native APK)を **同じ尺度で比較できるようにするための分析的な投影** であり、**Web/PWA 業界のデファクトスタンダードではない**。

「投影」は意図的:

- 本リポジトリの読者(Android 業務アプリ視点)が同じ語彙で 3 構成を読み比べられる
- HW アクセス境界(どこまで Native/SDK に届くか)の比較が層単位で素直になる
- 「Domain は Optional」と明記して PWA で省略してよいと示唆している

ただし「PWA を作るときも厳格に 3 層に分けるべき」という規範ではない点に注意。

#### 2.4.2 Web/PWA 業界の実際のレイヤ慣習

Web/PWA に統一的な「層モデル」は存在しない。代わりにいくつかの慣習的パターンがある。

##### パターン A. Components + Stores + Services(最も普及、Web プロジェクトの 7〜8 割)

```
src/
├── components/        UI 部品(再利用可能)
├── pages/ or views/   ルート対応の画面
├── composables/ (Vue) / hooks/ (React) / services/ (Angular)
│                      状態を持つロジック・副作用
├── stores/            Pinia / Redux / Zustand / NgRx
├── api/ or services/  fetch ラッパ・API クライアント
├── utils/             純関数
└── types/             TS 型定義
```

責務別フォルダ分けで、明示的な「縦の階層」は意識されない。本書の 3 層図に対応させると:

- UI Layer ≈ `components/` + `pages/`
- Domain Layer ≈ ほぼ存在しない(Hook / Store / Service に吸収)
- Data Layer ≈ `stores/` + `api/`

##### パターン B. Feature-based(中〜大規模で増加)

```
src/
├── features/
│   ├── inventory/
│   │   ├── components/
│   │   ├── api.ts
│   │   ├── store.ts
│   │   └── types.ts
│   ├── login/
│   └── order/
├── shared/
└── app/
```

機能単位で凝集。Redux Toolkit / Feature-Sliced Design(FSD)が推進。**横の階層(層)より縦の凝集(機能)** を優先する文化。

##### パターン C. Clean Architecture / Hexagonal(大規模・エンタープライズ・Angular で時々)

```
src/
├── presentation/      UI 層(Components)
├── application/       UseCase 層
├── domain/            Domain Entities + Interfaces
└── infrastructure/    API / DB 実装(Adapters)
```

ここで初めて「Domain Layer」が独立して登場し、本書の 3 層図とほぼ対応する。ただし **Web プロジェクトで採用は少数派**(複雑な業務ロジック・大規模 SaaS・厳密 DDD 採用組織のみ)。Vue/React コミュニティでは主流ではなく、Angular でやや見かける程度。

##### パターン D. Server Component / BFF 時代(Next.js App Router / Nuxt 3 / Remix)

Server Component / Server Action がデータ取得を担うため、クライアント側はインタラクションに薄く、**Data Layer 概念がクライアントから押し出される**。従来の 3 層モデルが当てはまりにくい新潮流。

#### 2.4.3 「Domain Layer (Optional)」は Web で意味があるか

| 観点 | Android | Web / PWA |
|---|---|---|
| Domain Layer の認知度 | ◎ 公式推奨 | △ 一部の Clean Architecture 派のみ |
| Domain Layer の実装 | UseCase クラスとして明示 | Hooks / Composables / Services に吸収 |
| 「Repository」の用語 | ◎ 標準 | △ Angular で時々、React/Vue では `api.ts` / `services/` 等 |
| 階層的レイヤ図の流通度 | 高(公式図で広く流通)| 低(コミュニティによって解釈差大) |
| ViewModel ↔ UseCase ↔ Repository の連鎖 | 標準実装パターン | 稀(Hook / Store が直接 fetch するのが普通) |

つまり Web で「ViewModel が UseCase を呼び UseCase が Repository を呼ぶ」のような厳格な階層運用は稀。**Hook / Store が直接 fetch を叩く** のが普通で、Domain Layer は省略されることが多い。

#### 2.4.4 本書の図の読み方(まとめ)

1. 各構成の **HW アクセス境界**(どこまで Native/SDK に届くか)を比較するための投影
2. レイヤの数や順序自体はリファレンス実装ではなく、**読者の慣れた言葉(Android アーキテクチャ用語)で 3 構成を並べた説明モデル**
3. 実際の PWA / Capacitor プロジェクトの構造は上記 A〜D のいずれかで実装されるのが一般的
4. **「Domain Layer を入れないと PWA が破綻する」というメッセージではない**(Optional は本当に Optional)
5. 実装に着手するときは、本書の図で **境界の議論** を理解した後、**実装上のフォルダ構造はパターン A か B を起点に選ぶ** のが現実的

---

## 3. AAR と Capacitor Plugin が「どのレイヤに存在するか」

| 成果物 | 物理的な所在 | ロードする主体 |
|---|---|---|
| **AAR**(`vendor-scanner-sdk.aar` 等)| APK 内 `classes.dex` に展開(Gradle が AAR を解凍 → クラスを dex 化 → APK に同梱)| Android Runtime のクラスローダ(`DexClassLoader`)|
| **Capacitor カスタムプラグイン**(Kotlin 実装)| 同上 | 同上 + Capacitor の `BridgeActivity` がリフレクション登録 |
| **Capacitor Bridge ネイティブ側** | APK 内(Capacitor の Android ライブラリ) | 同上 |
| **Capacitor Bridge JS 側** | dist/ 内 `@capacitor/core` バンドル | JS エンジン(V8 等) |
| **Plugin の Web 実装**(あれば、例: `MyPlugin.web.ts`)| dist/ 内 | JS エンジン |

ここで決定的なのは:

> **AAR の中身を「実行する」には Android Runtime(ART)が必要であり、ART は APK としてインストールされた時にしか起動しない。**

PWA = HTTPS で配信 = ブラウザがダウンロード = JavaScript エンジンに渡る。**この経路に ART は登場しない**。したがって AAR は物理的にも論理的にも到達不可能。

---

## 4. 「PWA で AAR を動かす」が不可能な 4 つの理由

### 4.1 配布物に AAR が含まれない

PWA の配布物は `dist/`(HTML/JS/CSS)のみ。Capacitor の `npx cap sync android` で AAR を取り込むのは `android/` プロジェクトであって、`dist/` には一切影響しない。**配布した瞬間に AAR は端末に存在しない**。

### 4.2 ブラウザに JVM/ART が無い

ブラウザのランタイムは JavaScript エンジン + WebAssembly。Java バイトコード(`.class` / `.dex`)を実行する仕組みを持たない。仮に AAR をブラウザに「配布」できたとしても、ロードできない。

> 参考: WebAssembly で Android Runtime を実装する研究プロジェクトは存在するが(CheerpJ 等)、商用利用はほぼ無く、Android SDK API(`Context`, `BluetoothAdapter` 等)を Web API にマッピングする手段がない。

### 4.3 Capacitor Bridge は APK 内のみで動作する

Capacitor Bridge の Native 側 = `com.getcapacitor.BridgeActivity` という **Android Activity**。Activity は ART が起動して生成するもの。ブラウザには Activity の概念がない。

JS 側 Bridge(`window.Capacitor.Plugins.*`)は dist/ に含まれて PWA でも存在するが、`Capacitor.getPlatform()` は `web` を返し、**Plugin 呼び出しは "Not implemented on web" でリジェクトされる**(Web 実装が登録されていない限り)。

### 4.4 ベンダー SDK は Web 実装を提供しない

外付けスキャナの SDK / Zebra EMDK 等は **AAR(Java/Kotlin)+ ネイティブ共有ライブラリ(`.so`)** の組合せで、Android API への深い依存を持つ:

- `Context.registerReceiver()` を内部で呼ぶ → ブラウザに Context は無い
- `BluetoothGatt` の callback を内部で受ける → ブラウザは標準 GATT API しか公開しない
- JNI 経由で C/C++ ライブラリを呼ぶ → ブラウザは `.so` をロード不可

これらを JS/Web API でエミュレートする道は無い。ベンダーが将来「Web SDK 版」を出さない限り、Web では SDK を使えない。

---

## 5. ケーススタディ — 外付け BT 型 UHF RFID/バーコードスキャナ

### 5.1 想定する端末の特性(業界一般像)

- **形態**: 外付け BT セパレート型(スマートデバイスとペアリングして使う形)
- **読取**: UHF 920MHz 帯 RFID(高速タグ読取・業務想定中距離)+ 2D バーコード(QR/DataMatrix/1次元)
- **接続**: Bluetooth 5.x(Classic + LE)、NFC(ペアリング補助)、USB-C(充電/通信)
- **SDK 提供**: Android(AAR)/ iOS / Windows、ベンダー無償配布が一般的
- **動作要件**: 必ずホスト端末(スマホ/タブレット/PC)とのペアリングが必要、独立動作不可

### 5.2 「AAR を Capacitor 化して PWA で」は何故無理か

上記第 4 節の 4 つの理由がすべて該当する。要約すると:

| 段階 | 問題 |
|---|---|
| 配布 | PWA 配布物に AAR を入れる仕組みがない |
| ロード | ブラウザに AAR をロードする仕組みがない |
| 実行 | SDK 内の `Context.registerReceiver` 等を満たすランタイムがない |
| 通信 | ベンダー独自の BT GATT プロトコルは SDK 内に隠蔽、Web Bluetooth で代替不可 |

「Web Bluetooth で当該端末と直接話せばいいのでは?」という発想は理論上ありうるが、現実には:

- 対象端末の GATT サービス UUID・キャラクタリスティック構造・データフレーム形式は公開されていない(SDK 内に隠される)
- 仮に公開されていても、UHF RFID 制御(アンテナ出力、フィルタ、トリガ、複数タグ同時読取)を Web Bluetooth から実装するのは事実上 SDK の再実装
- Safari は Web Bluetooth 自体に非対応 → iOS PWA で不可

### 5.3 実現可能な構成

| 案 | 概要 | 対象端末機能 | PWA 同居 |
|---|---|---|---|
| **A. Capacitor APK + カスタムプラグイン** | AAR を `android/app/libs/` に置き、`@CapacitorPlugin` で JS API を公開 | ◎ フル機能 | ハイブリッドコードベース内で別ビルド(PWA は外付け端末非対応で動く) |
| **B. 純ネイティブ APK**(Kotlin)| AAR を直接 build.gradle で取り込み、Compose UI から呼ぶ | ◎ フル機能 | × PWA とコードベース分離 |
| **C. PWA のみ**(外付け端末を端末カメラに置換)| 端末カメラ + html5-qrcode/BarcodeDetector で QR のみ | バーコードのみ(端末カメラ品質依存)、UHF RFID は完全不可 | ◎ PWA 単独 |

本件で対象端末の主機能(UHF RFID)が要件なら **案 A または B が必須**。「PWA で当該外付けスキャナを使う」は要件として両立しない。

### 5.4 案 A のレイヤ図

```
┌────────────────────────────────┐
│ UI Layer (Vue / Pinia)         │  ← PWA と共通(Ionic コンポーネント)
├────────────────────────────────┤
│ Capacitor Bridge (JS 側)       │
│  if (isNativePlatform()) {     │  ← 分岐
│    Scanner.startInventory()    │
│  } else {                      │
│    /* PWA: 機能無効化 */       │
│  }                             │
├════════════════════════════════┤  ← APK ビルド時のみ存在
│ Capacitor Bridge (Native 側)   │
├────────────────────────────────┤
│ カスタム Plugin (Kotlin)       │
│  @CapacitorPlugin("Scanner")    │
├────────────────────────────────┤
│ ベンダー提供 AAR                │
├────────────────────────────────┤
│ Android SDK                    │
│  (BluetoothAdapter / GATT)     │
└────────────────────────────────┘
        ↓ BT 5.0
   [外付けスキャナ端末]
```

PWA 配信時はこの図の点線より下が **存在しない** ため、UI 層の分岐で「PWA は外付けスキャナ不可」を明示する設計になる。

---

## 6. よくある誤解(FAQ)

### Q1. Capacitor で作ったら自動的に PWA でもネイティブでも動くのでは?

**動くのは UI 層と Web API 範囲の機能だけ**。Plugin 層を呼ぶコードは PWA では `Not implemented on web` で失敗する。両方で動かしたい機能は、Plugin 側に **Web 実装**(`.web.ts`)を別途用意する必要があり、ベンダー SDK 系には事実上書けない。

### Q2. PWA Elements(`@ionic/pwa-elements`)で何とかならないか?

PWA Elements は Camera / Modal / Toast 等、**Web API でエミュレート可能な一部 Plugin に限り** Web フォールバックを提供する。BT/USB/RFID/ベンダー SDK 系は対象外。

### Q3. TWA(Trusted Web Activity)なら?

TWA は APK 内で Chrome Custom Tabs を起動して PWA を表示する仕組み。実体は **ブラウザを開いているだけ** で、APK 内のコードに JS から到達する仕組みは持たない(Web Share Target 等の限定的 Intent 連携を除く)。SDK アクセスには不向き。

### Q4. WebAssembly で SDK を変換できないか?

理論上 Kotlin → Wasm はある(Kotlin/Wasm)が、Android SDK API(`Context`, `BluetoothAdapter`, `Intent` 等)に依存するコードは Wasm 側で動かす方法がない(Android Framework が Wasm に存在しない)。**ベンダー SDK のような Android Framework 依存コードは Wasm 化できない**。

### Q5. Service Worker でバックグラウンドから SDK を叩けないか?

Service Worker は **ブラウザのサンドボックス内**で動く。AAR/SDK へのアクセス手段は持たず、できることは fetch・Cache API・Push 受信 等のブラウザ API のみ。

### Q6. 「Capacitor で PWA」と公式が言っている = SDK 使えるのでは?

Capacitor 公式の「PWA サポート」は「同一コードベースから PWA も出力できる」という意味で、**PWA 側でも Plugin が動くという意味ではない**。`Capacitor.getPlatform() === 'web'` での分岐実装は開発者の責任。

### Q7. Android 12+ の Trusted Web Activity / Custom Tab で改善した?

通信プロトコル(Digital Asset Links)や UI 体験の改善はあるが、SDK アクセス可否の構造は変わらない。**ブラウザサンドボックスの外には出られない**という原則は不変。

---

## 7. 設計判断のための原則

PWA 採用の可否を判断するときの 3 原則:

### 原則 1. 「配信形態」と「実行ランタイム」を分けて考える

Capacitor を採用しても、配信形態によって実行ランタイムが変わる:

- HTTPS で配信 = ブラウザ実行 = PWA = SDK 不可
- ストア/MDM で APK 配信 = APK ホスト実行 = SDK 可

「Capacitor を使えば PWA でも SDK 使える」は誤り。

### 原則 2. Bridge と Plugin 層は「APK ビルド時にのみ実体化する」

`@capacitor/core` の JS API は dist/ に含まれるが、その先の Plugin 実装は **AAR/Kotlin** であり、APK 内にしか存在しない。PWA 配信時は Plugin 層がないため、JS API 呼び出しは pseudo-呼び出しでエラーになる。

### 原則 3. ベンダー SDK は「動作前提のランタイム」を強制する

外付けスキャナ SDK は Android Runtime + Bluetooth サブシステム + Context 等を前提に書かれている。これらを満たさないランタイム(=ブラウザ)では動かない。**「とにかく動かしたい」ではなく「動作前提を満たすランタイムは何か」から逆算する**。

---

## 8. レイヤ別 機能アクセス可能性マトリクス

| 機能カテゴリ | 純 PWA | Capacitor PWA(=同じ)| Capacitor APK | 純ネイティブ APK |
|---|---|---|---|---|
| HTTP / WebSocket | ◎ | ◎ | ◎ | ◎ |
| IndexedDB / OPFS | ◎ | ◎ | ◎ | (Room 等で代替)|
| Service Worker | ◎ | ◎ | ◎(限定的)| — |
| カメラ(getUserMedia)| ◎ | ◎ | ◎(@capacitor/camera)| ◎(CameraX)|
| 位置情報(Geolocation)| ◎ | ◎ | ◎ | ◎ |
| WebAuthn / パスキー | ◎ | ◎ | ◎ | ◎ |
| Web Bluetooth(汎用 GATT)| △ Chromium のみ | △ Chromium のみ | ◎(@capacitor-community/bluetooth-le)| ◎(BluetoothAdapter)|
| Web NFC(NDEF)| △ Chromium のみ | △ Chromium のみ | ◎(Plugin)| ◎ |
| Web USB / Serial | △ Chromium のみ | △ Chromium のみ | ◎ | ◎ |
| **BT Classic SPP** | **×** | **×** | ◎(Plugin)| ◎ |
| **NFC HCE**(カードエミュレーション)| **×** | **×** | ◎ | ◎ |
| **Foreground Service**(長時間常駐)| **×** | **×** | ◎ | ◎ |
| **WorkManager**(信頼性 BG)| **×** | **×** | ◎ | ◎ |
| **Android Keystore / TEE** | **×** | **×** | ◎ | ◎ |
| **Intent / BroadcastReceiver** | **×** | **×** | ◎(Plugin 化)| ◎ |
| **DataWedge Intent** | **×** | **×** | ◎(カスタムPlugin)| ◎ |
| **ベンダー AAR(各社 HT/スキャナ SDK)** | **×** | **×** | ◎(カスタムPlugin)| ◎ |
| **UHF RFID(外付けスキャナ・HT 内蔵問わず)** | **×** | **×** | ◎ | ◎ |
| **赤外線 / シリアル / UWB / Zigbee** | **×** | **×** | ◎(Plugin or 直)| ◎ |
| **MDM App Config 注入** | **×** | **×** | ◎ | ◎ |
| **Device Owner / Kiosk** | **×** | **×** | ◎ | ◎ |

凡例: ◎=完全対応 / △=制約あり(ブラウザ依存)/ ×=原理的に不可

**Capacitor を採用する価値**は「**Capacitor APK** 列を獲得する」こと。「Capacitor PWA」列は純 PWA と同じで、ベンダー SDK には到達できない。

---

## 9. 既存資料との接続

- 本書の主張は [`architecture_pwa.puml`](./architecture_pwa.puml) の `7_pwa_hw_capability` および `8_pwa_constraints` の図でも示されている。本書はその「**なぜそうなるのか**」のレイヤ的説明。
- [`architecture_hybrid.puml`](./architecture_hybrid.puml) の `4_hybrid_plugin_architecture` がカスタムプラグイン実装の構造、`5_hybrid_ht_scanner` が DataWedge Intent の Plugin 化例。外付け SDK 提供型の端末も同じパターン(BroadcastReceiver → BluetoothGatt + SDK 呼び出しに変わるだけ)。
- [`pwa_vs_native_criteria.md`](./pwa_vs_native_criteria.md) IV 章(HW 機能アクセス)IV10 UHF RFID、IV11 プリンタ、IV15 シリアル 等の足切り項目に該当。
- [`../framework_comparison/profile_ionic_capacitor.md`](../framework_comparison/profile_ionic_capacitor.md) 6.1 HT スキャナ連携、6.8 PWA 同時展開 の前提条件。
- [`../checks/05_js_framework_for_ionic.md`](../checks/05_js_framework_for_ionic.md) で評価した React/Vue/Angular の選定は UI 層の話であり、本書のレイヤ境界とは独立(どの JS FW を選んでも本書の制約は同じ)。

---

## 10. 参照リンク

### 公式

| 対象 | URL |
|---|---|
| Capacitor アーキテクチャ概要 | https://capacitorjs.com/docs/core-concepts |
| Capacitor プラットフォーム検出 | https://capacitorjs.com/docs/core-apis/web |
| Capacitor カスタムプラグイン(Android)| https://capacitorjs.com/docs/plugins/android |
| Capacitor Web フォールバック実装 | https://capacitorjs.com/docs/plugins/web |
| PWA Elements(@ionic/pwa-elements) | https://github.com/ionic-team/pwa-elements |
| Android クラスローダ | https://developer.android.com/reference/dalvik/system/DexClassLoader |
| Web Bluetooth 仕様 | https://webbluetoothcg.github.io/web-bluetooth/ |
| TWA(Trusted Web Activity)| https://developer.chrome.com/docs/android/trusted-web-activity |

---

## 更新履歴

- **2026-05-11**: 初版作成。外付け BT 型 UHF RFID/バーコードスキャナを例に「AAR + Capacitor プラグイン + PWA 配信」の不成立理由をレイヤ境界の観点から整理。
- **2026-05-11**: Section 2.4 を追加。本書の 3 層レイヤ図が Android 公式アーキテクチャの投影であって Web 業界のデファクトではないこと、および Web/PWA の実際のレイヤ慣習(パターン A〜D)を明記。
