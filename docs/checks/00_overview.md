# 概要(モバイル FW 評価チェックシート 一覧)

13 FW の **端末対応** と **3 種★点** を 1 表にまとめたサマリビュー。
評価ルール詳細は [`README.md`](./README.md)、各スコアの根拠は [`01_business_suitability.md`](./01_business_suitability.md) / [`02_learning_difficulty.md`](./02_learning_difficulty.md) / [`03_developer_experience.md`](./03_developer_experience.md) 参照。

凡例:
- ★★★★★ = 最良
- ★ = 最低
- 端末: ◎ = 全機能利用可 / ○ = 限定利用(拡張ライブラリ経由)/ × = 不可

---

## 端末対応マトリクス

| # | FW | 主言語 | Android | iOS | Windows | macOS | Linux | 組込機器 |
|---|---|---|---|---|---|---|---|---|
| 1 | Android Native | Kotlin | ◎ | × | × | × | × | ◎ |
| 2 | Flutter | Dart | ◎ | ◎ | ○ | ○ | ○ | ○ |
| 3 | React Native | JS / TS | ◎ | ◎ | ○ | ○ | × | × |
| 4 | Expo | JS / TS | ◎ | ◎ | × | × | × | × |
| 5 | Ionic Framework | TS | ◎ | ◎ | ○ | ○ | ○ | × |
| 6 | Capacitor | TS | ◎ | ◎ | ○ | ○ | ○ | × |
| 7 | KMP | Kotlin | ◎ | ◎ | ○ | ○ | ○ | △ |
| 8 | Compose Multiplatform | Kotlin | ◎ | ◎ | ◎ | ◎ | ◎ | × |
| 9 | .NET MAUI | C# | ◎ | ◎ | ◎ | ◎ | × | × |
| 10 | NativeScript | TS / JS | ◎ | ◎ | × | × | × | × |
| 11 | Apache Cordova | JS | ◎ | ◎ | × | × | × | × |
| 12 | Xamarin | C# | ◎ | ◎ | × | × | × | × |
| 13 | PWA | JS / TS | △ | △ | △ | △ | △ | × |

> 注: 端末欄の凡例
> - **◎**: 公式・第一級サポートで動作(プラットフォーム公式言語含む)
> - **○**: 拡張ライブラリ経由で動作(下表「限定利用ライブラリ等」参照)
> - **△**: ブラウザ経由で動作(PWA は OS の PWA 対応に依存、機能制限あり)
> - **×**: 動作不可
> - **Android Native の組込機器**: 業務HT(ハンディターミナル)等の Android ベース機器を含む

---

## 限定利用ライブラリ等

| FW | 限定利用ライブラリ等 |
|---|---|
| Android Native | — |
| Flutter | Flutter Desktop / Flutter Embedded / Flutter Web |
| React Native | RN for Windows / RN for macOS |
| Expo | (現状なし、Expo for Web は Web 出力のみ) |
| Ionic Framework | Capacitor Electron(デスクトップ)、Capacitor 単独で iOS / Android |
| Capacitor | Capacitor Electron / Tauri 統合 |
| KMP | Compose Multiplatform / Kotlin/JS / Kotlin Native |
| Compose Multiplatform | (本体で全プラットフォーム対応) |
| .NET MAUI | Mac Catalyst、Linux はサードパーティ実装 |
| NativeScript | — |
| Apache Cordova | — |
| Xamarin | — |
| PWA | OS の PWA 対応(Chromium 系強、Safari は機能制限) |

---

## サマリ(3 種★点)

| # | FW | 主言語 | 業務適正度 | 開発体験 | 習得難易度 |
|---|---|---|---|---|---|
| 1 | Android Native | Kotlin | ★★★★★ | ★★★★ | ★★★★★ |
| 2 | Flutter | Dart | ★★★ | ★★★★ | ★★★★ |
| 3 | React Native | JS / TS | ★★★ | ★★★★ | ★★★ |
| 4 | Expo | JS / TS | ★★★ | ★★★★ | ★★★ |
| 5 | Ionic Framework | TS | ★★★ | ★★★★ | ★★★ |
| 6 | Capacitor | TS | ★★★ | ★★★★ | ★★★★ |
| 7 | KMP | Kotlin | ★★★★ | ★★★ | ★★★★ |
| 8 | Compose Multiplatform | Kotlin | ★★★ | ★★★ | ★★★ |
| 9 | .NET MAUI | C# | ★★★★ | ★★★★ | ★★★★ |
| 10 | NativeScript | TS / JS | ★★ | ★★★ | ★★★ |
| 11 | Apache Cordova | JS | ★★ | ★★★ | ★★★ |
| 12 | Xamarin | C# | ★★ | ★★★ | ★★★★ |
| 13 | PWA | JS / TS | ★★★ | ★★★★★ | ★★★★ |

> 各★は対応シートの単純平均から導出した初期値。利用者は重みを編集して再計算する。

---

## 候補絞り込みのヒント

- **業務適正度 ★★★★以上**: Android Native / KMP / .NET MAUI(エンタープライズ向け第一候補)
- **開発体験 ★★★★以上**: Android Native / Flutter / React Native / Expo / Ionic / Capacitor / .NET MAUI / PWA(日々の生産性が高い)
- **習得難易度 ★★★★以上(=易しい)**: Android Native / Flutter / Capacitor / KMP / .NET MAUI / Xamarin / PWA
- **3 種すべて ★★★★以上**: **Android Native / .NET MAUI**(汎用的な業務アプリで両立しやすい)
- **3 種すべて ★★★以上**: 上記 + Flutter / KMP / Expo / Ionic / Capacitor / PWA / React Native / Compose Multiplatform(計 10 候補、NativeScript / Apache Cordova / Xamarin のみ除外)

---

## 詳細シートへの導線

- 業務適正度の根拠 → [`01_business_suitability.md`](./01_business_suitability.md)
- 習得難易度の根拠 → [`02_learning_difficulty.md`](./02_learning_difficulty.md)
- 開発体験の根拠 → [`03_developer_experience.md`](./03_developer_experience.md)
- 言語特性(対応領域・採用例・PWA 対応等)→ [`04_language_features.md`](./04_language_features.md)

---

## 更新履歴

- **2026-04-28**: 初版作成。13 FW の端末対応・3 種★を集約。
