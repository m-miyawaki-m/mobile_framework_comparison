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
