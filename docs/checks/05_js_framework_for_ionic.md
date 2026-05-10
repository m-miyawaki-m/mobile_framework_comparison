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

## 3. PWA 適性

### P1. Service Worker 統合

| 点 | 状態 |
|---|---|
| 5 | 公式 SW モジュール / schematic、設定が宣言的 |
| 4 | 公式 plugin(Workbox ラッパ等)で標準対応 |
| 3 | Workbox 等を手動統合、サンプル豊富 |
| 2 | 手動統合のみ、サンプル限定的 |
| 1 | 統合に大きな摩擦 |

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| React | 3 | 1.0 | CRA 内蔵 SW は非推奨化、Vite-PWA / Workbox 手動寄り |
| Vue | 4 | 1.0 | `vite-plugin-pwa` がデファクト、Workbox 設定が宣言的 |
| Angular | **5** | 1.0 | `@angular/service-worker` 公式モジュール、`ngsw-config.json` で宣言的 |

### P2. Bundle size

| 点 | 状態 |
|---|---|
| 5 | 最小構成で < 30 KB(gzip)、初期ロード軽量 |
| 4 | 30〜50 KB、許容範囲 |
| 3 | 50〜100 KB、Code Splitting で改善要 |
| 2 | 100〜200 KB、最適化が課題 |
| 1 | 200 KB 超、PWA 用途で重荷 |

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| React | 4 | 1.0 | React + ReactDOM で約 40 KB(gzip)、ライブラリ追加で増加 |
| Vue | **5** | 1.0 | Vue 3 ランタイム < 30 KB(gzip)、最軽量 |
| Angular | 3 | 1.0 | Angular CLI 標準で 100+ KB、最適化で 60〜80 KB 程度 |

### P3. PWA ビルドツール

| 点 | 状態 |
|---|---|
| 5 | manifest.json / SW 自動生成の公式 schematic / plugin |
| 4 | 公式またはデファクト plugin が完備 |
| 3 | 部分的に自動化、手動補強要 |
| 2 | 主にコミュニティ製 |
| 1 | 自動化なし、手動構成 |

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| React | 4 | 1.0 | Vite-PWA は使えるが CRA は非推奨化、デファクトが流動的 |
| Vue | 5 | 1.0 | `vite-plugin-pwa` が標準、manifest/SW を自動生成 |
| Angular | 5 | 1.0 | `ng add @angular/pwa` schematic で manifest/SW 自動生成 |

### P4. オフライン構築容易度

| 点 | 状態 |
|---|---|
| 5 | IndexedDB/Workbox 統合が標準、永続化ライブラリ豊富 |
| 4 | 主要パターン整備、サンプル多 |
| 3 | ライブラリは揃うが統合は手動 |
| 2 | 限定的、独自実装が多い |
| 1 | オフライン構築に大きな摩擦 |

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| React | 5 | 1.0 | Workbox / Dexie.js / idb は FW 非依存、3 FW 同等 |
| Vue | 5 | 1.0 | 同上 |
| Angular | 5 | 1.0 | 同上、@angular/service-worker のキャッシュ戦略も追加で利用可 |

### P5. SSR/SSG 対応

| 点 | 状態 |
|---|---|
| 5 | Next.js / Nuxt 級の完全な SSR/SSG フレームワークが利用可 |
| 4 | 公式 SSR フレームワーク、設定はやや複雑 |
| 3 | SSR は可能だが Ionic アプリ用途では制約あり |
| 2 | SSG のみ、限定的 |
| 1 | SSR/SSG 公式対応なし |

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| React | **5** | 1.0 | Next.js が最強、ただし Ionic + Next.js は構成に注意 |
| Vue | **5** | 1.0 | Nuxt が同等、Ionic + Nuxt も実用例あり |
| Angular | 4 | 1.0 | Angular Universal は公式だが設定複雑、Ionic との統合は限定的 |

---

## 4. 学習・人材

### S1. 立ち上げ期間(5=短い)

| 点 | 状態 |
|---|---|
| 5 | 一般プログラマが 2〜3 週で実務 OK |
| 4 | 1〜2 ヶ月で実務 OK |
| 3 | 3〜4 ヶ月で実務 OK |
| 2 | 4〜6 ヶ月で実務 OK |
| 1 | 6 ヶ月超 |

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| React | 2 | 1.0 | Hooks/関数型/JSX/状態管理選択の負荷で 3〜5 ヶ月 |
| Vue | **5** | 1.0 | HTML テンプレ近似で 2〜3 週、最短 |
| Angular | 3 | 1.0 | DI/RxJS/Module/Reactive Forms で 3〜4 ヶ月 |

### S2. HTML/CSS 資産親和性

| 点 | 状態 |
|---|---|
| 5 | HTML テンプレ直書き、既存資産そのまま流用可 |
| 4 | テンプレベース、軽微な構文置換のみ |
| 3 | テンプレベースだがディレクティブ等で学習要 |
| 2 | JSX/TSX 等の中間形式、変換要 |
| 1 | HTML 資産流用が困難 |

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| React | 3 | 1.0 | JSX、class→className、style 等の置換要 |
| Vue | **5** | 1.0 | SFC の `<template>` は HTML 直書き |
| Angular | 4 | 1.0 | HTML テンプレ + Angular ディレクティブ、構造は HTML 寄り |

### S3. 国内人材プール

| 点 | 状態 |
|---|---|
| 5 | 求人・人材ともに最大層、採用容易 |
| 4 | 求人・人材ともに豊富 |
| 3 | 採用は可能だが選択肢限定 |
| 2 | 採用が課題、教育前提 |
| 1 | 極めて限定的 |

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| React | **5** | 1.0 | 国内最大層(selection_criteria.md B4: React ≫ Kotlin ≫ Vue) |
| Vue | 4 | 1.0 | 国内人気高、求人も豊富 |
| Angular | 3 | 1.0 | 業務系 SIer に根強い、ただし減少傾向 |

### S4. 日本語資料

| 点 | 状態 |
|---|---|
| 5 | 公式日本語、書籍多数、Qiita/Zenn 記事が新鮮 |
| 4 | 書籍・記事豊富、公式翻訳あり |
| 3 | 書籍はあるがやや古め、記事は中程度 |
| 2 | 書籍少、記事は海外翻訳寄り |
| 1 | 日本語資料がほぼない |

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| React | 5 | 1.0 | 公式日本語、書籍多数、Qiita/Zenn 最豊富 |
| Vue | 5 | 1.0 | 公式日本語、Evan You が日本コミュニティと近い歴史 |
| Angular | 3 | 1.0 | 書籍はあるが新鮮さに欠ける、Qiita/Zenn 中程度 |

### S5. AI 補完精度

| 点 | 状態 |
|---|---|
| 5 | 学習データ最大、TS 型情報で補完が極めて精度高 |
| 4 | 学習データ豊富、補完精度高 |
| 3 | 学習データは中規模、補完は実用レベル |
| 2 | 学習データ限定的、補完に誤りあり |
| 1 | 学習データ極小 |

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| React | 5 | 1.0 | npm 系で学習データ最大、JSX/TSX も豊富 |
| Vue | 4 | 1.0 | Vue 3 Composition API 学習データ十分、SFC は若干苦手 |
| Angular | 4 | 1.0 | TS 型情報多く補完精度高、ただし v17+ Signals は新しい |

---
