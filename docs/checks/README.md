# モバイルフレームワーク選定 評価チェックシート

本ディレクトリは、モバイルアプリ向けフレームワーク選定にあたり**プロジェクト固有事情に依存しない汎用評価軸**で各 FW を採点する再利用可能なチェックシート群である。

- 設計仕様: [`docs/superpowers/specs/2026-04-28-framework-checklist-design.md`](../superpowers/specs/2026-04-28-framework-checklist-design.md)
- 一次データ: [`docs/framework_comparison/framework_profiles.md`](../framework_comparison/framework_profiles.md)
- 情報基準日: 2026 年 4 月時点

---

## 評価対象(13 フレームワーク)

| # | FW | 主言語 | 備考 |
|---|---|---|---|
| 1 | Android ネイティブ(Jetpack Compose + Android SDK) | Kotlin | 業務アプリ第一推奨 |
| 2 | Flutter | Dart | |
| 3 | React Native | JavaScript / TypeScript | 素のRN |
| 4 | Expo(React Native + Expo SDK) | JavaScript / TypeScript | EAS Build含む |
| 5 | Ionic Framework | TypeScript | UI層 |
| 6 | Capacitor | TypeScript | ネイティブブリッジ層 |
| 7 | Kotlin Multiplatform (KMP) | Kotlin | ロジック共通化 |
| 8 | Compose Multiplatform | Kotlin | UI共通化 |
| 9 | .NET MAUI | C# | |
| 10 | NativeScript | TypeScript / JavaScript | |
| 11 | Apache Cordova | JavaScript | 縮小傾向 |
| 12 | Xamarin | C# | EOL(2024-05) |
| 13 | PWA | JavaScript / TypeScript | ブラウザ前提 |

---

## ファイル構成

| ファイル | 内容 |
|---|---|
| [`00_overview.md`](./00_overview.md) | サマリ + 端末ターゲット表 + 3種★点 |
| [`01_business_suitability.md`](./01_business_suitability.md) | 業務適正度 8 項目(B1〜B8) |
| [`02_learning_difficulty.md`](./02_learning_difficulty.md) | 習得難易度 8 項目(L1〜L8、5=易) |
| [`03_developer_experience.md`](./03_developer_experience.md) | 開発体験・開発環境 8 項目(D1〜D8) |
| [`04_language_features.md`](./04_language_features.md) | 言語仕様(事実情報、スコアなし) |
| [`05_js_framework_for_ionic.md`](./05_js_framework_for_ionic.md) | Ionic+Capacitor PWA 向け JS FW(React/Vue/Angular)5×5=25 項目 |

---

## スコアリング規約

### 1. スケール

- **1〜5 点の整数評価**(0.5 刻み不可)
- **5 = 望ましい状態**(習得難易度では 5 = 最も易しい)
- 各項目は冒頭に「点数 → 状態の言語化(代表例つき)」を 5 行で記述する **アンカー方式**

### 2. 仮想ペルソナ

評価のブレを抑える前提条件:

- **業務適正度・開発体験**: 中規模(10〜50画面)の Android 業務アプリを 2〜3 年スパンで保守する開発組織
- **習得難易度**: 他言語経験はあるが当該言語未経験の一般プログラマー

### 3. 集計

- **単純平均**: 各項目スコアの算術平均
- **重み付き平均**: Σ(スコア × 重み)÷ Σ(重み)
- **★換算**: 平均値を 5 段階おすすめ度に変換
  - 4.5 以上 = ★★★★★(5)
  - 3.5〜4.49 = ★★★★(4)
  - 2.5〜3.49 = ★★★(3)
  - 1.5〜2.49 = ★★(2)
  - 1.5 未満 = ★(1)

### 4. N/A の扱い

該当しない項目は **N/A** とし、平均計算の **分母から除外**(0 点扱いではない)。重み欄も `—`。

### 5. 重み列

各項目の重みはデフォルト `1.0`。利用者がプロジェクト優先度に応じて 0.0〜5.0 で調整する想定。本シート自体は重みを変更しない(原本として固定)。

---

## 利用ワークフロー

1. [`00_overview.md`](./00_overview.md) の端末対応 + 3種★ で候補を 3〜5 に絞り込む
2. [`04_language_features.md`](./04_language_features.md) で対応プラットフォーム(Web/サーバ/組込/ゲーム/CLI/データ)とスキル投資の汎用性を確認
3. [`01〜03`](./01_business_suitability.md) の詳細スコア表で根拠を確認(備考列に一次データ参照)
4. 自プロジェクト用にコピーして **重み列を編集**
5. 重み付き平均で順位を再計算 → 上位を最終候補とする

---

## プロジェクト固有事情との合成方法

本シートでは扱わない以下の **プロジェクト固有事情** は別軸チェックとして併用:

| 固有事情 | 合成方法 |
|---|---|
| チーム既存スキル(Java / JS / Kotlin 等) | L1〜L8 の重みを既存スキル親和度で調整 |
| 開発OS制約(Windows のみ等) | D8 の重みを上げる |
| 既存コード資産(Web / Android 等) | 流用可能な FW の B8 採用実績を優先 |
| 配布・運用要件(Store / MDM / 直配布) | B3 / B4 / B5 の重み調整 |
| 業界規制(金融 / 医療 / 公共) | B5(商用サポート)の重みを最大に |

---

## 更新ポリシー

- 各 FW の **採用時点で公式ソースで再確認**(LTS 日付、EOL 日付、ライセンス、サポート契約条件は変動)
- 一次データは [`framework_profiles.md`](../framework_comparison/framework_profiles.md) を更新し、本チェックシートは派生として再生成
- メジャーアップデート(LTS 切替、ライセンス変更、EOL 発表等)があった場合は当該項目を再評価

---

## 更新履歴

- **2026-04-28**: 初版作成
- **2026-05-11**: `05_js_framework_for_ionic.md`(Ionic 採用後の JS FW サブ評価)を追加
