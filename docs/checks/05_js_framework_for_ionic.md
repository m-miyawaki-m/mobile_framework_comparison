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

(以下、Section 2〜6 をカテゴリごとに追記。Section 7〜8 は最後に。)
