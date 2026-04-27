# 業務適正度(B1〜B8)

業務アプリ用途で各 FW がどれだけ適しているかを 8 項目で評価する。
評価ルール詳細は [`README.md`](./README.md) 参照。

- スコア: 1〜5(5 = 望ましい)
- 重み: デフォルト 1.0(利用者が調整可)
- 仮想ペルソナ: 中規模 Android 業務アプリを 2〜3 年スパンで保守する開発組織

| 項目 | 内容 |
|---|---|
| B1 | リリース・サポートの予測可能性 |
| B2 | 開発企業・コミュニティの分散度 |
| B3 | ネイティブハードウェアアクセス |
| B4 | セキュリティ・脆弱性対応体制 |
| B5 | 商用サポート可否 |
| B6 | ライセンス安定性 |
| B7 | 後方互換性(破壊的変更頻度の低さ) |
| B8 | エンタープライズ採用実績 |

---

## B1: リリース・サポートの予測可能性

### アンカー

- **5**: 明示 LTS あり + 規則的リリース + 長期サポート(Java OpenJDK、Angular、.NET)
- **4**: 明示 LTS あり or 規則的リリース のどちらか(.NET MAUI、Kotlin)
- **3**: 後方互換維持で事実上安定だが明示 LTS なし(TypeScript、React)
- **2**: LTS なし・リリース不規則で追従負荷あり(Flutter、React Native、Expo)
- **1**: EOL or 縮小傾向(Xamarin、Cordova、PhoneGap)

### スコア表

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native | 4 | 1.0 | Android SDK は毎年更新、Jetpack 月次、Google Play Target API 引き上げ規則的だが明示 LTS なし |
| Flutter | 2 | 1.0 | 四半期 stable・月次 beta だが明示 LTS なし、追従負荷あり |
| React Native | 2 | 1.0 | 2〜3ヶ月で新マイナー、直近数バージョンのみサポート |
| Expo | 2 | 1.0 | 四半期 SDK、直近 3 SDK 程度サポート |
| Ionic Framework | 3 | 1.0 | 年 1 メジャー前後、後方互換重視だが明示 LTS なし |
| Capacitor | 3 | 1.0 | 半年〜年 1、Android/iOS SDK に追従、Ionic Enterprise で SLA 強化可 |
| KMP | 3 | 1.0 | Kotlin 同期、2023 Stable 到達、明示 LTS なしだが安定 |
| Compose Multiplatform | 2 | 1.0 | JetBrains 主導だが API 進化中、明示 LTS なし |
| .NET MAUI | 4 | 1.0 | .NET 同期(年 1)、LTS=3 年(STS=18ヶ月)、月次パッチ |
| NativeScript | 2 | 1.0 | リリース頻度低下、明示 LTS なし、モメンタム低 |
| Apache Cordova | 1 | 1.0 | プロジェクト縮小、新規非推奨 |
| Xamarin | 1 | 1.0 | 2024-05-01 EOL、.NET MAUI が後継 |
| PWA | 4 | 1.0 | Web プラットフォーム標準、ブラウザベンダーが LTS 相当 |

---

## B2: 開発企業・コミュニティの分散度

### アンカー

- **5**: 多ベンダー体制 or 巨大独立コミュニティ(Java OpenJDK、React Native)
- **4**: 主導企業 + 強力な複数企業/財団参加(.NET、TypeScript、React)
- **3**: 単一企業主導だが財団化・企業規模大(Kotlin、Angular)
- **2**: 単一企業の戦略依存(Flutter、Dart、Ionic)
- **1**: 個人 or 小チーム依存(Vue、NativeScript)

### スコア表

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native | 5 | 1.0 | Google + AOSP 巨大コミュニティ、複数企業実装(Samsung 等) |
| Flutter | 2 | 1.0 | Google 単独主導、戦略依存 |
| React Native | 5 | 1.0 | Meta + Microsoft + Callstack + Expo + コミュニティ |
| Expo | 3 | 1.0 | Expo 社主導だが RN コア + 複数企業の上に成立 |
| Ionic Framework | 2 | 1.0 | Ionic(営利企業)単独 |
| Capacitor | 2 | 1.0 | Ionic(営利企業)単独 |
| KMP | 3 | 1.0 | JetBrains 主導 + Kotlin Foundation + Google 後援 |
| Compose Multiplatform | 2 | 1.0 | JetBrains 単独主導 |
| .NET MAUI | 4 | 1.0 | Microsoft 主導 + .NET Foundation |
| NativeScript | 1 | 1.0 | OpenJS Foundation 配下だがモメンタム低 |
| Apache Cordova | 2 | 1.0 | Apache Foundation 配下だが活動縮小 |
| Xamarin | 1 | 1.0 | EOL、メンテナンスのみ |
| PWA | 5 | 1.0 | W3C / WHATWG / 複数ブラウザベンダーで標準化 |

---

## B3: ネイティブハードウェアアクセス

### アンカー

- **5**: 業務HT周辺機器ベンダーSDKが公式言語前提(Kotlin / Java + Android)
- **4**: 主要機能はFW公式 or 高品質ラッパで網羅(Flutter)
- **3**: コミュニティラッパ経由で実装可だが品質ばらつき(RN / Expo)
- **2**: 拡張プラグイン経由で限定的(Ionic / Capacitor)
- **1**: 原理的不可 or 重大制約(PWA)

### スコア表

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native | 5 | 1.0 | Zebra / Honeywell / Datalogic SDK が Java/Kotlin 前提、Intent / EMDK 全機能 |
| Flutter | 4 | 1.0 | 公式 plugin + Zebra など主要HT で公式サンプル、Platform Channel で逃げ道あり |
| React Native | 3 | 1.0 | 3rd party ラッパ経由、新アーキで品質改善傾向 |
| Expo | 3 | 1.0 | Expo SDK + Custom Native Module(Bare 化必要な場合あり) |
| Ionic Framework | 2 | 1.0 | Capacitor プラグイン経由、HW SDK は限定的 |
| Capacitor | 2 | 1.0 | プラグイン経由、業務HT は自前ラッパ必要 |
| KMP | 5 | 1.0 | Android 側は Kotlin 直接、HW SDK 完全アクセス |
| Compose Multiplatform | 4 | 1.0 | Compose UI + Android 側は Kotlin 経由で完全アクセス |
| .NET MAUI | 3 | 1.0 | Platform Specifics / Handler 経由、HT ベンダーは MAUI 対応途上 |
| NativeScript | 3 | 1.0 | JS 経由で Java API 直接呼出だがエコシステム狭い |
| Apache Cordova | 2 | 1.0 | プラグイン経由、HT 系は自前実装多 |
| Xamarin | 3 | 1.0 | C# Bindings 経由(EOL のため新規不可) |
| PWA | 1 | 1.0 | Web API 限定(BLE / NFC / Camera は実験的、HT SDK は原理的不可) |

---

## B4: セキュリティ・脆弱性対応体制

### アンカー

- **5**: 月次CVE窓口 + OS暗号API/Keystore直結 + 公式セキュリティチーム(Android / Jetpack、OpenJDK)
- **4**: 公式セキュリティ窓口あり、対応速度高(Kotlin、Flutter、Angular)
- **3**: GitHub Advisory ベース、対応速度中(React Native、TypeScript)
- **2**: コミュニティ依存で対応速度ばらつき(Vue、サードパーティ多用FW)
- **1**: 玉石混交の3rd party依存が中核(Capacitor 旧プラグイン、Cordova)

### スコア表

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native | 5 | 1.0 | Android Security Bulletin 月次 + AndroidX 公式、Keystore / Biometric 直結 |
| Flutter | 4 | 1.0 | security@flutter.dev + Google セキュリティチーム |
| React Native | 3 | 1.0 | GitHub Security Advisories + Meta 対応 |
| Expo | 3 | 1.0 | Expo + RN コア両方の対応に依存 |
| Ionic Framework | 3 | 1.0 | 公式 + 商用契約で SLA 強化可 |
| Capacitor | 2 | 1.0 | 公式コアは安定、プラグインは玉石混交 |
| KMP | 4 | 1.0 | JetBrains Security + Kotlin 公式 |
| Compose Multiplatform | 3 | 1.0 | JetBrains Security 同等だが広域に新しい |
| .NET MAUI | 4 | 1.0 | Microsoft Security Response Center、月次 Patch Tuesday |
| NativeScript | 2 | 1.0 | コミュニティ依存、対応速度ばらつき |
| Apache Cordova | 1 | 1.0 | プラグイン依存、メンテ放棄多 |
| Xamarin | 2 | 1.0 | Microsoft 対応継続だが EOL 後はベストエフォート |
| PWA | 4 | 1.0 | ブラウザベンダー(Chrome / Safari / Firefox)が定期パッチ |

---

## B5: 商用サポート可否

### アンカー

- **5**: 複数ベンダーが SLA 付き商用サポート提供(Java: Oracle / Red Hat / Azul / AWS)
- **4**: 単独ベンダーが商用サポート提供(.NET、Kotlin / IntelliJ、Ionic Enterprise)
- **3**: 公式サポートはないがコンサル企業経由で実質可(React Native: Callstack 等)
- **2**: 限定的・ベンダー間接(Flutter Partner 経由)
- **1**: 商用サポートなし(コミュニティのみ)

### スコア表

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native | 4 | 1.0 | JetBrains 商用契約(Kotlin)+ Google Cloud 関連、HT ベンダー保守契約 |
| Flutter | 2 | 1.0 | Google 直接商用サポートなし、Flutter Partner 経由 |
| React Native | 3 | 1.0 | Meta 直接契約なし、Callstack / Expo / Microsoft 経由 |
| Expo | 4 | 1.0 | EAS 商用プラン(Production / Enterprise)で SLA 付与 |
| Ionic Framework | 4 | 1.0 | Ionic Enterprise 公式契約 |
| Capacitor | 4 | 1.0 | Ionic Enterprise 公式契約 |
| KMP | 4 | 1.0 | JetBrains 商用契約 |
| Compose Multiplatform | 4 | 1.0 | JetBrains 商用契約 |
| .NET MAUI | 4 | 1.0 | Microsoft 単独商用サポート、Azure DevOps 統合(.NET LTS と同期) |
| NativeScript | 1 | 1.0 | 商用サポートほぼなし |
| Apache Cordova | 1 | 1.0 | 商用サポートなし |
| Xamarin | 2 | 1.0 | Microsoft 拡張サポート(EOL 後は限定) |
| PWA | 1 | 1.0 | Web 標準として直接の商用サポートはなし(Web ホスティング契約は別) |

---

## B6: ライセンス安定性

### アンカー

- **5**: Apache / MIT / BSD で枯れている(Kotlin、Flutter、TypeScript、.NET)
- **4**: GPL 系だが Classpath Exception 等で実質クリア(OpenJDK)
- **3**: ベンダーライセンス併存だが実用上問題なし(Oracle JDK with NFTC)
- **2**: 商用条件が複雑、規模で課金(Unreal Engine の売上ロイヤリティ)
- **1**: 過去に一方的な商用条件変更の前例あり(Unity ランタイム料金騒動)

### スコア表

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native | 5 | 1.0 | Apache 2.0(AOSP)、Jetpack Compose も Apache 2.0 |
| Flutter | 5 | 1.0 | BSD-3-Clause |
| React Native | 5 | 1.0 | MIT(旧 Meta 特許条項撤廃済) |
| Expo | 5 | 1.0 | MIT(EAS Build / Update は有料プラン) |
| Ionic Framework | 5 | 1.0 | MIT(Enterprise 契約は有償) |
| Capacitor | 5 | 1.0 | MIT |
| KMP | 5 | 1.0 | Apache 2.0 |
| Compose Multiplatform | 5 | 1.0 | Apache 2.0 |
| .NET MAUI | 5 | 1.0 | MIT |
| NativeScript | 5 | 1.0 | Apache 2.0 |
| Apache Cordova | 5 | 1.0 | Apache 2.0 |
| Xamarin | 5 | 1.0 | MIT |
| PWA | 5 | 1.0 | Web 標準仕様自体は無料、ブラウザベンダーごとライセンス |

---

## B7: 後方互換性(破壊的変更頻度の低さ)

### アンカー

- **5**: 10 年単位の互換維持文化(Java「Write Once, Run Forever」)
- **4**: メジャーアップデートでも軽微な修正で済む(React、TypeScript)
- **3**: 規則的だがマイグレーション必要(Angular、.NET)
- **2**: メジャー毎に大規模 Migration Guide が出る(Flutter、Kotlin 2.0)
- **1**: メジャー間で事実上再書き(Vue 2→3、React Native 新アーキ移行)

### スコア表

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native | 3 | 1.0 | Target API 強制引き上げ、Compose API 進化、ng update 相当の自動補助あり |
| Flutter | 2 | 1.0 | API が頻繁に改定、大規模 Migration Guide |
| React Native | 1 | 1.0 | 新アーキ(JSI / Fabric)移行、ライブラリ互換性問題深刻 |
| Expo | 2 | 1.0 | SDK バージョンごとにライブラリ入替 |
| Ionic Framework | 4 | 1.0 | 後方互換重視、メジャー間でも軽微 |
| Capacitor | 3 | 1.0 | Android / iOS SDK 追従でメジャー時に変更あり |
| KMP | 3 | 1.0 | Stable 後は安定、ただし K2 切替の影響あり |
| Compose Multiplatform | 2 | 1.0 | API 進化中、Stable 化前後で変更頻度高 |
| .NET MAUI | 3 | 1.0 | .NET メジャー毎に整理、LTS 切替で対応必要 |
| NativeScript | 3 | 1.0 | 変更頻度低いがエコシステム追従不足 |
| Apache Cordova | 3 | 1.0 | 縮小傾向のためリリース頻度低下、既存プロジェクトはほぼ変更なし、ただし新規 Plugin 互換は未保証 |
| Xamarin | 1 | 1.0 | EOL、MAUI への移行は実質再書き |
| PWA | 4 | 1.0 | Web 標準は後方互換重視、Service Worker 等の API 仕様も安定 |

---

## B8: エンタープライズ採用実績

### アンカー

- **5**: 業務基幹で広範採用、Fortune 500 多数(Java、Android Native)
- **4**: エンタープライズ採用が広く認知(.NET、Kotlin、TypeScript)
- **3**: 業務利用は伸びているが業界別偏り(Flutter、React Native)
- **2**: 採用例はあるがニッチ(Ionic、KMP)
- **1**: 業務利用ほぼ未検証 or 縮小(NativeScript、Cordova 新規)

### スコア表

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| Android Native | 5 | 1.0 | 物流 / 小売 / 金融など業務基幹で世界規模採用 |
| Flutter | 3 | 1.0 | Google Pay / Alibaba / BMW など、業務系は伸びているが偏りあり |
| React Native | 3 | 1.0 | Facebook / Instagram / Discord、業務系も増えているが業界偏り |
| Expo | 3 | 1.0 | 中規模アプリで採用増、エンタープライズ用途も拡大中 |
| Ionic Framework | 3 | 1.0 | 業務アプリ採用多数(社内ツール特化) |
| Capacitor | 3 | 1.0 | Ionic と同様に社内業務アプリで広く採用 |
| KMP | 2 | 1.0 | 採用例増加中だがまだニッチ |
| Compose Multiplatform | 1 | 1.0 | Stable 直後で業務採用例少 |
| .NET MAUI | 4 | 1.0 | Microsoft エコシステム企業で広く採用、Xamarin からの移行先 |
| NativeScript | 1 | 1.0 | エンタープライズ採用ほぼ未検証 |
| Apache Cordova | 2 | 1.0 | 過去に多数採用されたがレガシ移行中 |
| Xamarin | 2 | 1.0 | EOL のため新規採用なし、既存資産のみ |
| PWA | 3 | 1.0 | Twitter / Starbucks / Pinterest など、業務系は社内ツール中心 |

---

## 集計

各 FW の B1〜B8 を平均し、★換算したもの。重み付き平均は重み = 1.0(均等)前提のため単純平均と同値。利用者は重みを変更して再計算する。

| FW | B1 | B2 | B3 | B4 | B5 | B6 | B7 | B8 | 単純平均 | 重み付き平均 | ★換算 |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Android Native | 4 | 5 | 5 | 5 | 4 | 5 | 3 | 5 | 4.50 | 4.50 | ★★★★★ |
| Flutter | 2 | 2 | 4 | 4 | 2 | 5 | 2 | 3 | 3.00 | 3.00 | ★★★ |
| React Native | 2 | 5 | 3 | 3 | 3 | 5 | 1 | 3 | 3.13 | 3.13 | ★★★ |
| Expo | 2 | 3 | 3 | 3 | 4 | 5 | 2 | 3 | 3.13 | 3.13 | ★★★ |
| Ionic Framework | 3 | 2 | 2 | 3 | 4 | 5 | 4 | 3 | 3.25 | 3.25 | ★★★ |
| Capacitor | 3 | 2 | 2 | 2 | 4 | 5 | 3 | 3 | 3.00 | 3.00 | ★★★ |
| KMP | 3 | 3 | 5 | 4 | 4 | 5 | 3 | 2 | 3.63 | 3.63 | ★★★★ |
| Compose Multiplatform | 2 | 2 | 4 | 3 | 4 | 5 | 2 | 1 | 2.88 | 2.88 | ★★★ |
| .NET MAUI | 4 | 4 | 3 | 4 | 4 | 5 | 3 | 4 | 3.88 | 3.88 | ★★★★ |
| NativeScript | 2 | 1 | 3 | 2 | 1 | 5 | 3 | 1 | 2.25 | 2.25 | ★★ |
| Apache Cordova | 1 | 2 | 2 | 1 | 1 | 5 | 3 | 2 | 2.13 | 2.13 | ★★ |
| Xamarin | 1 | 1 | 3 | 2 | 2 | 5 | 1 | 2 | 2.13 | 2.13 | ★★ |
| PWA | 4 | 5 | 1 | 4 | 1 | 5 | 4 | 3 | 3.38 | 3.38 | ★★★ |

> 注: 各セルのスコアは上記アンカーに基づく初期値(2026-04-28 時点)。利用者は自プロジェクトの優先度に応じて重み列を編集し、上記表の単純平均/重み付き平均を再計算する。

---

## 更新履歴

- **2026-04-28**: 初版作成。B1〜B8 のアンカー定義と 13 FW の初期スコアを記載。
