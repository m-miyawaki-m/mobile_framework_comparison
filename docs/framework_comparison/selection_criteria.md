# モバイルアプリ言語・フレームワーク 選定観点一覧

本ドキュメントは `android_business_app_framework_comparison.md` を補完する選定観点集である。
要件・チーム・技術特性・エコシステム・ビジネス・**保守/ガバナンス**・**開発環境**・**言語特性深掘り** の 8 カテゴリに分けて整理する。

- 情報は **2026年4月時点**
- 本ドキュメントの主目的は、**実際に選定判断を下す際のチェックリストとしての再利用性**
- **言語/FW 別の一次データは [`framework_profiles.md`](./framework_profiles.md) を参照**。本ファイルの各比較表はそこから導出した二次物

---

## A. 要件サイド(プロジェクトから来る観点)

| # | 観点 | 具体的な問い |
|---|---|---|
| A1 | ターゲットOS | Androidのみ / +iOS / +Web / +Desktop |
| A2 | プロジェクト規模・保守期間 | 〜10画面/〜半年 / 10〜50画面/1〜2年 / 50+画面/5年+ |
| A3 | UI要件 | ブランド統一 / OS純正UX / 標準フォーム / 高負荷アニメ・3D |
| A4 | ハードウェア連携 | カメラ程度 / BT・センサ / スキャナSDK / NFC/RFID/プリンタ |
| A5 | オフライン耐性 | 常時接続OK / 断続的接続 / 堅牢オフライン必須 |
| A6 | バックグラウンド処理 | 不要 / 軽量同期 / 長時間常駐(8h) |
| A7 | セキュリティ・コンプライアンス | 一般 / 金融・医療規制 / 生体・証明書認証 |
| A8 | 配布・運用 | Store / MDM / APK直配布 / OTA必要度 |
| A9 | OSアップデート追従速度 | 新OS機能を即時取り込む必要があるか |
| A10 | Web同時展開の有無 | 現状・将来、コード資産共有の要否 |

---

## B. チーム・組織サイド(ヒトから来る観点)

| # | 観点 | 具体的な問い |
|---|---|---|
| B1 | 既存チームスキル | Kotlin / Java / JS+jQuery / React / Vue / Angular / Dart / C# |
| B2 | 既存コード資産 | Androidネイティブ / Webアプリ / Reactアプリ / グリーンフィールド |
| B3 | 言語移行コスト | Java→Kotlin=極小 / Java→Dart=小 / Java→TS=小〜中 / Java→素のJS=中 |
| B4 | 国内人材プール | React ≫ Kotlin ≫ Vue > Flutter > Angular > KMP > NativeScript |
| B5 | 採用難易度/市場厚み | JS系=厚い、Kotlin=安定、Dart=増加中、KMP=薄い |
| B6 | 学習コスト・立ち上げ期間 | 極小(1〜2週)/ 小(2〜4週)/ 中(1〜2ヶ月)/ 大(2〜3ヶ月) |
| B7 | パラダイム適応コスト | 宣言的UI、関数型(Hooks)、非同期(Promise/Future/Flow) |
| B8 | モバイル経験者の有無 | 1名確保できるか(リスク対策) |

---

## C. 言語特性サイド(テクノロジから来る観点)

| # | 観点 | 具体的な問い |
|---|---|---|
| C1 | 型安全性 | TS ○ / Kotlin・Dart(null safety)◎ / 素のJS × |
| C2 | OOP・DI親和性 | Java/Spring経験 → Kotlin(Hilt)が直系、Angular(TS)も近い |
| C3 | 並行・非同期モデル | Kotlin coroutine/Flow / Dart Future/Stream / JS Promise/async |
| C4 | コンパイル方式 | AOT(Kotlin/Dart/Swift=起動速い)/ JIT(Java)/ 動的(JS) |
| C5 | 言語の成熟度・後方互換 | Java/Kotlin/TS=成熟、Dart=安定、Swift=速い進化 |
| C6 | 言語のLTS・後援企業 | Kotlin(JetBrains+Google)/ Dart(Google)/ TS(Microsoft) |

---

## D. エコシステム・開発体験サイド

| # | 観点 | 具体的な問い |
|---|---|---|
| D1 | ライブラリ数/品質 | npm(最大)> pub.dev > Maven Central > 各社SDK |
| D2 | HWベンダーSDK前提言語 | Zebra/Honeywell/Datalogic = Java/Kotlin 前提 |
| D3 | ビルド・依存管理 | Gradle(Java/Kotlin/Flutter=CI構築素直)/ npm / CocoaPods |
| D4 | IDE・ツール | Android Studio / Xcode / VS Code / IntelliJ |
| D5 | テスト基盤 | JUnit / flutter_test / Jest・Vitest / Jasmine+TestBed |
| D6 | Hot Reload / Fast Refresh | Flutter=高速、RN/Expo=Fast Refresh、ネイティブ=Live Edit |
| D7 | AI補完(LLM)との相性 | 学習データ量順: JS/TS ≧ Java/Kotlin > Dart、型情報多い言語が精度高い |

---

## E. 長期保守・ビジネスサイド

| # | 観点 | 具体的な問い |
|---|---|---|
| E1 | FW本体のLTSと端末寿命(5〜7年)整合 | ネイティブ > Flutter/KMP > RN > Ionic/MAUI |
| E2 | Android Target SDK 強制引き上げ追従速度 | 主要FWは追従、Ionic/MAUIは遅延可能性 |
| E3 | サードパーティ依存度 | Ionic/RN(多く品質ばらつき)> Flutter(pub.dev公式度高)> ネイティブ/KMP(最小) |
| E4 | TCO(教育+開発+保守) | 学習コスト×期間、人材単価、保守メンバー入替時コスト |
| E5 | ポジショントーク排除 | 複数ソースで相互検証、国内市場は別途確認 |

---

## F. 保守・ガバナンス観点(保守的視点)

### F1. 言語習熟難易度(Java経験者を基準とした段階別期間)

| 言語 | 基礎習得(構文・型) | 実務レベル(設計可能) | エキスパート(規約策定) | 備考 |
|---|---|---|---|---|
| Kotlin | 1〜2週 | 1〜2ヶ月 | 6ヶ月〜1年 | Android Studio の Java→Kotlin 自動変換あり |
| Dart | 1〜2週 | 2〜3ヶ月 | 1年〜 | Java構文親和、Future/Stream/Widget Tree が壁 |
| TypeScript | 2〜3週 | 2〜3ヶ月 | 1年〜 | 型+JS動的性+ビルドツール |
| JavaScript(素) | 1〜2週 | 1〜2ヶ月 | 6ヶ月〜 | this束縛・プロトタイプで混乱しやすい |
| Swift | 2〜3週 | 2〜3ヶ月 | 1年〜 | iOS SDK理解と一体 |
| C#(.NET) | 1〜2週 | 1〜2ヶ月 | 6ヶ月〜1年 | Javaに最も近い |

「エキスパート」= チームに技術規約を敷けて、コードレビューで品質担保できるレベル。

### F2. ライセンスと商用制約

| 対象 | ライセンス | 商用制約 |
|---|---|---|
| Kotlin | Apache 2.0 | なし |
| Dart | BSD-3-Clause | なし |
| Java(OpenJDK) | GPLv2 + Classpath Exception | なし(ディストリに注意) |
| Java(Oracle JDK) | Oracle NFTC(17+)/ Oracle Technology Network License(11以下) | 本番用途は条件付き無料、商用サポートは有償契約 |
| TypeScript | Apache 2.0 | なし |
| Swift | Apache 2.0 | なし |
| C# / .NET | MIT | なし |
| Flutter | BSD-3-Clause | なし |
| React Native | MIT | なし(旧 Meta 特許条項は撤廃済) |
| React | MIT | なし |
| Vue | MIT | なし |
| Angular | MIT | なし |
| Ionic Framework | MIT | 公式サポート契約は有償 |
| Capacitor | MIT | 同上 |
| Expo SDK | MIT | EAS Build/Update は有料プラン |
| NativeScript | Apache 2.0 | なし |
| KMP / Compose MP | Apache 2.0 | なし |
| Jetpack Compose | Apache 2.0 | なし |
| .NET MAUI | MIT | なし |
| Unity | 商用(Personal 無料枠 + Pro/Enterprise 有償) | 売上・従業員規模で有償義務、**2023年のランタイム料金騒動の前例あり** |
| Unreal Engine | Epic EULA | 年間売上 100万ドル超で 5% ロイヤリティ |
| Cordova | Apache 2.0 | なし |
| Xamarin | MIT | **EOL(2024-05-01)** |

### F3. 開発元・ガバナンス・Bus Factor

| 対象 | 主開発元 | ガバナンス | Bus Factor |
|---|---|---|---|
| Kotlin | JetBrains(Googleが後援) | 単一企業主導 + Kotlin Foundation | 中〜高 |
| Dart | Google | 単一企業主導 | Google の戦略依存 |
| Java | OpenJDK コミュニティ(Oracle 主導、IBM/Red Hat/Azul 参画) | 多ベンダー体制 | **最高**(JEP + JCP) |
| TypeScript | Microsoft | 単一企業主導 + オープン設計 | 高 |
| Swift | Apple(+ Swift Community) | 企業主導 + SE-XXXX 提案制度 | 中〜高 |
| C# / .NET | Microsoft(.NET Foundation) | 企業主導 + 財団 | 高 |
| Flutter | Google | 単一企業主導 | **Google 戦略依存(最大の保守リスク)** |
| React Native | Meta + Microsoft + Callstack + Expo 他 | 企業複数 + コミュニティ | 高 |
| React | Meta | 企業主導 | 高(利用規模が極大) |
| Vue | Vue.js(Evan You + 小規模チーム) | **個人 + 小チーム依存** | 中(スポンサー財源) |
| Angular | Google | 単一企業主導 | 中〜高 |
| Ionic / Capacitor | Ionic(営利企業、資金調達済) | 単一企業 | 中(企業存続性に依存) |
| NativeScript | OpenJS Foundation(元 Progress Software) | 財団化済みだがモメンタム低 | **低**(要注意) |
| KMP / Compose MP | JetBrains | 単一企業 | 中〜高 |
| Jetpack Compose | Google | Android プラットフォーム一部 | 高 |
| .NET MAUI | Microsoft | 企業 + 財団 | 高 |

### F4. リリース・アップデートサイクル

| 対象 | メジャー頻度 | マイナー/パッチ | コメント |
|---|---|---|---|
| Kotlin | 年 1〜1.5(2.0/2.1/2.2…) | 6ヶ月 + バグ修正随時 | JetBrains の予測可能なロードマップ |
| Dart/Flutter | 四半期(3.x刻み) | 月次 beta | Flutter と同期 |
| Java | 6ヶ月メジャー | 四半期セキュリティ更新 | LTS は 2 年毎(8/11/17/21/25…) |
| TypeScript | 3〜4ヶ月(x.y 刻み) | 随時 | 予測可能、破壊的変更は限定的 |
| Swift | 年 1 メジャー(x.0) | 年中 x.y 複数 | Xcode/iOS SDK と同期 |
| .NET / C# | 年 1 メジャー(毎年 11 月) | 月次パッチ | 偶数バージョンが LTS |
| Flutter | 四半期 stable | 月次 beta/patch | 明示 LTS なし |
| React Native | 2〜3ヶ月で新マイナー | 随時 patch | 直近数バージョンのみサポート |
| Expo SDK | 四半期 | 随時 | 直近 3 SDK 程度サポート |
| React | 不定期(メジャー数年おき) | 随時 | **後方互換を強く重視** |
| Vue | 不定期(3.x マイナー随時) | — | Vue 2→3 は事実上再書 |
| Angular | 6ヶ月メジャー | 随時 | **最も規則的** |
| Ionic Framework | 年 1 メジャー前後 | 随時 | |
| Capacitor | 半年〜年 1 | 随時 | Android/iOS SDK に追従 |
| KMP | Kotlin と同期 | 同上 | |
| Jetpack Compose | AndroidX(月次〜四半期) | — | |
| .NET MAUI | .NET と同期(年 1) | 月次 | |

### F5. LTS・サポート期限・EOL

| 対象 | LTS の有無 | サポート期間 | 既知の EOL |
|---|---|---|---|
| Kotlin | なし(事実上後方互換維持) | — | — |
| Dart | なし | — | — |
| Java | あり | LTS 毎に 6 年+(ベンダー別: Oracle/Red Hat/Azul/Amazon Corretto/Temurin) | Java 8 は 2030 年まで延長サポート可(ベンダー別) |
| TypeScript | なし | — | — |
| .NET | あり | LTS=3 年 / STS=18ヶ月 | .NET 6=2024-11、.NET 7=2024-05、.NET 8=2026-11 |
| Flutter | **明示 LTS なし** | 概ね 3〜6ヶ月 | — |
| React Native | **明示 LTS なし** | 直近 6 マイナー程度 | — |
| Angular | あり | 6ヶ月 Active + 12ヶ月 LTS = **計 18ヶ月** | v16 以前は順次 EOL |
| Vue | — | Vue 3 が現行 | **Vue 2 = 2023-12-31 EOL 済** |
| Xamarin | あり(旧) | — | **2024-05-01 EOL 済 → .NET MAUI へ** |
| Cordova | — | プロジェクト縮小中 | 新規非推奨 |
| PhoneGap | あり(旧) | — | **2020 EOL 済** |
| KMP | あり(Stable 到達 2023) | — | — |
| .NET MAUI | .NET に追従 | .NET 8 LTS= 2026-11 等 | — |

### F6. 破壊的変更頻度と後方互換ポリシー

| 対象 | 破壊的変更の発生頻度 | 傾向 |
|---|---|---|
| Java | 極めて低い | 「Write Once, Run Forever」の文化 |
| TypeScript | 低〜中 | strict フラグ追加等で型が厳密化、コード修正要 |
| Kotlin | 中 | 2.0 で K2 コンパイラ切替、言語仕様の進化継続 |
| Dart | 中 | 3.0 で null safety 強制、Future Core 移行 |
| React | 低 | フレームワーク中最も後方互換重視 |
| Vue | **高**(Vue 2→3 は事実上再書) | Composition API 化 |
| Angular | 中 | v 毎に API 変更、Material/RxJS 追従要、`ng update` でマイグレ補助 |
| Flutter | 中〜高 | API が頻繁に改定、大規模 Migration Guide あり |
| React Native | **高** | 新アーキ(JSI/Fabric)移行、ライブラリ互換性問題 |
| Expo | 中 | SDK バージョンごとにライブラリ入替 |
| Swift | 中 | バージョン毎の言語機能追加 |
| .NET | 中 | LTS 毎に API 整理 |

### F7. 脆弱性対応・CVE 体制

| 対象 | 窓口 | 対応スピード |
|---|---|---|
| OpenJDK | Oracle Critical Patch Update(四半期)+ 各ディストリ | 迅速(標準的) |
| Android/Jetpack | Android Security Bulletin(月次)+ AndroidX | 迅速 |
| Kotlin | JetBrains Security | 迅速 |
| Flutter / Dart | Google セキュリティチーム + security@flutter.dev | 迅速 |
| React Native | GitHub Security Advisories | 迅速 |
| Angular | Angular Security | 迅速 |
| Vue | Vue Security | 中程度 |
| npm ライブラリ群 | 各作者依存 | **玉石混交**(ここが実質的な弱点) |
| Capacitor プラグイン | 同上 | 同上 |
| Ionic 商用契約 | SLA 付きの有償サポートあり | 契約次第 |

### F8. エンタープライズサポート・商用契約

| 対象 | 公式商用サポート |
|---|---|
| Oracle JDK | Oracle Java SE Subscription |
| Red Hat Build of OpenJDK | Red Hat Enterprise Linux 契約 |
| Azul Platform Core/Prime | Azul 商用契約 |
| Amazon Corretto | AWS サポート契約 |
| Kotlin / IntelliJ | JetBrains 商用契約 |
| Flutter | **Google の直接商用サポートなし**(Flutter Partner 経由) |
| React Native | **Meta 直接契約なし**(Callstack 等コンサルが主) |
| Angular / Dart / TypeScript | ベンダー直接の商用 SLA なし |
| Ionic | Ionic Enterprise 契約(公式) |
| .NET / MAUI | Microsoft サポート契約 |
| Unity | Unity Pro/Industry で直接サポート |
| KMP | JetBrains ビジネス契約 |

OSSのデフォルト状態では「コミュニティ SLA=0」。金融・公共・医療案件では **商用サポート契約できる選択肢** が重要。

### F9. 日本語リソース・国内コミュニティ

| 対象 | 国内コミュニティ | 書籍・学習資料 |
|---|---|---|
| Java | 極めて厚い(JJUG 等) | 豊富 |
| Kotlin | 厚い(Kotlin もくもく会他) | 豊富 |
| React / Vue / Angular | 厚い | 豊富(React が No.1) |
| TypeScript | 厚い | 豊富 |
| Flutter | 中程度(flutter.jp、FlutterKaigi) | 近年増加 |
| React Native | 薄〜中 | 限定的 |
| Dart | 薄い | Flutter 経由の情報のみ |
| KMP / Compose MP | 薄い | 海外記事頼り |
| .NET MAUI | 中程度 | Microsoft 社内ツール向け中心 |
| NativeScript | 極薄 | ほぼ皆無 |
| Ionic | 中 | レガシー情報が多い |

### F10. EOL・後継パス

| 対象 | 状態 | 後継パス |
|---|---|---|
| Xamarin | 2024-05 EOL | **.NET MAUI** |
| Vue 2 | 2023-12 EOL | Vue 3(要リライト) |
| PhoneGap | 2020 EOL | Cordova → Capacitor |
| Cordova | 活動縮小 | Capacitor |
| AngularJS(v1) | 2022 EOL | Angular(v2+)※別物 |
| Java 8 | ベンダー別延長(〜2030 年代) | Java 11/17/21 LTS |
| Android SDK 旧 | Google Play Target SDK 要件で**強制引き上げ**(毎年) | 都度追従 |

---

## H. 言語特性 深掘り観点(言語選定用、C の拡張)

C 節は言語一般の概要を扱うが、本節は **言語固有の内部特性** を選定判断に反映させるための深掘り観点。個別言語の詳細値は `profile_<言語名>.md` 参照。

| # | 観点 | 具体的な問い |
|---|---|---|
| H1 | パフォーマンス特性 | 起動時間、スループット、メモリ、GC pause はどれくらい? |
| H2 | メモリ管理モデル | GC / RC / ARC / Ownership のどれ? 業務用途で問題になるか? |
| H3 | エラーハンドリング哲学 | Exception / Result / Option / Panic、チーム文化と合うか? |
| H4 | 標準ライブラリの範囲 | 言語単体でどこまで可能か、外部依存をどれだけ避けられるか |
| H5 | メタプログラミング | Annotation Processing / マクロ / Reflection / Source Gen |
| H6 | 相互運用性・FFI | 他言語・ネイティブとどう連携するか |
| H7 | デバッグ体験 | スタックトレース可読性、デバッガ品質、プロファイラ |
| H8 | ツーリング品質 | LSP 精度、補完、リファクタリング、Navigate-to-definition |
| H9 | 業界・ドメイン別採用実績 | Fintech / SI / ゲーム / 組込 / 官公庁 等での実績 |
| H10 | AI/LLM 補完相性 | 学習データ量、型情報活用、誤補完傾向 |
| H11 | コミュニティ活発度指標 | GitHub stars、SO tag 活動、Dev Survey 順位 |
| H12 | 日本語教材・書籍・認定 | 国内教育リソースの充実度 |
| H13 | コンパイル速度 | ビルド時間、CI 時間への影響 |
| H14 | 配布バイナリサイズ | アプリサイズへの寄与 |
| H15 | 言語ロードマップ | 公式が示す中期計画 |
| H16 | コーディング規約・スタイル文化 | 公式スタイルガイド、linter デフォルトの厳しさ |
| H17 | 言語の設計哲学 | 安全性重視 / 表現力重視 / 簡潔さ重視 |
| H18 | 学習曲線の形状 | 最初楽→後キツ / 最初キツ→後楽 / 常に中程度 |

**関連プロファイル**:

- [`profile_kotlin.md`](./profile_kotlin.md)(雛形・本件第一推奨)
- (TypeScript / Dart / Java は Kotlin プロファイル確認後に順次作成予定)

---

## G. 開発環境観点(開発マシンから見た実務的視点)

### G1. 推奨 IDE・エディタ

| 対象 | 主 IDE | 代替 | コメント |
|---|---|---|---|
| ネイティブ Android (Kotlin) | **Android Studio**(公式、IntelliJ IDEA Community ベース、無償) | IntelliJ IDEA Ultimate(商用) | Google が毎年複数回更新、Gradle 同梱 |
| Flutter / Dart | **Android Studio + Flutter plugin** / **VS Code + Flutter extension** | IntelliJ IDEA | VS Code 派が多数派、両方公式サポート |
| React Native | **VS Code** | WebStorm、Android Studio(Android 部)、Xcode(iOS 部) | ネイティブビルドは結局 Android Studio/Xcode が必要 |
| Expo | VS Code | 同上 | Expo Go / Dev Client で実機テストが主 |
| Ionic + Capacitor | **VS Code**(Ionic 公式拡張あり) | WebStorm、IntelliJ(Angular用) | Web 開発と同じツールチェーン |
| KMP + Compose MP | **Android Studio(KMP plugin)** / IntelliJ IDEA | Fleet(JetBrains 新 IDE) | iOS UI は Xcode 併用 |
| .NET MAUI | **Visual Studio 2022**(Win)/ VS for Mac(EOL)/ VS Code + C# Dev Kit | Rider(JetBrains) | Win 中心、Mac は iOS ビルド時 |
| Unity | **Unity Editor** + VS Code/Rider | Visual Studio | 別世界のツールチェーン |

### G2. ビルドツール・依存管理

| 対象 | ビルド | 依存管理 | 特徴 |
|---|---|---|---|
| Android ネイティブ | Gradle(Kotlin DSL / Groovy DSL) | Gradle / Maven Central | 業務アプリ定番、CI 構築が素直 |
| Flutter | `flutter build`(内部で Gradle/Xcode) | `pub` / pub.dev | 単一 CLI で一貫 |
| React Native | Metro bundler + Gradle + Xcode | npm / yarn / pnpm | 多層で複雑、EAS で抽象化可 |
| Expo | EAS Build(クラウド) or ローカル | npm / yarn / pnpm | **Mac 不要で iOS ビルド可** が強み |
| Ionic + Capacitor | npm + Capacitor CLI + Gradle/Xcode | npm / yarn / pnpm | Web ビルド + ネイティブラッパ |
| KMP | Gradle(Kotlin DSL) | Gradle / Maven Central | Kotlin ネイティブと一貫 |
| .NET MAUI | MSBuild / dotnet CLI | NuGet | Visual Studio 統合が強力 |

### G3. 開発マシン OS 要件(iOS ビルドの Mac 依存度)

| 対象 | Windows | macOS | Linux | iOS ビルド |
|---|---|---|---|---|
| Android ネイティブ | ◎ | ◎ | ◎ | — |
| Flutter(Android/Web) | ◎ | ◎ | ◎ | Android/Web は不要 |
| Flutter(iOS)| × | ◎ | × | **Mac 必須**(または Codemagic/GitHub Actions macOS runner) |
| React Native(Android)| ◎ | ◎ | ◎ | — |
| React Native(iOS)| × | ◎ | × | **Mac 必須**(または EAS Build) |
| **Expo(iOS)**| **◎** | ◎ | **◎** | EAS Build で **Mac 不要**(本件で Windows 開発時の強い利点) |
| Ionic + Capacitor(Android)| ◎ | ◎ | ◎ | — |
| Ionic + Capacitor(iOS)| × | ◎ | × | **Mac 必須** |
| KMP(Android)| ◎ | ◎ | ◎ | — |
| KMP(iOS)| × | ◎ | × | **Mac 必須**(Xcode が必要) |
| .NET MAUI(Android/Win)| ◎ | ◎ | — | — |
| .NET MAUI(iOS)| △(リモート Mac 経由) | ◎ | × | **Mac 必須** |

**Windows 開発チームがiOS対応する場合の現実解**: Expo EAS Build、Codemagic、GitHub Actions の macOS runner、MacinCloud、Bitrise等クラウドビルド。**Expo は Mac 不要で iOS 開発できる唯一のスマートな選択肢**。

### G4. 初期セットアップ・ディスク占有量(概算)

| 対象 | セットアップ手順の複雑度 | ディスク占有 | 初期ビルド時間 |
|---|---|---|---|
| Android ネイティブ | 中(Android Studio + SDK) | 15〜30 GB | 数分 |
| Flutter(Android のみ) | 中 | 20〜40 GB | 数分 |
| Flutter(Android+iOS) | 高(+ Xcode) | 50〜80 GB | 5〜10 分 |
| React Native(Android のみ) | **高**(Node + SDK + Metro) | 15〜30 GB | 初回 5〜15 分(Gradle + npm) |
| Expo Managed | **低**(Node + Expo CLI) | 5〜10 GB | 数分(EAS ビルドはクラウド) |
| Ionic + Capacitor | 低〜中 | 5〜15 GB | 数分(Web ビルド主) |
| KMP | 高(Android Studio + Xcode + KMP plugin) | 50〜80 GB | 10〜15 分 |
| .NET MAUI | 高(Visual Studio + Workloads) | 40〜60 GB | 5〜10 分 |

### G5. ホットリロード・開発フィードバックループ

| 対象 | 機能名 | 状態保持 | 速度 | 制約 |
|---|---|---|---|---|
| Flutter | **Hot Reload** | ◎ | 最速(< 1 秒) | State 再初期化が必要なケースは Hot Restart |
| React Native | **Fast Refresh** | ◎ | 高速 | Hooks の構造変更時は再マウント |
| Expo | Fast Refresh(RN と同じ) | ◎ | 高速 | Expo Go で簡単実機確認 |
| Ionic(WebView) | Vite/Webpack HMR + Live Reload | ◎ | 高速 | Web 開発と同じ感覚 |
| Android ネイティブ(Compose) | **Live Edit**(実験的)/ **Apply Changes**(中間) | △ | 中 | 制約多く、完全な Hot Reload 相当は未達 |
| KMP | Android 側 Apply Changes | △ | 中 | Compose MP で改善中 |
| .NET MAUI | **.NET Hot Reload** / **XAML Hot Reload** | ◎ | 中〜高 | |
| SwiftUI(参考) | Preview / Hot Reload | ◎ | 中 | |

**Hot Reload の完成度は Flutter が最強、次いで RN/Expo と Ionic**。ネイティブは改善中だが届いていない。

### G6. エミュレータ・シミュレータ・実機デバッグ

| 対象 | エミュ/シム | 実機デバッグ | ログ | リモートデバッグ |
|---|---|---|---|---|
| Android ネイティブ | Android Emulator(公式) | ADB + USB/Wi-Fi | **Logcat**(Android Studio 統合) | Firebase Test Lab / AWS Device Farm |
| Flutter | Android Emu / iOS Sim / Web / Desktop | 各 OS | `flutter logs` + DevTools | Firebase Test Lab |
| React Native | Android Emu / iOS Sim | 各 OS | Metro ログ + RN DevTools | Firebase Test Lab |
| Expo | Expo Go(実機、最速)/ Android Emu / iOS Sim | 同上 | Expo DevTools | EAS Build の QR で即配布 |
| Ionic + Capacitor | Android Emu / iOS Sim / **ブラウザ(最速)** | 各 OS | Chrome DevTools(Android WebView 経由)/ Safari Web Inspector(iOS) | — |
| KMP | Android Emu / iOS Sim | 各 OS | Logcat / Xcode Console | — |
| .NET MAUI | Android / iOS / Windows / Mac Catalyst | 各 OS | Visual Studio Debugger | App Center |

### G7. プロファイラ・DevTools

| 対象 | 公式プロファイラ |
|---|---|
| Android ネイティブ | **Android Studio Profiler**(CPU / Memory / Network / Energy / Power Rail) |
| Flutter | **Flutter DevTools**(ブラウザ統合、Performance / Memory / Network / Widget Inspector / CPU Sampler) |
| React Native | **React Native DevTools**(Chrome DevTools ベース、新アーキ標準)、React DevTools |
| Ionic(WebView) | Chrome DevTools / Safari Web Inspector |
| KMP | Android Studio Profiler(Android側)+ Xcode Instruments(iOS側) |
| .NET MAUI | Visual Studio Diagnostic Tools、App Center Analytics |

**Flipper(RN 旧推奨) は 2024 年以降メンテ縮小、新規は RN DevTools を使うこと。**

### G8. Lint / Formatter / 静的解析

| 対象 | Linter | Formatter | 静的解析 |
|---|---|---|---|
| Kotlin | **ktlint**、**Detekt**、Android Lint | ktlint format | Android Studio Inspection |
| Java | Checkstyle、SpotBugs、PMD | google-java-format | SonarQube |
| Dart / Flutter | **flutter_lints**、analyzer | `dart format` | `dart analyze` |
| TypeScript / JS | **ESLint**、@typescript-eslint | **Prettier** | tsc strict |
| Swift | **SwiftLint** | swift-format | SwiftLint |
| C# | **Roslyn Analyzers**、StyleCop | dotnet format | SonarQube、Resharper |
| Ionic(Angular) | ESLint + @angular-eslint | Prettier | TSLint(レガシー) |
| Vue / React(+ Ionic) | ESLint + eslint-plugin-vue / react | Prettier | tsc |

### G9. テスト実行環境

| 対象 | Unit | Widget/UI | E2E |
|---|---|---|---|
| Android ネイティブ | JUnit / MockK | **Espresso** / **Compose Test** | Maestro / Appium |
| Flutter | **flutter_test** | **Widget Test** | **integration_test** / patrol / Maestro |
| React Native | **Jest** | **React Native Testing Library** | **Detox** / Maestro / Appium |
| Expo | Jest | RNTL | Detox / Maestro(Bare)、Expo Custom |
| Ionic(Angular) | **Jasmine + Karma**(旧)/ **Jest** | Angular TestBed | Cypress / Playwright / Appium |
| Ionic(Vue/React) | Vitest / Jest | Vue Test Utils / RTL | 同上 |
| KMP | kotlin-test / JUnit / XCTest | — | Platform 別 |
| .NET MAUI | xUnit / NUnit / MSTest | MAUI UI Tests | Appium |

### G10. CI/CD・クラウドビルドサービス

| サービス | 対応FW | 特徴 | 備考 |
|---|---|---|---|
| **GitHub Actions** | 全て(macOS/Linux/Win runner) | 汎用、無償枠あり | macOS runner は料金高め、iOS ビルドコストに注意 |
| **Bitrise** | 全モバイル | モバイル特化、Steps ライブラリ豊富 | 業務アプリでの採用多 |
| **Codemagic** | Flutter特化(他も対応) | Flutter向けに最適化 | Flutter チーム推奨 |
| **EAS Build** | Expo(RN) | Expo 公式、**Windows から iOS ビルド可** | Expo プロジェクトの第一選択 |
| **App Center** | RN / MAUI / Native / Xamarin | Microsoft、配信・分析統合 | 2025-03 で廃止発表 → Visual Studio App Center 後継案内 |
| **Ionic Appflow** | Ionic / Capacitor | Ionic 公式、Live Update 含む | Ionic Enterprise 契約で強化 |
| **Google Play Console Internal Testing** | Android 全般 | ストア経由の段階配信 | |
| **Firebase App Distribution** | Android/iOS 全般 | TestFlight 相当の社内配布 | |
| **Jenkins / CircleCI / GitLab CI** | 全般 | オンプレ/クラウド柔軟 | macOS runner は別途確保要 |
| **Azure DevOps Pipelines** | .NET MAUI / RN / Native | Microsoft 系統合 | |

### G11. SDK・ランタイムの複数バージョン管理

| 対象 | 複数バージョン管理ツール |
|---|---|
| Android SDK | `sdkmanager`(公式 CLI)、Android Studio SDK Manager |
| Kotlin / JVM | **SDKMAN!**、asdf、Gradle 側で固定 |
| Node.js(RN/Expo/Ionic) | **nvm**(Mac/Linux)/ **nvm-windows** / **Volta** / **fnm** / asdf |
| Flutter SDK | **fvm**(Flutter Version Management)、asdf |
| Ruby(iOS の CocoaPods 用) | **rbenv** / rvm / asdf |
| Python(ビルドスクリプト用) | **pyenv** / asdf |
| .NET | `dotnet-install`、Visual Studio Installer で併存 |

実運用では **asdf** または **mise** に統一すると Node/Java/Ruby/Python/Flutter を 1 ツールで管理可能。

### G12. 依存脆弱性スキャン・SBOM

| 対象 | 公式/標準ツール |
|---|---|
| Android / Gradle | **Gradle Dependency Check**、OWASP Dependency-Check、GitHub Dependabot |
| npm(RN/Expo/Ionic) | **npm audit**、**pnpm audit**、Snyk、Socket、Dependabot |
| pub.dev(Flutter) | **`dart pub outdated`**、GitHub Dependabot(Flutter対応) |
| NuGet(.NET) | **`dotnet list package --vulnerable`**、Dependabot |
| 汎用 | **Renovate**、**Socket**、GitHub Advisory Database |
| SBOM 生成 | CycloneDX Gradle/npm plugin、Syft |

### G13. Windows 開発環境での実質制約と推奨

本件想定(Windows 開発マシン + Android 業務アプリ + jQuery/Java チーム)において:

| 観点 | ネイティブ Android | Flutter | Expo(RN) | Ionic+Capacitor | KMP | .NET MAUI |
|---|---|---|---|---|---|---|
| Windows でのAndroid開発 | ◎ | ◎ | ◎ | ◎ | ◎ | ◎ |
| ローカルビルドの軽さ | ○ | ○ | **◎(ほぼ Web 感覚)** | ◎ | △(重い) | ○ |
| Hot Reload の使い勝手 | △(Live Edit 制約多) | **◎** | ◎ | ◎ | △ | ○ |
| ディスク占有 | 中 | 中〜大 | **小** | **小** | 大 | 大 |
| iOS 対応時の追加 Mac 要否 | — | Mac 必須 | **Mac 不要(EAS)** | Mac 必須 | Mac 必須 | Mac 必須 |
| 既存 VS Code / npm 資産流用 | × | ○ | ◎ | **◎** | × | △ |
| セットアップの素直さ | ○(Android Studio 一択) | ○ | ◎ | ◎ | △(KMP plugin 追加要) | ○(VS 2022) |

**結論**: Windows + Android 永続なら **ネイティブ Android** または **Ionic+Capacitor(+Vue)** が開発環境面で最もストレスが少ない。将来 iOS 展開の可能性がある場合は **Expo (RN)** が Windows 開発時に唯一 Mac 不要で iOS ビルド可能な利点がある。

---

## 選定判別フロー(保守的視点の足切り例)

1. **BUS FACTOR 足切り** — 企業単独後援のみ、かつコミュニティも小規模 → リスク高(例: NativeScript、Dart/Flutter は「Google 依存」)
2. **LTS 足切り** — 長期保守案件(5 年+)で明示 LTS が無い → Flutter/RN はリリース毎追従を運用計画に織り込み必須
3. **ライセンス足切り** — Unity のような **一方的な商用条件変更の前例** があるものは業務アプリ基盤として要注意
4. **商用サポート足切り** — SLA が必要な業界(金融/医療/公共) → Java 商用ディストリ / Ionic Enterprise / .NET / Unity Pro のいずれか
5. **破壊的変更足切り** — 内製チーム規模が小さい場合、Vue 2→3 型のメジャー再書き対応は負担大 → 後方互換重視 FW(React/Angular)が無難
6. **習熟難易度足切り** — 業務委託や人員入替えが多い場合、Kotlin/C#(Java 親和)や TypeScript(型情報あり)が再学習負荷小

---

## 観点使用の経験則

1. **最初に絞り込む**: A1(ターゲットOS)→ A2(規模・保守期間)→ B1/B2(既存スキル・資産)
2. **次に弾く**: A3〜A7(UI・HW・オフライン・セキュリティ)で満たせない FW/言語を除外
3. **最後に比較**: B3〜B6(学習・採用)と C1〜C6(言語特性)で同点決勝
4. **長期視点**: E1〜E4(LTS・追従・依存度・TCO)と F1〜F10(ガバナンス)で逆転判断あり

典型的な逆転例:

- 「Android永続」確定 → クロスプラットフォーム系の最大価値が消失、Kotlin が強く有利に
- 「既存Web+jQuery資産」確定 → TypeScript(Vue+Ionic)がスキル連続性で有利
- 「20画面規模」確定 → Angular(TS)の重装備は過剰
- 「金融・公共案件」 → F8(商用サポート)が最上位の足切り条件に

---

## 参照リンク(F. 保守・ガバナンス観点の裏付け)

### 言語

| # | 対象 | URL |
|---|---|---|
| F-1 | Kotlin リリース履歴 | https://kotlinlang.org/docs/releases.html |
| F-2 | Kotlin Foundation | https://kotlinfoundation.org/ |
| F-3 | Dart 公式 | https://dart.dev/ |
| F-4 | OpenJDK プロジェクト | https://openjdk.org/ |
| F-5 | Oracle Java SE Support Roadmap | https://www.oracle.com/java/technologies/java-se-support-roadmap.html |
| F-6 | Oracle NFTC(No-Fee Terms and Conditions) | https://www.oracle.com/downloads/licenses/no-fee-license.html |
| F-7 | Eclipse Temurin(OpenJDK ディストリ) | https://adoptium.net/temurin/releases/ |
| F-8 | Azul Zulu / Platform Core | https://www.azul.com/downloads/ |
| F-9 | Amazon Corretto | https://aws.amazon.com/corretto/ |
| F-10 | TypeScript リリース(MS DevBlog) | https://devblogs.microsoft.com/typescript/ |
| F-11 | Swift Evolution | https://www.swift.org/swift-evolution/ |
| F-12 | .NET サポートポリシー | https://dotnet.microsoft.com/en-us/platform/support/policy/dotnet-core |
| F-13 | ECMAScript(TC39) | https://tc39.es/ |

### フレームワーク

| # | 対象 | URL |
|---|---|---|
| F-14 | Flutter リリースチャネル | https://docs.flutter.dev/release/archive |
| F-15 | React Native リリースワーキンググループ | https://github.com/reactwg/react-native-releases |
| F-16 | React バージョニング方針 | https://react.dev/ |
| F-17 | Angular リリーススケジュール | https://angular.dev/reference/releases |
| F-18 | Vue リリース/Vue 2 EOL 告知 | https://blog.vuejs.org/posts/vue-2-eol |
| F-19 | NativeScript 公式 | https://nativescript.org/ |
| F-20 | OpenJS Foundation(NativeScript ホスト) | https://openjsf.org/ |
| F-21 | Kotlin Multiplatform 公式 | https://kotlinlang.org/docs/multiplatform.html |
| F-22 | Compose Multiplatform 公式 | https://www.jetbrains.com/lp/compose-multiplatform/ |
| F-23 | Jetpack Compose 公式 | https://developer.android.com/jetpack/compose |
| F-24 | .NET MAUI サポートポリシー | https://dotnet.microsoft.com/en-us/platform/support/policy/maui |
| F-25 | Xamarin サポート終了 | https://dotnet.microsoft.com/en-us/platform/support/policy/xamarin |
| F-26 | Ionic Enterprise(商用サポート) | https://ionic.io/pricing |
| F-27 | Capacitor 公式 | https://capacitorjs.com/ |
| F-28 | Expo / EAS 価格表 | https://expo.dev/pricing |
| F-29 | Apache Cordova | https://cordova.apache.org/ |

### 配布・プラットフォーム要件

| # | 対象 | URL |
|---|---|---|
| F-30 | Google Play Target API Level 要件 | https://support.google.com/googleplay/android-developer/answer/11926878 |
| F-31 | Android Security Bulletins | https://source.android.com/docs/security/bulletin |
| F-32 | Unity ライセンス/料金(ランタイム料金撤回の経緯を含む) | https://unity.com/pricing |
| F-33 | Unreal Engine EULA / ロイヤリティ | https://www.unrealengine.com/en-US/eula/publishing |

### 利用上の注意

- 公式ソースのリリース日付・EOL 日付は各ページで **随時更新** される。特に Google Play Target API 要件は毎年引き上げられるため採用時点で再確認。
- ガバナンス・Bus Factor の評価は主観を含む。コミュニティ活動状況(GitHub issue/PR の応答性、コミット頻度、サードパーティ依存)と合わせて評価。
- 企業の商用サポート契約は料金・SLA 条件が変わるため、契約検討時は直接ベンダーに見積依頼。

---

## 更新履歴

- **2026-04-23**: 初版作成。観点 A〜F(うち F は保守・ガバナンス観点)を整理。
