# Android向けモバイルアプリ フレームワーク比較(業務アプリ観点)

本ドキュメントは `mobile_framework_comparison_7.xlsx` の調査結果を Markdown に再整理したものである。
情報は **2026年4月時点**。市場シェアや採用率の数値は出典により差があるため、トレンドの参考値として扱うこと。

- 対象: Android をメインターゲットにする **業務アプリ**(社内業務・フィールド・HT等)
- 対象外: ゲーム(Unity/Unreal)、iOS 専用、組込・車載特化

---

## 1. 主要フレームワーク一覧(業務アプリ観点)

### 1.1 マルチプラットフォーム(主要評価対象)

| フレームワーク | 言語 | UI描画方式 | 主なメリット | 主なデメリット | 代表採用事例 | 業務アプリ適性 |
|---|---|---|---|---|---|---|
| **Flutter** | Dart | 独自エンジン Impeller でピクセル単位に自前描画 | UI完全一致、AOTで120FPS、Hot Reload、Google後援でシェア拡大(~46%)、DevTools/テスト基盤が公式一貫、Widget豊富 | Dart学習、iOS UX の違和感、APK 8–12MB~、新OS機能追従がネイティブ比やや遅い、国内人材プール薄め | Google Pay / BMW My BMW / Alibaba Xianyu(2億UU) / Toyota車載 / PUBG Mobile | ◎(UI一貫性重視の業務アプリに最適) |
| **React Native** | JS/TS | OS純正ネイティブUIを JSI/Fabric(新アーキ)経由 | 端末ネイティブな見た目、新アーキで旧来の性能課題を解消、npmエコシステム最大、Web(React)資産・人材流用、大手本番採用多数 | 完全コード共有は80–95%、サードパーティ依存多、複雑アニメは Flutter 比で劣位、ネイティブモジュールには Swift/Kotlin 知識要 | Facebook / Instagram / Office / Shopify / Discord / Walmart | ◎(Web資産・JS人材がある組織) |
| **Expo (RN)** | JS/TS | RN 上の統合プラットフォーム。EAS Build/Update でクラウドビルド・OTA | 立ち上げ最速、OTA でストア審査なしに即時配信、公式 expo-* 品質が安定、Expo Router の File-based ルーティング | 細かいネイティブ制御は Dev Client / Prebuild 必須、EAS 有料プラン、OTA 配信はストアポリシーに注意 | MrBeast / BlueSky(多数のRN新規) | ◎(RNを採るなら事実上の第一選択肢) |
| **Ionic + Capacitor** | JS/TS (+ Angular/React/Vue) | WebView で Web アプリを描画 + Capacitor でネイティブAPI | 既存Webスキルを転用、Web/PWA/モバイルで90%+コード共有、Ionic UI コンポーネントがOSデザイン寄り、立ち上げ最速 | WebView のため高負荷UI/3D/重アニメは不利、ネイティブ操作感は作り込み要、Capacitor プラグイン品質にばらつき | BBC Sounds / Burger King / T-Mobile / Diesel | ○ 社内業務/情報系では◎、高負荷UIでは△ |
| **NativeScript** | JS/TS (Angular/Vue/Svelte) | JS からネイティブUI APIを直接呼び出し(WebView非使用) | WebView不使用でネイティブUI、JSからネイティブAPI直接呼び出し | エコシステム小、大手採用少、国内人材極少、2024年以降モメンタム鈍化 | SAP / GeekyAnts(限定的) | △(参考) |
| **KMP + Compose Multiplatform** | Kotlin (+ Swift) | ロジックは Kotlin 共通、UI はネイティブ or Compose MP | ネイティブ性能・UX維持、ロジック層の段階的共通化、JetBrains後援で将来性、本番安定 | iOS UI は結局 Swift/SwiftUI 知識要、UI共通化は学習コスト高、Flutter/RN のような「爆速書き捨て」開発には不向き | Netflix / Cash App / McDonald's / Philips / Forbes / Careem | ○ 既存Androidネイティブ資産ある組織では◎ |

### 1.2 ハイブリッド/レガシー(参考)

| フレームワーク | 言語 | 位置付け |
|---|---|---|
| .NET MAUI | C#/XAML | Xamarin 後継。Androidモバイルに限ると人材・情報が少ない。.NET社内標準組織のみ候補 |
| Cordova / PhoneGap | JS/HTML/CSS | Capacitor の前世代。PhoneGap は 2020 年終了。新規非推奨 |

### 1.3 ネイティブ(プラットフォーム専用)

| フレームワーク | 言語 | コメント |
|---|---|---|
| **ネイティブ Android(Jetpack Compose / Android SDK)** | Kotlin | 最高性能・新OS機能即時追従・国内人材豊富。Android専用のため iOS が必要ならコスト倍。**Android 単独なら第一選択肢** |
| SwiftUI / UIKit | Swift | 本件 Android 対象のため対象外 |

### 1.4 対象外

- **Unity / Unreal**(ゲームエンジン): 業務アプリのUI/UXパラダイムと乖離、過剰
- **Flutter for Embedded / Qt Mobile**: 車載・産業機器向け特化で本件要件外

---

## 2. プロジェクト特性別 推奨マトリクス

凡例: ◎=強く推奨 / ○=適合 / △=条件付き / ×=非推奨

| 判断軸 | ケース | Flutter | RN+Expo | Ionic+Capacitor | KMP+Compose | 備考 |
|---|---|---|---|---|---|---|
| **ターゲットOS** | Androidのみ | ○ | ○ | ○ | ◎ | ネイティブ Kotlin も有力 |
|  | Android + iOS | ◎ | ◎ | ○ | ○ | 全FW対応可 |
|  | + Web(同UI) | ◎ | ○(RN Web) | ◎(本質) | △ | Web同時展開なら Ionic / Flutter |
|  | + Desktop | ◎ | △ | ◎(Electron) | △ | Flutter / Ionic 有利 |
| **UI要件** | 標準フォーム/CRUD | ◎ | ◎ | ◎ | ◎ | 生産性で選ぶ |
|  | ブランド統一・ピクセル完全一致 | ◎ | △ | ○ | △ | Flutter 得意 |
|  | OS純正ネイティブUX必須 | △ | ◎ | △ | ◎ | RN / KMP |
|  | 高負荷アニメ/3D | ◎ | ○ | × | ○ | Flutter / ネイティブ |
| **チームスキル** | Web React | △ | ◎ | ◎(+React) | △ | RN / Ionic で即戦力 |
|  | Web Vue/Angular | △ | △ | ◎ | △ | Ionic ほぼ一択 |
|  | 既存Android Kotlin | ○ | △ | △ | ◎ | KMP で資産活用 |
|  | 混成・これから採用 | ◎ | ◎ | ○ | △ | Flutter / RN は採用市場厚い |
| **規模** | 小(〜10画面, 〜半年) | ○ | ◎ | ◎ | △ | 立ち上げ速度重視 |
|  | 中(10〜50画面, 1〜2年) | ◎ | ◎ | ◎ | ○ | いずれも適合 |
|  | 大(50画面〜, 5年以上) | ◎ | ◎ | ○(Angular必須) | ◎ | 長期保守は Angular / Flutter / KMP |
| **機能要件** | オフライン/ローカルDB | ◎(Drift/Isar) | ◎ | ○ | ◎(SQLDelight) | KMP は型安全で強い |
|  | Bluetooth/センサ/HW統合 | ○ | ○ | △ | ◎ | 低遅延・高頻度は KMP/ネイティブ |
|  | カメラ/QR/位置 | ◎ | ◎ | ◎ | ◎ | どれでもOK |
|  | 決済/生体認証/高セキュリティ | ◎ | ◎ | ○ | ◎ | WebView の Ionic は慎重評価 |
|  | バックグラウンド常駐 | ○ | ○ | △ | ◎ | ネイティブ制御要 |
| **運用要件** | OTA配信 | △ | ◎(EAS) | ◎(Webリロード) | △ | RN/Expo or Ionic |
|  | OSアップデート追従速度 | ○ | ○ | △ | ◎ | 新OS即時対応はネイティブ寄り |
|  | 採用市場の人材 | ◎ | ◎ | ◎ | ○ | JS 系が最多 |

### 2.1 代表ユースケース別 最終推奨

| ユースケース | 第一推奨 | 第二推奨 | 理由 |
|---|---|---|---|
| 社内業務アプリ(申請/承認/閲覧、Web併用) | Ionic+Capacitor+Angular | Ionic+Capacitor+React | Web資産流用、Web同時展開、業務フォームに強いAngular |
| 営業支援/フィールド(オフライン・カメラ・位置情報) | **Flutter** | RN+Expo | 性能・オフラインDB・UIコンポーネント充実 |
| コンシューマ向けSaaSモバイル版 | **RN+Expo** | Flutter | Web(React)資産共有、OTA迅速更新 |
| 基幹系モバイル(大規模・長期保守) | **Flutter** | KMP+Compose MP | UI一貫性・5年以上保守性・AOT性能 |
| Android中心で将来iOS展開の可能性あり | **KMP + ネイティブUI** | Flutter | 既存Android資産を活かし段階的iOS対応 |
| MVP/短期プロトタイプ | **RN+Expo** | Flutter | 立ち上げ最速、EAS Build で即配布 |
| 高負荷UI(アニメ・グラフ・ダッシュボード) | **Flutter** | RN(Reanimated) | 独自描画で120FPS安定 |
| 既存Webをそのままモバイル化 | **Ionic+Capacitor** | — | Web資産90%+共有、最短 |

### 2.2 最終判断チェックリスト(10項目)

1. ターゲットOSは?(Androidのみ / +iOS / +Web / +Desktop)
2. 既存の技術資産とチームスキルは?(Kotlin / React / Vue / Angular / 混成)
3. UI要件は?(ブランド統一 / ネイティブUX / フォーム中心 / 高負荷アニメ)
4. プロジェクト規模と保守期間は?(〜半年 / 1〜2年 / 5年以上)
5. ハードウェア連携要件は?(カメラ程度 / Bluetooth・センサ / 常駐BG)
6. OTA 配信(ストア審査回避)の重要度は?
7. 人材採用のしやすさをどこまで優先するか?
8. セキュリティ・コンプライアンス要件は?(金融/医療等の規制)
9. オフライン動作・ローカルDB の重要度は?
10. Web 版同時展開・将来必要性は?

---

## 3. HT(ハンディターミナル)業務アプリ特化観点

前提: Android ベース業務用 HT(Zebra / Honeywell / Datalogic)、20画面規模、API 連携主体(マスタ取得・トランザクション送信・オフラインキュー)。

### 3.1 HT 業務アプリ特有の10要件

| # | 要件 | 一般スマホアプリとの違い |
|---|---|---|
| 1 | スキャナSDK統合(Zebra DataWedge/EMDK、Honeywell、Datalogic) | 専用レーザ/イメージャ制御。Keyboard Wedge で済むか Intent 連携が要か |
| 2 | ハードウェア物理キー | 物理トリガーやファンクションキーからの発火、KeyEvent 相当の制御 |
| 3 | MDM 対応(SOTI / Workspace ONE / Intune / Zebra StageNow) | MDM 経由の APK 配布が前提、App Config 動的注入 |
| 4 | Kiosk / ロックダウン | 単一業務アプリのみ起動、Home/Back 封じ、Device Owner / MDM 制御 |
| 5 | オフライン耐性 | 通信断での業務継続、ローカルキュー・自動再送が肝 |
| 6 | 古いAndroid/特殊SoC(Android 8〜13、ARMv7 も) | 最新APIに頼れない、Target/Min SDK、NDK ABI の選定に影響 |
| 7 | バッテリー/8時間連続稼働 | 省電力チューニング、WakeLock・通信頻度最適化 |
| 8 | 業務要件UI(小画面3.5〜5型、手袋操作、屋外視認、片手操作) | 入力スループット優先、流麗アニメは不要 |
| 9 | 配布・バージョン管理(MDM 一括、段階的ロールアウト、拠点別) | 社内CI/CD・署名・配信フロー整備 |
| 10 | 長期保守(同一端末5〜7年) | FW 本体の LTS/後方互換性が重要 |

### 3.2 HT 要件 × フレームワーク 適性マトリクス

| 要件 | ネイティブAndroid | Flutter | RN+Expo | Ionic+Capacitor | KMP+Compose | コメント |
|---|---|---|---|---|---|---|
| DataWedge Keyboard Wedge | ◎ | ◎ | ◎ | ◎ | ◎ | スキャン=キー入力。InputフォーカスがあればどのFWもOK |
| DataWedge Intent API(詳細制御) | ◎ | ◎(Zebra公式デモあり) | ○(非公式プラグイン) | ○(Capacitor Intent プラグイン) | ◎ | Expo は Managed 不可、Bare/Dev Client 必須 |
| Zebra EMDK | ◎ | △(サンプル) | △(3rd partyラッパ要) | ×(非推奨) | ◎ | 低レベル制御が必須な案件のみ EMDK |
| Honeywell / Datalogic SDK | ◎ | ○(コミュニティプラグイン) | △(個別対応) | △(カスタム自作) | ◎ | 各社SDKは Java/Kotlin前提 |
| HWキーマッピング | ◎ | ○(RawKeyboardListener) | ○(ネイティブモジュール要) | △(WebView境界で取りこぼし) | ◎ | 物理キー多用ならネイティブ/Flutter |
| MDM App Config取得 | ◎ | ○ | ○ | ○ | ◎ | いずれも可だがネイティブ直実装が最も確実 |
| Kiosk / Device Owner | ◎ | ○ | ○ | ○ | ◎ | 結局ネイティブ権限APIが必要 |
| オフライン/SQLite | ◎(Room) | ◎(Drift/Isar) | ◎(WatermelonDB) | ○(SQLite plugin) | ◎(SQLDelight) | Flutter/KMP は型安全で堅牢 |
| 古いAndroid(API 26〜29) | ◎ | ○ | ○ | ○ | ◎ | 実装上は API 21-24 世代でも実績あり |
| 非力SoC上のUI性能 | ◎(最軽量) | ◎(AOT、軽量) | ○(JS Engine) | △(WebView負荷) | ◎ | WebView 初期レンダが遅い |
| APK サイズ | ◎(数MB) | △(10〜15MB〜) | ○(5〜10MB) | ◎(小さい) | ◎ | MDM 配信帯域に影響 |
| バッテリー効率 | ◎ | ○ | ○ | △(常時JS実行) | ◎ | 8時間連続稼働では N/Flutter/KMP が安心 |
| API連携 REST/gRPC | ◎(Retrofit/Ktor) | ◎(Dio) | ◎(Axios/fetch) | ◎ | ◎(Ktor) | 差がつかない |
| オフラインキュー・再送 | ◎(WorkManager) | ○(workmanager plugin) | ○(BG Fetch) | △(BG制限強い) | ◎(WorkManager直結) | ネイティブ WorkManager が最強 |
| 20画面フォーム | ◎(Compose) | ◎ | ◎ | ◎ | ◎ | 画面数20は全FWの得意領域 |
| MDM配信・CI/CD | ◎ | ○ | ○ | ○ | ◎ | Gradle ベースが CI 構築素直 |
| ベンダーサンプル・ドキュメント | ◎(完全) | ○(Zebra公式、他社少) | △(非公式中心) | △(最小限) | ○(Zebra増加傾向) | Honeywell/Datalogic はネイティブ前提が大半 |
| 国内SIer 実装実績 | ◎ | ○(増加中) | △ | ○(レガシーWeb資産活用時) | △(新興) | 国内 HT SI はネイティブ主流、次いで Xamarin/Ionic |

### 3.3 総合スコア(HT + 20画面 + API連携主体)

| フレームワーク | スキャナ | 性能/バッテリー | 開発生産性 | 国内人材 | 長期保守性 | 総合 | コメント |
|---|---|---|---|---|---|---|---|
| **ネイティブ Android (Kotlin + Compose)** | ◎ | ◎ | ○ | ◎ | ◎ | ★★★★★ | HT 業界の事実上標準。Android 限定なら第一選択肢 |
| **Flutter** | ◎ | ◎ | ◎ | ○ | ◎ | ★★★★★ | Zebra 公式 DataWedge デモあり。AOT性能で非力SoC好適 |
| KMP + ネイティブUI (Compose) | ◎ | ◎ | ○ | △ | ◎ | ★★★★☆ | 将来iOS対応の可能性がある場合に有力 |
| RN + Expo | ○ | ○ | ◎ | ◎ | ○ | ★★★☆☆ | Expo は Dev Client/Prebuild 必須、物理キー/MDMで摩擦 |
| Ionic + Capacitor | ○ | △ | ◎ | ◎ | ○ | ★★★☆☆ | Wedge モードで済むなら最短。長時間稼働・複雑制御は不利 |
| .NET MAUI / Xamarin | ○ | ○ | ○ | △ | △ | ★★☆☆☆ | 歴史的には日系SIerで採用。Xamarin EOL → MAUI 移行課題 |

### 3.4 HT 案件の意思決定フロー

1. **対象デバイスとスキャナ要件を確定**
   機種 / DataWedge の有無 / Wedge で足りるか Intent 必要か / 物理キー利用
2. **iOS 展開の可能性を確定**
   あり → Flutter / KMP / RN 優位、なし(Android 永続) → ネイティブも有力
3. **チーム構成**
   Kotlin 中心 → ネイティブ or KMP、JS/TS 中心 → RN+Expo or Ionic、Dart 新規OK → Flutter
4. **既存資産**
   Android 資産 → KMP / ネイティブ継続、Web 資産 → Ionic、React 資産 → RN+Expo、新規 → Flutter or ネイティブ
5. **性能・UI 要件**
   物理キー・BG 処理・低遅延 → ネイティブ/KMP/Flutter、フォームCRUD+Wedgeのみ → どれでも可、8時間常駐 → ネイティブ/Flutter/KMP
6. **MDM / 配布要件**
   MDM + App Config + Device Owner → ネイティブ最確実、Flutter/KMP も可

**典型的な推奨**

- Android HT 専用・ネイティブチームあり → **ネイティブ Kotlin + Compose**
- クロスプラットフォーム / UI生産性重視 → **Flutter**
- 既存Androidコード資産 + 将来iOS → **KMP + Compose**
- Web 資産活用・Wedge のみ → **Ionic + Angular/React**
- JS/React チーム主体 → **RN + Expo (Dev Client)**

### 3.5 HT 実装チェックリスト(選定影響度付き・20項目抜粋)

選定影響度: 高=FW選定に直接効く / 中=実装難度・品質に差 / 低=FW非依存

全20項目のうち、影響度「**高**」=7項目、「中」=5項目、「低」=8項目。高の項目(主に No.1/5/6/8/17/18/19)が実質的な選定判断材料。

| カテゴリ | 代表チェック項目 | 選定影響度 | 影響内容 |
|---|---|---|---|
| スキャナ | DataWedge プロファイル自動作成/切替 | 高 | Intent連携品質: Flutter(公式デモ) > ネイティブ > RN(非公式) > Ionic(自作) > Expo Managed(不可) |
|  | MultiBarcode 要件 | 中 | Intent API 必須のため Expo Managed 不可 |
| HW | 物理ファンクションキー/ボリュームキー | 高 | Ionic(WebView境界)が取りこぼしリスク |
|  | NFC/RFID/プリンタ(ZPL) | 高 | プラグイン有無で差大、プリンタSDKは Java/Kotlin 前提 |
| 通信 | BG 同期(定期ポーリング/push) | 高 | Ionic 不利、ネイティブ WorkManager が最強 |
|  | 社内VPN/クライアント証明書 | 中 | HTTPクライアント設定方法が FW 毎に異なる |
| 運用 | MDM App Config 注入 | 中 | プラグイン成熟度で差、ネイティブは標準API直叩き |
| UI/UX | 画面遷移速度(〜300ms) | 高 | 非力SoCでは Ionic(WebView初期レンダ)不利 |
| 保守 | FW 本体の LTS と端末寿命(5〜7年)整合 | 高 | ネイティブ > Flutter/KMP > RN > Ionic/MAUI |
|  | サードパーティプラグインのメンテ | 高 | 依存度: Ionic/RN(多) > Flutter(pub.dev公式度高) > ネイティブ/KMP(最小) |
|  | Android Target SDK 強制引き上げ追従 | 中 | Ionic/MAUI は遅延の可能性 |

---

## 4. Ionic 採用時の JS フレームワーク選定

Ionic Framework は React / Vue / Angular / Vanilla のいずれとも組合せ可能。

| 評価観点 | React | Vue | Angular |
|---|---|---|---|
| 学習容易性 | 中(JSX/Hooks) | ◎ 最も易しい(HTML寄りテンプレ) | 高コスト(DI/RxJS/Decorators) |
| 生産性(短期) | ◎ | ◎ SFC で実装速 | ○ ボイラープレート多め |
| 生産性(長期・大規模) | ○(設計依存) | ○(規約弱め) | ◎ 大規模・長期保守最適 |
| 適した規模 | 小〜超大規模 | 小〜中で最強 | 中〜大規模専用 |
| 型安全性(TS) | ○ | ○ | ◎ 必須・最も型安全 |
| 状態管理 | 多数(Redux/Zustand/TanStack Query) | Pinia 事実上標準 | RxJS+Services |
| エコシステム | ◎ npm 最大 | ○ 中規模 | ○ エンタープライズ充実 |
| 国内人材プール | ◎ 最多 | ○ 中 | △ 少(金融/ES系SIer集中) |
| テスト容易性 | ○ Jest/RTL | ○ Vitest/Vue Test Utils | ◎ Jasmine/Karma+TestBed 同梱 |
| 業務アプリ適性 | ○ 自由度高 | ○ 中小業務画面に最適 | ◎ 大規模業務フォーム/RBAC/多言語に最適 |
| AI 補完相性 | ◎ 学習データ最多 | ○ | ○(型情報多いが固有パターンで誤ることあり) |
| 総合おすすめ度 | ★★★★☆ バランス | ★★★★☆ 小〜中・短期最適 | ★★★★★(大)/ ★★☆☆☆(小)|

### 選定クイックガイド

- 既存 React 経験あり → **React + Ionic**
- 既存 Vue 経験あり → **Vue + Ionic**
- 既存 Angular / エンタープライズ SIer → **Angular + Ionic**
- 小規模(〜10画面, 〜半年)→ **Vue(or React)**
- 中規模(10〜50画面, 1〜2年)→ **React or Vue**
- 大規模(50画面以上, 複数チーム, 5年+)→ **Angular**
- 短期立ち上げ重視 → Vue / 長期運用バランス → React / 型安全・規約統制・テスト重視 → Angular

> 本件(HT Android・20画面・API主体・jQuery+Java 経験)向け推奨:
> **Vue + Ionic が最有力**。jQuery からのテンプレート構文移行がスムーズ、20画面は Vue のスイートスポット、Pinia で状態管理もシンプル、2〜3週間で戦力化。
> Angular は 20画面では過剰投資(DI/RxJS/Reactive Forms は 50画面以上・複数チーム・5年以上保守で回収される)。

---

## 5. PWA vs ネイティブアプリ実装(HT観点・Android永続前提)

### 5.1 基本的な違い

| 観点 | PWA | ネイティブ/ハイブリッドアプリ |
|---|---|---|
| 実行環境 | ブラウザ/WebView + Service Worker | OSランタイム上、ネイティブAPI直接 |
| 配布 | URL + A2HS、ストア審査不要 | Play / MDM / APK 直配布 |
| 更新 | サーバ更新で即反映 | MDM配信 / APK再配布 |
| HWアクセス | ブラウザAPI範囲内(BT/USB/NFCはChromium限定) | 全OS API(スキャナ/NFC/BLE/USB/プリンタ) |
| オフライン | SW + IndexedDB(容量・保証に制約) | SQLite/Room + WorkManager(堅牢) |
| BG処理 | Service Worker で短時間のみ | WorkManager/Foreground Service で本格的 |
| 性能 | ブラウザ/JS エンジン経由で遅い | ネイティブ最速、Flutter/RN も実用同等 |
| セキュリティ | HTTPS 必須、標準Webモデル | OSサンドボックス・キーストア |
| 開発コスト | Web スキルのみ、1 コードベース | モバイル開発スキル要 |

### 5.2 HT 特有観点での可否マトリクス

| HT要件 | PWA | アプリ | 注意点 |
|---|---|---|---|
| DataWedge Keyboard Wedge | 可 | 可 | PWAの最大の武器。`<input>` に届くため Webフォームがあれば動く |
| DataWedge Intent API(詳細制御) | **不可** | 可 | PWAは Broadcast Intent 受信不可。MultiBarcode等はネイティブ必須 |
| 物理キーマッピング | 条件付 | 可 | Web の keydown/keyup で一部、SCAN/F1-F12 は取りこぼしあり |
| Honeywell / Datalogic SDK | 条件付 | 可 | Wedge は可。詳細制御・RFID・プリンタはネイティブSDK |
| NFC/RFID(産業用UHF) | 条件付 | 可 | Web NFC は Chromium NDEF 限定 |
| プリンタ(ZPL) | 条件付 | 可 | ネットワークプリンタHTTPなら可、BT/USBはネイティブ |
| オフライン業務継続 | 条件付 | 可 | SW+IndexedDB で基本可、キャッシュクリア/容量制約リスク |
| 8h 常駐稼働 | 条件付 | 可 | Chromium メモリ事故リスク、キオスク+MDMで軽減 |
| BG 同期 | 条件付 | 可 | Background Sync は短時間のみ |
| MDM 一括配信 | 条件付 | 可 | URL固定は可、App Config 動的注入はアプリが容易 |
| Kiosk / Single-App | 可 | 可 | Lock Task / Chromeキオスクで対応 |
| 非力SoC軽快性 | 条件付 | 可 | ブラウザ自体のオーバーヘッド重 |
| 即時更新 | 可 | 可(MDM) | PWAはサーバ更新で即反映が強み |

### 5.3 実装アーキテクチャ 5パターン(Android永続前提)

| パターン | 構成 | 本件適性 |
|---|---|---|
| ① 純PWA | Webアプリ+URLアクセス | △ 簡易照会系のみ |
| ② PWA+Chromeキオスク | Chrome --kiosk + MDMで構成 | ○ Wedge系で済むなら実用的 |
| ③ TWA | PWAをAndroidアプリラッパ + MDM | △ HT実績少 |
| ④ ハイブリッド(Ionic+Capacitor+Vue) | Web(Vue)をネイティブラッパでAPK化 | ○ Web資産流用時の現実解 |
| ⑤ 純ネイティブ(Kotlin+Compose) | ネイティブ Android | **◎ 本件で最有力** |

### 5.4 3つの質問で決まる基本シナリオ

- **Q1**. スキャナは Keyboard Wedge で足りるか?
  YES → PWA/ハイブリッドも可 / NO(Intent要) → ネイティブ/ハイブリッド一択
- **Q2**. 8時間連続稼働・通信断跨ぎの業務継続・物理キー多用か?
  YES → ネイティブ強推奨 / NO(常時オンライン・短時間) → PWA/ハイブリッド可
- **Q3**. 既存 Web/jQuery 資産を流用したいか?
  YES → PWA または Vue+Ionic / NO → ネイティブ Kotlin が素直

### 5.5 本件での最終判断

| 推奨 | 内容 |
|---|---|
| A(既存Web資産活用+軽要件) | **PWA + Chromeキオスク** 最短開発・最小運用、Wedge で基本HT業務成立 |
| B(Web資産活用+本格HT機能) | **Ionic + Capacitor + Vue** Web資産をVueへ段階リライト、MDM配布可、Intent・プリンタも対応 |
| C(本格HT・長期保守・新規)★**本件最有力** | **ネイティブ Kotlin + Jetpack Compose** Android 永続でクロスプラットフォームの優位性消失、Javaチームに最も馴染む、各社SDK直接使用で不確実性最小 |

**避けるべき選択**

- KMP(Android 永続で本来価値消失)
- React Native(jQuery+Java チームに遠い)
- Angular+Ionic(20画面で過剰装備)

---

## 6. チームスキル観点(jQuery + Java 経験者中心)

### 6.1 Android 永続前提による評価変化

Android 専用前提では、クロスプラットフォームの最大価値「iOSとコード共通化」が失われる。

- ネイティブ Kotlin: **さらに優位性が増す**(競合の主要アドバンテージ消失)
- KMP + Compose MP: **大幅 downgrade**(純粋なオーバーヘッド)
- Flutter: UI 生産性の魅力は残るが「1コードで2OS」の決定打が消える
- React Native: 同様に魅力減
- Ionic+Capacitor: Web+モバイルのコード共有という別軸の価値は維持
- PWA: 影響なし

### 6.2 Java → 各言語への移行難易度

| 移行先 | 類似度 | 学習コスト | つまづきポイント |
|---|---|---|---|
| Kotlin(ネイティブAndroid) | ◎ 極めて高い | 極小 | null safety / coroutine / 拡張関数 |
| Dart(Flutter) | ○ 高い | 小 | Future/Stream, Widget Tree 思考 |
| TypeScript(Vue/React/Angular+Ionic) | ○ 中〜高 | 小〜中 | JS特有の振る舞い、npm/バンドラ/Promise |
| 素の JS | △ 低い | 中 | 動的型付け、this束縛、プロトタイプ |

### 6.3 フレームワーク別 学習コスト詳細(立ち上げ期間目安)

- 極小=1〜2週 / 小=2〜4週 / 中=1〜2ヶ月 / 大=2〜3ヶ月

| フレームワーク | 言語移行 | UIパラダイム | 立ち上げ | 既存スキル活用度 | 評価 |
|---|---|---|---|---|---|
| ネイティブAndroid(Kotlin+Compose) | 極小 | 大(Compose) | 2〜3ヶ月 | Java/OOP/DI/Gradle/テスト全て | **Javaスキル最大活用、Android永続で競合なし** |
| Flutter(Dart) | 小 | 大(Widget Tree) | 2〜3ヶ月 | Java親近感・OOP | Dart は Java 経験者に親しみやすい |
| **Vue + Ionic(TS)** | 小〜中 | 中(テンプレート) | 2〜3ヶ月 | HTML/CSS・jQuery経験 | **jQuery→テンプレ移行最スムーズ、20画面スイートスポット** |
| React + Ionic(TS/JSX) | 中 | 大(Hooks/関数型) | 3〜5ヶ月 | TS型のみ | jQuery経験者にHooks馴染むまで時間要 |
| Angular + Ionic(TS) | 小〜中 | 中(テンプレ+TS) | 3〜4ヶ月 | Java DI/型/モジュール/テスト | Spring的だが RxJS 学習コスト大、20画面では過剰装備 |
| KMP + Compose | 極小 | 大(Compose) | 3〜4ヶ月 | Java/Kotlin/Gradle/DI全て | Android 永続ではネイティブの劣化版 |
| RN + Expo | 中 | 大(Hooks+モバイル) | 4〜6ヶ月 | TS型のみ | jQuery+Java チームには最も遠い |
| .NET MAUI | 極小(C#) | 大(XAML) | 3〜4ヶ月 | Java→C#は容易 | コミュニティ限定・長期保守リスク |

### 6.4 統合最終推奨(HT Android永続 + 20画面 + API主体 + jQuery/Java経験)

| 順位 | フレームワーク | HT適性 | チーム適合度 | 総合 | 推奨理由 |
|---|---|---|---|---|---|
| **1位** | **ネイティブ Android(Kotlin + Jetpack Compose)** | ★★★★★ | ★★★★★ | ★★★★★ | Android永続でクロスプラットフォームの優位消失、Javaスキル最大活用、Spring/Gradle/JUnit そのまま転用、HT業界標準、各社SDK前提 |
| 2位 | Vue + Ionic(TS + Capacitor) | ★★★☆☆ | ★★★★☆ | ★★★★☆ | Web資産活用型の最適解、jQuery経験者のテンプレ移行スムーズ、Pinia/Vue Router 軽量 |
| 3位 | Flutter(Dart) | ★★★★★ | ★★★★☆ | ★★★★☆ | Android永続では優位性低下、Dart親近感、Zebra公式DataWedgeデモあり |
| 4位 | PWA(Chromeキオスク) | ★★☆☆☆ | ★★★★☆ | ★★★☆☆ | 既存Web資産最短活用、Wedge 十分実用、Intent/物理キー/堅牢オフライン 不要時のみ |
| 5位 | Angular + Ionic | ★★★☆☆ | ★★★☆☆ | ★★★☆☆ | 20画面では過剰投資、RxJS学習コスト大 |
| 圏外 | KMP + Compose MP | ★★★★☆ | ★★★★★ | ★★☆☆☆ | Android永続では純粋オーバーヘッド |
| 圏外 | RN / React+Ionic | ★★★☆☆ | ★★☆☆☆ | ★★☆☆☆ | jQuery+Javaチームに遠い、Android永続でRN メリット消失 |

### 6.5 主なリスクと対策

| リスク | 影響度 | 対策 |
|---|---|---|
| jQuery 的命令的DOM操作の癖で宣言的UIで冗長コード | 大 | コードレビュー体制、社内サンプル集、最初の3ヶ月は外部メンター投入 |
| 非同期処理(Promise/Future/Flow/coroutine)理解不足→コールバック地獄・race condition | 大 | 言語別非同期パターンを初期教育の最重要項目に、リポジトリ層で隠蔽 |
| モダンJS(ES2020+)・TSへの抵抗感 | 中 | JS 系採用時は TS 必須化、ESLint/Prettier で規約強制 |
| 初期立ち上げ期の生産性低下 | 大 | 学習期間2〜3ヶ月をスケジュール明示、PoC実施、技術相談窓口確保 |
| モバイル経験者ゼロ | 大 | 1名は採用または業務委託でレビュー/相談役確保 |
| 長期保守メンバー入替時の再学習コスト | 中 | 社内ドキュメント継続整備、Javaに近い言語(Kotlin/TS)採用 |

---

## 7. 総括

**業務アプリでの典型的推奨パターン**(Android中心前提)

| シナリオ | 推奨 |
|---|---|
| Android HT・ネイティブ/Javaチーム・長期保守 | **ネイティブ Kotlin + Jetpack Compose** |
| 営業支援/フィールド・高UI品質・Android+iOS | **Flutter** |
| 社内業務・Web併用・既存Web資産 | **Ionic + Capacitor (+ Angular か Vue)** |
| SaaSモバイル・Webチーム(React)・OTA重視 | **React Native + Expo** |
| 既存Android資産・将来iOS段階拡張 | **KMP + Compose Multiplatform** |
| 軽量・短期・Web資産最大活用 | **PWA + Chromeキオスク** |

**避けるべきパターン**

- Unity/Unreal: 業務アプリのUIパラダイムと乖離
- Cordova/PhoneGap/Xamarin: レガシー、新規非推奨
- Android 永続で KMP: オーバーヘッドのみになる
- 20画面で Angular+Ionic: 過剰装備

---

## 8. 参照ソース

2026年4月時点で取得。公式ドキュメント以外は各社・各筆者の見解を含むため、実プロジェクトでは最新の公式情報で再確認を推奨。

### 8.1 公式・一次情報(ドキュメント/リリース)

| # | 対象 | タイトル | URL |
|---|---|---|---|
| 1 | Flutter | Flutter 公式 | https://flutter.dev/ |
| 2 | Flutter | pub.dev(Flutterパッケージレジストリ) | https://pub.dev/ |
| 3 | React Native | React Native 公式 | https://reactnative.dev/ |
| 4 | Expo | Expo 公式 | https://expo.dev/ |
| 5 | Ionic | Ionic Framework 公式 | https://ionicframework.com/ |
| 6 | Capacitor | Capacitor 公式(Ionic社) | https://capacitorjs.com/ |
| 7 | NativeScript | NativeScript 公式 | https://nativescript.org/ |
| 8 | KMP | Kotlin Multiplatform 公式(JetBrains) | https://kotlinlang.org/docs/multiplatform.html |
| 9 | Compose MP | Compose Multiplatform 公式 | https://www.jetbrains.com/lp/compose-multiplatform/ |
| 10 | Jetpack Compose | Android Jetpack Compose 公式 | https://developer.android.com/jetpack/compose |
| 11 | .NET MAUI | .NET MAUI 公式(Microsoft) | https://dotnet.microsoft.com/apps/maui |
| 12 | Unity | Unity 公式 | https://unity.com/ |

### 8.2 比較記事・市場動向(二次情報)

| # | 対象 | タイトル | URL |
|---|---|---|---|
| 1 | Flutter vs RN | Flutter vs React Native: 46% vs 35% Market Share [2026](tech-insider.org) | https://tech-insider.org/flutter-vs-react-native-2026/ |
| 2 | Flutter vs RN | Flutter vs React Native in 2026: Definitive Comparison for Enterprise Apps(Cozcore) | https://www.cozcore.com/blog/flutter-vs-react-native-2026/ |
| 3 | Flutter vs RN | React Native vs Flutter in 2026: Performance, Cost & Speed(Discretelogix) | https://www.discretelogix.com/react-native-vs-flutter/ |
| 4 | Flutter vs RN | Flutter vs React Native 2026 for Enterprise Apps(MiTSoftware) | https://mitsoftware.com/en/blog/flutter-vs-react-native-2026-business-app/ |
| 5 | Flutter vs RN | React Native vs Flutter: which one in 2026?(SoftTeco) | https://softteco.com/blog/react-native-vs-flutter |
| 6 | Flutter vs RN | Flutter vs React Native 2026: Performance Cost DX(Agilesoftlabs) | https://www.agilesoftlabs.com/blog/2026/02/flutter-vs-react-native-2026-cost-dx_17 |
| 7 | Flutter vs RN | Flutter vs React Native 2026 – Performance, Cost & Scalability(Tech4LYF) | https://www.tech4lyf.com/blog/flutter-vs-react-native-2026/ |
| 8 | Flutter vs RN | React Native vs Flutter 2026: Code Quality or Tech Debt?(Aetherius) | https://www.aetherius-solutions.com/blog-posts/react-native-vs-flutter-in-2026 |
| 9 | Flutter | Why Flutter Outperforms React Native and Native Development in 2026(Foresight Mobile) | https://foresightmobile.com/blog/why-flutter-outperforms-the-competition |
| 10 | Flutter vs RN | Flutter vs React Native in 2026: Ultimate Showdown(TechAhead) | https://www.techaheadcorp.com/blog/flutter-vs-react-native-in-2026-the-ultimate-showdown-for-app-development-dominance/ |
| 11 | KMP | Flutter vs Kotlin Multiplatform: The 2026 Guide(Luciq) | https://www.luciq.ai/blog/flutter-vs-kotlin-mutliplatform-guide |
| 12 | KMP | Flutter vs Kotlin Multiplatform: Which Is Better for 2026?(Wildnet Edge) | https://www.wildnetedge.com/blogs/flutter-vs-kotlin-multiplatform-which-is-better |
| 13 | KMP | Kotlin Multiplatform vs Flutter vs React Native: 2026(TechQware) | https://www.techqware.com/blog/kotlin-multiplatform-vs-flutter-vs-react-native-what-to-choose |
| 14 | KMP | Flutter vs Kotlin Multiplatform: 2026 Comparison(Solguruz) | https://solguruz.com/blog/flutter-vs-kotlin/ |
| 15 | KMP | Kotlin Multiplatform in 2026: Why We Finally Deleted Our Flutter Code(Medium - Sivaraj Ramasamy) | https://medium.com/@sivaraaj/kotlin-multiplatform-in-2026-why-we-finally-deleted-our-flutter-code-6c3eb9ef6144 |
| 16 | KMP | Kotlin Multiplatform vs Flutter for Cross-Platform Apps in 2026(Orangemantra) | https://www.orangemantra.com/blog/kotlin-multiplatform-vs-flutter/ |
| 17 | KMP | Kotlin Multiplatform vs Flutter vs React Native: 2025 Developer Guide(KMPShip) | https://www.kmpship.app/blog/kmp-vs-flutter-vs-react-native-2025 |
| 18 | Flutter批判 | Why Flutter Isn't Ideal for Cross-Platform Development in 2026(KITRUM) | https://kitrum.com/blog/why-flutter-isnt-ideal-for-cross-platform-development/ |
| 19 | KMP | Kotlin vs Flutter Comparison 2026: Market Share, DX & Future Outlook(C-Metric) | https://www.c-metric.com/blog/kotlin-vs-flutter-who-will-rule-the-cross-platform-market-in-2026/ |
| 20 | KMP | Flutter vs Kotlin: Business-Driven Comparison 2026(Limeup) | https://limeup.io/blog/flutter-vs-kotlin/ |
| 21 | Capacitor | Building Native Apps with Capacitor and Vue/React/Angular(Blue Altair) | https://www.bluealtair.com/blog/building-native-apps-with-capacitor-and-vue/react/angular |
| 22 | Ionic | Ionic & Capacitor: The Best Path For Performance(Ionic公式ブログ) | https://ionic.io/blog/ionic-capacitor-the-best-path-for-performance |
| 23 | Ionic vs RN | Ionic vs React Native in 2026(Hakuna Matata Tech) | https://www.hakunamatatatech.com/our-resources/blog/mobile-application-development |
| 24 | React/Vue/Angular | React vs Vue vs Angular: Which Should You Choose in 2026?(Medium - hashbyt) | https://medium.com/@hashbyt/react-vs-vue-vs-angular-2026-6f5e9b3f0e3e |

### 8.3 PWA vs アプリ実装(追加参照)

| # | 対象 | タイトル | URL |
|---|---|---|---|
| 1 | PWA vs Native | PWA vs Native App — 2026 Comparison Table(Progressier) | https://progressier.com/pwa-vs-native-app-comparison-table |
| 2 | PWA Camera | PWA Camera Access: Can It Match Native App Capabilities?(Edana) | https://edana.ch/en/2026/03/25/can-a-web-app-pwa-access-the-camera-like-a-native-app/ |
| 3 | Zebra DataWedge | Zebra DataWedge Keystroke Output Techdocs | https://techdocs.zebra.com/datawedge/latest/guide/output/keystroke/ |
| 4 | Zebra DataWedge | Zebra DataWedge Getting Started | https://techdocs.zebra.com/datawedge/latest/guide/gettingstarted/ |
| 5 | PWA | Progressive Web Apps Vs Native Apps: What's the Best Choice in 2026?(Mobiloud) | https://www.mobiloud.com/blog/progressive-web-apps-vs-native-apps |
| 6 | PWA | PWA vs Native App: Ultimate Comparison Guide for 2026(testmuai) | https://www.testmuai.com/learning-hub/pwa-vs-native/ |
| 7 | PWA Barcode | QR Code Detection PWA Demo(Progressier) | https://progressier.com/pwa-capabilities/qr-code-and-barcode-reader |

### 8.4 利用上の注意

- 二次情報の多くは開発会社・ブログ記事で、自社サービス推奨のポジショントークを含む場合あり。複数ソースで相互検証済み。
- 市場シェア数値(Flutter 46% / RN 35% 等)は出典により異なる。トレンドの参考値として扱うこと。
- フレームワークのバージョン・性能特性は頻繁に更新される。実プロジェクト採用前に必ず公式ドキュメント最新版を確認すること。
- 日本国内の人材市場・料金感は上記二次情報(主に海外)と異なる場合があり、国内エージェント・求人媒体での別途調査を推奨。
- 業務アプリ要件(セキュリティ・監査・コンプライアンス)の詳細は各公式ドキュメント・エンタープライズサポート契約での確認が必要。

---

## 関連ドキュメント

### 言語・フレームワーク比較(`framework_comparison/`)

- [モバイルアプリ言語・フレームワーク 選定観点一覧](./framework_comparison/selection_criteria.md) — 要件/チーム/言語特性/エコシステム/ビジネス/**保守・ガバナンス**/**開発環境**の 7 カテゴリから選定観点を整理。ライセンス・開発元・リリースサイクル・LTS・EOL・商用サポート・IDE・ビルド・Hot Reload・CI/CD 等を含む。
- [フレームワーク/言語別 プロファイル集](./framework_comparison/framework_profiles.md) — 言語 7 種・フレームワーク 13 種の個別ファクトシート(開発元・ライセンス・最新版・リリース頻度・LTS・EOL・推奨IDE・ビルド・テスト・CI/CD・業務適性・公式リンク)。比較表の一次データソース。
- [詳細プロファイル: Android ネイティブ(Kotlin + Jetpack Compose)](./framework_comparison/profile_android_native.md) — 本件第一推奨 FW の深掘り版。バージョン履歴・JDK/Gradle/SDK 要件・HT 連携(DataWedge/EMDK)・MDM(App Config/Device Owner)・12 週学習ロードマップ・コスト見積もり・落とし穴・拡充参照リンク。
- [詳細プロファイル: Kotlin(言語)](./framework_comparison/profile_kotlin.md) — 言語単体の詳細版。H 節(言語特性深掘り観点、18項目)を反映し、パフォーマンス・メモリ管理・エラーハンドリング・メタプログラミング・FFI・ツーリング品質・AI補完相性・学習曲線形状・業界採用実績・ロードマップ・日本語リソース等を収録。
- [詳細プロファイル: Flutter](./framework_comparison/profile_flutter.md) — FW 単体の詳細版。アーキテクチャ(Engine + Framework + Impeller)・Widget ツリー・Dart 特性・Hot Reload・Windows 開発環境・HT連携(Zebra 公式 DataWedge デモ)・MDM・オフライン(Drift/Isar)・ML Kit・主要採用事例・学習曲線・落とし穴・ロードマップ等を収録。
- [詳細プロファイル: React Native + Expo](./framework_comparison/profile_react_native.md) — 新アーキ(JSI/Fabric/TurboModules/Bridgeless)・Hermes・Expo SDK/EAS Build/Update・**Windows から iOS ビルド(Mac 不要)**・HT連携(Dev Client 必須)・React Navigation・状態管理(Zustand/TanStack Query)・OTA(EAS Update)・主要採用事例(Meta/MS/Shopify/Discord/Expo 新規)を収録。
- [詳細プロファイル: Ionic + Capacitor](./framework_comparison/profile_ionic_capacitor.md) — Ionic Framework(Stencil.js Web Components)+ Capacitor(ネイティブランタイム)+ Angular/React/Vue 組合せ。WebView 性能制約・HT Wedge モード・Capacitor プラグイン(公式/Community/Capawesome)・Ionic Appflow OTA・PWA 同時展開・社内業務アプリ最適・jQuery+Java チームに最適(Vue 併用で 2〜3 週立ち上げ)を収録。
- [詳細プロファイル: Kotlin Multiplatform + Compose Multiplatform](./framework_comparison/profile_kmp.md) — KMP Stable(2023-11)・Compose MP iOS Stable(2024-05)・expected/actual・Source Set 構成・Kotlin/Native・SKIE・主要ライブラリ(Ktor/SQLDelight/Koin)・iOS 連携・本件 Android 永続では不採用推奨の根拠。
- [言語の活用範囲 と ネイティブ機能アクセス・代表ライブラリ](./framework_comparison/framework_native_features.md) — (1) 各言語で Android 以外に何が作れるか(Web/サーバ/デスクトップ/IoT等)、(2) カメラ・バーコード・NFC・BLE・GPS・生体認証・プッシュ通知・DB・MDM・印刷・ML 等 23 カテゴリ × 主要 FW の代表ライブラリ対応表。

### PWA vs ネイティブ比較(`pwa_vs_native/`)

- [PWA vs ネイティブアプリ 選定観点 詳細](./pwa_vs_native/pwa_vs_native_criteria.md) — 15 カテゴリ 118 項目(配布/更新/性能/**HW 機能 22 項目**(赤外線・シリアル・UWB・Zigbee等含む)/オフライン/BG/セキュリティ/UX/開発/MDM/ポリシー/ビジネス/規制/地域/ハイブリッド)。PWA 不可の足切り 22 項目と決定フロー、典型シナリオ別推奨、本件適用結果。
- [PWA vs ネイティブ アーキテクチャ比較(PlantUML)](./pwa_vs_native/architecture_comparison.puml) — 6 種類の図を収録: レイヤ構造(deployment)、カメラアクセスフロー(sequence)、HT スキャナ連携、選定決定フロー(activity)、HW アクセス対応マトリクス、更新・配信フロー。

---

## 更新履歴

- **2026-04-23**: 初版作成(`mobile_framework_comparison_7.xlsx` 全7シートから Markdown 化)。
- **2026-04-23**: 選定観点一覧を `selection_criteria.md` に別ドキュメントとして分離。
