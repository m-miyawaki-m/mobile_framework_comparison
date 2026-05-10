# Ionic+Capacitor PWA 向け JS フレームワーク 評価スコアシート

「PWA を Ionic + Capacitor で構築する」前提が確定した後の **JS フレームワーク層(React / Vue / Angular)選定** に使う汎用スコアシート。docs/checks/ 本体は 13 FW のマクロ比較を扱い、本シートはその下位(Ionic 採用後の JS FW 選び)を専門に扱うサブ評価。

- 設計仕様: [`docs/superpowers/specs/2026-05-11-js-framework-for-ionic-scorecard-design.md`](../superpowers/specs/2026-05-11-js-framework-for-ionic-scorecard-design.md)
- 一次データ: [`docs/framework_comparison/profile_ionic_capacitor.md`](../framework_comparison/profile_ionic_capacitor.md) / [`docs/framework_comparison/selection_criteria.md`](../framework_comparison/selection_criteria.md)
- 情報基準日: 2026 年 5 月時点

---

## 0. 評価対象とスコアリング規約

### 0.1 評価対象(3 JS フレームワーク)

| # | JS FW | バージョン | Ionic バインディング |
|---|---|---|---|
| 1 | React | 18+ / 19 | `@ionic/react` |
| 2 | Vue | 3.x | `@ionic/vue` |
| 3 | Angular | 17+ / 18+ | `@ionic/angular` |

### 0.2 評価軸(5 カテゴリ × 5 項目 = 25 項目)

| カテゴリ | コード | 中身 |
|---|---|---|
| Ionic/Capacitor 統合度 | I1〜I5 | Ionic 公式バインディング・Live Reload・Router・コンポーネント・Capacitor Plugin |
| PWA 適性 | P1〜P5 | Service Worker・Bundle size・PWA ビルドツール・オフライン・SSR/SSG |
| 学習・人材 | S1〜S5 | 立ち上げ期間・HTML 親和性・国内人材・日本語資料・AI 補完 |
| 開発体験(JS FW 固有)| D1〜D5 | リアクティビティ・状態管理・Form・型安全・Lint |
| 長期保守・ガバナンス | G1〜G5 | LTS・破壊的変更・Bus Factor・商用サポート・エコ健全性 |

### 0.3 スコアリング規約

`docs/checks/README.md` の規約を継承。

- **1〜5 整数**(0.5 刻み不可)、**5 = 望ましい**
- **★ 換算**: 4.5 以上=★★★★★/3.5〜4.49=★★★★/2.5〜3.49=★★★/1.5〜2.49=★★/1.5 未満=★
- **集計**: 単純平均 / 重み付き平均
- **重み**: デフォルト 1.0、利用者がプロジェクト優先度で 0.0〜5.0 に調整
- **N/A**: 平均計算の分母から除外、重み欄も `—`

---

## 1. サマリ(★後で埋める)

(Task 7 で確定値に置換)

---

## 2. Ionic/Capacitor 統合度

### I1. 公式バインディング成熟度

| 点 | 状態 |
|---|---|
| 5 | Ionic 1 系から継続的維持、公式チュートリアル・ドキュメント最厚 |
| 4 | Ionic 4 以降の対応で十分成熟、公式サポートあり |
| 3 | バインディングは存在、機能差や API ラグあり |
| 2 | コミュニティ実装中心、Ionic 公式の支援が薄い |
| 1 | 非公式 / 実用困難 / 開発中断 |

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| React | 4 | 1.0 | `@ionic/react` は Ionic 4(2019)から対応、Hooks API 対応 |
| Vue | 4 | 1.0 | `@ionic/vue` は Ionic 4(2019)から対応、Vue 3 専用 |
| Angular | **5** | 1.0 | Ionic 1(2013)からの本家、最古参で公式チュートリアルが最厚 |

### I2. Live Reload 品質

| 点 | 状態 |
|---|---|
| 5 | Vite HMR で瞬時(< 数百ms)、実機 Live Reload も安定 |
| 4 | HMR は高速だが初回起動に数秒、実機 Live Reload は概ね安定 |
| 3 | HMR は機能するが速度・状態保持に難 |
| 2 | リロードが手動寄り、状態保持なし |
| 1 | Hot Reload なし、毎回ビルド要 |

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| React | **5** | 1.0 | Vite + React Fast Refresh、HMR 瞬時 |
| Vue | **5** | 1.0 | Vite + Vue HMR、HMR 瞬時 |
| Angular | 4 | 1.0 | Angular CLI(Webpack/esbuild)はやや重い、ただし安定 |

### I3. Router/Navigation 統合

| 点 | 状態 |
|---|---|
| 5 | Stack-based Navigation・Tabs・Modal・Sidemenu のすべてが破綻なく統合 |
| 4 | 主要パターンは統合済、エッジケースに若干の癖 |
| 3 | 基本機能は動くが、ライフサイクル・履歴管理で罠あり |
| 2 | 部分実装、自前で補強が必要 |
| 1 | 公式統合なし、コミュニティ製のみ |

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| React | 4 | 1.0 | `IonReactRouter`(React Router ベース)、Stack Navigation に若干の癖 |
| Vue | 4 | 1.0 | `IonVueRouter`(Vue Router ベース)、概ね素直 |
| Angular | **5** | 1.0 | Angular Router + Ionic Routing が最も成熟、業務向け |

### I4. コンポーネント網羅性

| 点 | 状態 |
|---|---|
| 5 | ion-* 全コンポーネントが公式ラッパで利用可、API ラグなし |
| 4 | 主要コンポーネントは網羅、新規追加が他 FW より遅れる |
| 3 | 一部コンポーネントが未対応、回避策あり |
| 2 | 半数程度のみ対応 |
| 1 | コア機能のみ |

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| React | 5 | 1.0 | Web Components 実装のため API は 3 FW 共通、ラッパは完全網羅 |
| Vue | 5 | 1.0 | 同上 |
| Angular | 5 | 1.0 | 同上 |

### I5. Capacitor Plugin 相性

| 点 | 状態 |
|---|---|
| 5 | 公式ドキュメントの例が FW 別に整備、サンプル豊富 |
| 4 | 主要プラグインの例あり、コミュニティサンプル多 |
| 3 | プラグインは動くがサンプルが他 FW より薄い |
| 2 | サンプル少なく自前で導入要 |
| 1 | 統合に大きな摩擦 |

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| React | 5 | 1.0 | Capacitor Plugin は JS API のため FW 非依存、3 FW 同等 |
| Vue | 5 | 1.0 | 同上 |
| Angular | 5 | 1.0 | 同上 |

---
