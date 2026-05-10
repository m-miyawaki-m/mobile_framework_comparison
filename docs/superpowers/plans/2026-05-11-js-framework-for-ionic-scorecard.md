# JS Framework Scorecard for Ionic+Capacitor PWA - Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** `docs/checks/05_js_framework_for_ionic.md` を新規作成し、React/Vue/Angular を 5カテゴリ×5項目=25項目 で評価する汎用スコアシートを完成させる。さらに `docs/checks/README.md` に新シートへの参照を追加する。

**Architecture:** 単一マークダウンファイルに 0〜8 章で構成。docs/checks/ の既存スコアリング規約(1〜5整数、★換算、重み列、N/A)を継承。カテゴリごとにアンカー(1〜5 各点の言語化)+ 3 FW スコア表 + 備考を記載。最後にサマリ(カテゴリ別平均と総合★)、候補絞り込みヒント、参照リンクを置く。

**Tech Stack:** Markdown(GitHub-flavored)、git。

**Spec:** [`docs/superpowers/specs/2026-05-11-js-framework-for-ionic-scorecard-design.md`](../specs/2026-05-11-js-framework-for-ionic-scorecard-design.md)

---

## File Structure

- **新規作成**: `docs/checks/05_js_framework_for_ionic.md`(単一ファイル、約400〜500行想定)
- **修正**: `docs/checks/README.md`(ファイル構成セクションに 1 行追加)

サブセクションごとに 1 タスク = 1 コミット。Section 1 のサマリは各カテゴリ確定後に埋める(Task 7)。

---

### Task 1: Skeleton + Section 0 + Section 1 placeholder

**Files:**
- Create: `docs/checks/05_js_framework_for_ionic.md`

- [ ] **Step 1: ファイルを新規作成し、ヘッダ・Section 0(評価対象とスコアリング規約)・Section 1(サマリ placeholder)を書き込む**

`docs/checks/05_js_framework_for_ionic.md` に以下の内容で新規作成:

```markdown
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
```

- [ ] **Step 2: ファイル末尾までの構成プレースホルダコメントが正しく書かれていることを確認**

確認: `Read docs/checks/05_js_framework_for_ionic.md` で先頭〜末尾まで読み、Section 0 が完全に記載され、Section 1 と「(以下、Section 2〜6 を…)」のプレースホルダが存在することを目視確認。

- [ ] **Step 3: Commit**

```bash
git add docs/checks/05_js_framework_for_ionic.md
git commit -m "docs(checks): scaffold JS framework scorecard for Ionic+Capacitor

Section 0(評価対象とスコアリング規約)と章構造のプレースホルダを設置。

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

### Task 2: Section 2 - Ionic/Capacitor 統合度 (I1〜I5)

**Files:**
- Modify: `docs/checks/05_js_framework_for_ionic.md`(プレースホルダ「(以下、Section 2〜6 を…)」を Section 2 の内容に置換)

- [ ] **Step 1: プレースホルダを削除し、Section 2 を追記**

ファイル末尾の `(以下、Section 2〜6 をカテゴリごとに追記。Section 7〜8 は最後に。)` の行を削除し、以下を末尾に追加:

```markdown
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
```

- [ ] **Step 2: Section 2 が正しく書き込まれていることを確認**

確認: `Read docs/checks/05_js_framework_for_ionic.md` で Section 2 の I1〜I5 各項目が(アンカー表 + スコア表)の2表構成で記載されていることを目視確認。

- [ ] **Step 3: Commit**

```bash
git add docs/checks/05_js_framework_for_ionic.md
git commit -m "docs(checks): add I1-I5 Ionic/Capacitor integration scoring

React/Vue/Angular の Ionic 統合度 5 項目をアンカー言語化付きで採点。

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

### Task 3: Section 3 - PWA 適性 (P1〜P5)

**Files:**
- Modify: `docs/checks/05_js_framework_for_ionic.md`(末尾に追記)

- [ ] **Step 1: Section 3 を末尾に追記**

ファイル末尾(Section 2 終端の `---` の後)に以下を追加:

```markdown
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
```

- [ ] **Step 2: Section 3 が正しく書き込まれていることを確認**

確認: `Read docs/checks/05_js_framework_for_ionic.md`(該当範囲)で P1〜P5 が記載されていることを目視確認。

- [ ] **Step 3: Commit**

```bash
git add docs/checks/05_js_framework_for_ionic.md
git commit -m "docs(checks): add P1-P5 PWA suitability scoring

Service Worker 統合・Bundle size・PWA ビルドツール・オフライン・SSR/SSG を採点。

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

### Task 4: Section 4 - 学習・人材 (S1〜S5)

**Files:**
- Modify: `docs/checks/05_js_framework_for_ionic.md`(末尾に追記)

- [ ] **Step 1: Section 4 を末尾に追記**

```markdown
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
```

- [ ] **Step 2: Section 4 が正しく書き込まれていることを確認**

確認: `Read` で S1〜S5 が記載されていることを目視確認。

- [ ] **Step 3: Commit**

```bash
git add docs/checks/05_js_framework_for_ionic.md
git commit -m "docs(checks): add S1-S5 learning/talent scoring

立ち上げ期間・HTML親和性・国内人材・日本語資料・AI補完を採点。

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

### Task 5: Section 5 - 開発体験 (D1〜D5)

**Files:**
- Modify: `docs/checks/05_js_framework_for_ionic.md`(末尾に追記)

- [ ] **Step 1: Section 5 を末尾に追記**

```markdown
## 5. 開発体験(JS FW 固有)

### D1. リアクティビティ表現力

| 点 | 状態 |
|---|---|
| 5 | 宣言的かつ依存追跡自動、記述量最小 |
| 4 | 直感的、ボイラープレート少 |
| 3 | 動作するが依存配列等の手動管理が必要 |
| 2 | 冗長、再描画制御が複雑 |
| 1 | リアクティビティ表現が貧弱 |

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| React | 3 | 1.0 | `useState/useEffect`、依存配列の手動管理が負担 |
| Vue | **5** | 1.0 | `ref/reactive` で依存追跡自動、記述量最小 |
| Angular | 4 | 1.0 | v17+ で Signals 導入、従来は RxJS で柔軟 |

### D2. 状態管理

| 点 | 状態 |
|---|---|
| 5 | 公式デファクトが確立、軽量で TS との相性良 |
| 4 | 公式推奨またはデファクトあり、選択に迷わない |
| 3 | 選択肢多すぎて意思決定が課題 |
| 2 | デファクトが流動的 |
| 1 | 状態管理ライブラリが乏しい |

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| React | 4 | 1.0 | Redux/Zustand/Jotai/Recoil 等、選択肢多すぎ問題 |
| Vue | **5** | 1.0 | Pinia が公式デファクト、軽量・TS 親和性高 |
| Angular | 4 | 1.0 | NgRx 重厚、Component Store/Signals Store の選択肢あり |

### D3. Form/Validation

| 点 | 状態 |
|---|---|
| 5 | 公式 Form モジュール、検証 API が一級市民 |
| 4 | デファクト Form ライブラリが確立 |
| 3 | 複数の Form ライブラリ、選択要 |
| 2 | Form 構築が個別実装寄り |
| 1 | Form/Validation サポートが限定的 |

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| React | 4 | 1.0 | React Hook Form + Zod がデファクト |
| Vue | 4 | 1.0 | vee-validate / FormKit がデファクト |
| Angular | **5** | 1.0 | Reactive Forms 公式、複雑業務フォーム最強 |

### D4. 型安全(TS 統合度)

| 点 | 状態 |
|---|---|
| 5 | TS 必須または推奨、テンプレ内も型推論される |
| 4 | TS との親和性高、strict モード安定 |
| 3 | TS は使えるがテンプレ内で型推論が弱い |
| 2 | TS 対応はあるが部分的 |
| 1 | TS 対応が限定的 |

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| React | 4 | 1.0 | JSX 型推論は強いが、Props 型と children 型で苦戦することあり |
| Vue | 4 | 1.0 | Volar で SFC 内型推論強化、Vue 3.3+ で改善 |
| Angular | **5** | 1.0 | TS 必須、テンプレート内も Angular Language Service で型推論 |

### D5. Lint/Formatter

| 点 | 状態 |
|---|---|
| 5 | 公式 ESLint plugin、スタイルガイド完備 |
| 4 | デファクト plugin あり、運用が安定 |
| 3 | plugin 複数、選択要 |
| 2 | 限定的 |
| 1 | Lint ルールが乏しい |

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| React | 5 | 1.0 | `eslint-plugin-react` / `eslint-plugin-react-hooks` 標準 |
| Vue | 5 | 1.0 | `eslint-plugin-vue` 公式 |
| Angular | 5 | 1.0 | `@angular-eslint` 公式 |

---
```

- [ ] **Step 2: Section 5 が正しく書き込まれていることを確認**

確認: `Read` で D1〜D5 が記載されていることを目視確認。

- [ ] **Step 3: Commit**

```bash
git add docs/checks/05_js_framework_for_ionic.md
git commit -m "docs(checks): add D1-D5 developer experience scoring

リアクティビティ・状態管理・Form・型安全・Lintを採点。

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

### Task 6: Section 6 - 長期保守・ガバナンス (G1〜G5)

**Files:**
- Modify: `docs/checks/05_js_framework_for_ionic.md`(末尾に追記)

- [ ] **Step 1: Section 6 を末尾に追記**

```markdown
## 6. 長期保守・ガバナンス

### G1. LTS

| 点 | 状態 |
|---|---|
| 5 | 明示 LTS あり、サポート期間が長期(12 ヶ月以上)で公開 |
| 4 | 事実上の後方互換維持、メジャー長寿 |
| 3 | LTS の明示はないが直近メジャーは継続サポート |
| 2 | サポート期間が短い、過去 EOL 前例あり |
| 1 | LTS なし + 過去 EOL 前例で再発リスク |

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| React | 4 | 1.0 | 明示 LTS なしだが事実上後方互換維持(selection_criteria.md F5) |
| Vue | 2 | 1.0 | Vue 2 = 2023-12 EOL 前例、Vue 3 のみ現行、明示 LTS なし |
| Angular | **5** | 1.0 | 6m Active + 12m LTS = 18m を明示(F5)|

### G2. 破壊的変更頻度(5=少ない)

| 点 | 状態 |
|---|---|
| 5 | 後方互換最重視、メジャー間でもコード変更ほぼ不要 |
| 4 | メジャー間で軽微な変更、自動マイグレーション完備 |
| 3 | メジャー間で中程度の変更、`ng update` 等の補助あり |
| 2 | メジャー間で大幅変更、手動移行多 |
| 1 | 過去にメジャー再書き前例あり(Vue 2→3 級) |

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| React | **5** | 1.0 | フレームワーク中最も後方互換重視(F6) |
| Vue | 2 | 1.0 | Vue 2→3 は事実上再書、再発リスク懸念(F6) |
| Angular | 3 | 1.0 | v ごとに API 変更、`ng update` で補助(F6) |

### G3. Bus Factor

| 点 | 状態 |
|---|---|
| 5 | 複数大企業 + 巨大コミュニティ、消失リスク極小 |
| 4 | 大企業主導 + 厚いコミュニティ |
| 3 | 大企業主導、コミュニティ普通 |
| 2 | 小規模チーム / 個人依存 |
| 1 | 単一個人 / 開発停滞気味 |

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| React | **5** | 1.0 | Meta + Vercel + 巨大エコ、利用規模極大(F3) |
| Vue | 3 | 1.0 | Evan You + 小規模チーム、スポンサー財源依存(F3) |
| Angular | 4 | 1.0 | Google 単一企業主導(F3) |

### G4. 商用サポート

| 点 | 状態 |
|---|---|
| 5 | ベンダー直契約 + SLA 明確 |
| 4 | パートナー経由で実質的な商用サポートあり |
| 3 | コンサル契約が一般的、SLA は契約次第 |
| 2 | コミュニティ SLA = 0、商用契約困難 |
| 1 | 商用サポート選択肢なし |

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| React | 4 | 1.0 | Meta 直契約なし、Callstack 等コンサル経由(F8) |
| Vue | 3 | 1.0 | Vue 公式商用契約はない、コンサル個別契約(F8) |
| Angular | 4 | 1.0 | Google 主導、Angular 公式の有償 SLA はなくパートナー経由(F8) |

### G5. エコシステム健全性

| 点 | 状態 |
|---|---|
| 5 | デファクトライブラリの保守活発、依存更新が継続 |
| 4 | 主要ライブラリは健全、一部に保守遅れ |
| 3 | 中程度、保守遅れと健全な ライブラリの混在 |
| 2 | デファクトが流動的、保守鈍化 |
| 1 | エコシステム健全性が懸念レベル |

| FW | スコア | 重み | 備考 |
|---|---|---|---|
| React | **5** | 1.0 | npm 最大規模、ただし fatigue 問題はあり |
| Vue | 4 | 1.0 | Vue 3 移行完了、デファクト(Pinia/Nuxt)健全 |
| Angular | 4 | 1.0 | 公式統合厚、ただしコミュニティ ライブラリは減少気味 |

---
```

- [ ] **Step 2: Section 6 が正しく書き込まれていることを確認**

確認: `Read` で G1〜G5 が記載されていることを目視確認。

- [ ] **Step 3: Commit**

```bash
git add docs/checks/05_js_framework_for_ionic.md
git commit -m "docs(checks): add G1-G5 long-term maintenance scoring

LTS・破壊的変更・Bus Factor・商用サポート・エコ健全性を採点。

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

### Task 7: Section 1 サマリを確定値で埋める

**Files:**
- Modify: `docs/checks/05_js_framework_for_ionic.md`(Section 1 のプレースホルダを置換)

- [ ] **Step 1: Section 1 のプレースホルダ「(Task 7 で確定値に置換)」を実値に置換**

`docs/checks/05_js_framework_for_ionic.md` の `## 1. サマリ(★後で埋める)` から `(Task 7 で確定値に置換)` までを以下に置換:

```markdown
## 1. サマリ

### 1.1 カテゴリ別平均と総合★(初期スコア=重み 1.0)

| FW | I. Ionic 統合 | P. PWA | S. 学習・人材 | D. 開発体験 | G. 長期保守 | **総合平均** | **★** |
|---|---|---|---|---|---|---|---|
| React | 4.6 ★★★★★ | 4.2 ★★★★ | 4.0 ★★★★ | 4.0 ★★★★ | 4.6 ★★★★★ | **4.28** | ★★★★ |
| Vue | 4.6 ★★★★★ | 4.8 ★★★★★ | 4.6 ★★★★★ | 4.6 ★★★★★ | 2.8 ★★★ | **4.28** | ★★★★ |
| Angular | 4.8 ★★★★★ | 4.4 ★★★★ | 3.4 ★★★ | 4.6 ★★★★★ | 4.0 ★★★★ | **4.24** | ★★★★ |

> 総合★は 3 FW とも ★★★★ で並ぶ。差はカテゴリ別重みで明確化する。

### 1.2 一行所感

- **Vue**: 学習・人材・開発体験で圧倒、長期保守(LTS / 破壊的変更)が明確な弱点
- **React**: バランス型、長期保守と人材で強い、JSX/Hooks の学習負荷大
- **Angular**: Ionic 統合度トップ・TS 型安全強・Form 強い、学習と人材プールで劣る

### 1.3 強みと弱みの可視化

| 観点 | 最強 | 最弱 |
|---|---|---|
| 学習速度 | Vue (S1=5) | React (S1=2) |
| HTML 親和性 | Vue (S2=5) | React (S2=3) |
| 国内人材 | React (S3=5) | Angular (S3=3) |
| リアクティビティ表現 | Vue (D1=5) | React (D1=3) |
| Form 強度 | Angular (D3=5) | React/Vue (D3=4) |
| 型安全 | Angular (D4=5) | React/Vue (D4=4) |
| LTS | Angular (G1=5) | Vue (G1=2) |
| 後方互換 | React (G2=5) | Vue (G2=2) |
| Bundle size | Vue (P2=5) | Angular (P2=3) |
| Service Worker 統合 | Angular (P1=5) | React (P1=3) |

---
```

- [ ] **Step 2: Section 1 が確定値で埋まっていることを確認**

確認: `Read docs/checks/05_js_framework_for_ionic.md` で Section 1 が 3 つの表(カテゴリ別平均/一行所感/強み弱み)で構成されていることを目視確認。

- [ ] **Step 3: Commit**

```bash
git add docs/checks/05_js_framework_for_ionic.md
git commit -m "docs(checks): fill in summary section with category averages

カテゴリ別平均・総合★・一行所感・強み弱み可視化を Section 1 に投入。

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

### Task 8: Section 7 候補絞り込みヒント + Section 8 参照リンク + 更新履歴

**Files:**
- Modify: `docs/checks/05_js_framework_for_ionic.md`(末尾に追記)

- [ ] **Step 1: Section 7/8/更新履歴 を末尾に追記**

```markdown
## 7. 候補絞り込みのヒント

汎用観点でのざっくり指針(プロジェクト固有事情は別途重み調整で表現):

| 最優先したい観点 | 推奨 FW | 理由 |
|---|---|---|
| 学習速度 / 国内人材容易性 | **Vue** | S1=5, S3=4, S4=5 |
| 長期保守 / 後方互換 / 大エコシステム | **React** | G1=4, G2=5, G3=5, G5=5 |
| TS 型安全 / 複雑業務フォーム / Ionic 統合度 | **Angular** | D3=5, D4=5, I1=5, I3=5 |
| PWA 軽量配信(Bundle size 優先)| **Vue** | P2=5 |
| 既存 HTML/CSS 資産活用 | **Vue** | S2=5(SFC `<template>` が HTML 直書き)|
| 大規模 SI 案件・厳格な型運用 | **Angular** | D4=5, I3=5, G1=5 |
| Next.js 級の SSR/SSG 同時展開 | **React / Vue** | P5=5 |
| 3 観点(I/P/S/D/G)すべて中庸 | 案件規模・チーム既存スキルで決定 | 総合★は 3 FW とも ★★★★ |

### 重み調整シナリオ例

利用者は本シートの重み列(デフォルト 1.0)を以下のように調整して再計算する:

| シナリオ | 重み調整例 |
|---|---|
| 学習速度最優先 | S1 を 3.0、S2 を 2.0 に |
| エンタープライズ長期保守 | G1/G2/G4 を 3.0 に |
| PWA 軽量配信 | P1/P2/P3 を 2.5 に |
| 業務フォーム中心 | D3/D4 を 2.5、I3 を 2.0 に |
| HT/PWA 混在(社内配信)| P1/P2 + I5 を 2.0 に |

---

## 8. 参照リンク

### 一次データ

| 対象 | 場所 |
|---|---|
| Ionic + Capacitor 詳細プロファイル | [`docs/framework_comparison/profile_ionic_capacitor.md`](../framework_comparison/profile_ionic_capacitor.md) |
| 観点 A〜H・F 節(LTS/Bus Factor/破壊的変更/商用サポート)| [`docs/framework_comparison/selection_criteria.md`](../framework_comparison/selection_criteria.md) |
| 13 FW 一覧と 3 軸サマリ | [`docs/checks/00_overview.md`](./00_overview.md) |
| docs/checks/ スコアリング規約 | [`docs/checks/README.md`](./README.md) |

### 公式

| 対象 | URL |
|---|---|
| Ionic Framework | https://ionicframework.com/ |
| Capacitor | https://capacitorjs.com/ |
| React | https://react.dev/ |
| Vue | https://vuejs.org/ |
| Angular | https://angular.dev/ |
| `@ionic/react` | https://ionicframework.com/docs/react |
| `@ionic/vue` | https://ionicframework.com/docs/vue |
| `@ionic/angular` | https://ionicframework.com/docs/angular |

### PWA / ビルドツール

| 対象 | URL |
|---|---|
| vite-plugin-pwa | https://vite-pwa-org.netlify.app/ |
| @angular/service-worker | https://angular.dev/ecosystem/service-workers |
| Workbox | https://developer.chrome.com/docs/workbox |

---

## 更新履歴

- **2026-05-11**: 初版作成(設計仕様 `2026-05-11-js-framework-for-ionic-scorecard-design.md` に基づく)
```

- [ ] **Step 2: Section 7/8/更新履歴 が正しく書き込まれていることを確認**

確認: `Read` で Section 7(候補絞り込みヒント)、Section 8(参照リンク)、更新履歴 が記載されていることを目視確認。

- [ ] **Step 3: Commit**

```bash
git add docs/checks/05_js_framework_for_ionic.md
git commit -m "docs(checks): add hints, references, and changelog sections

候補絞り込みヒント・重み調整シナリオ・参照リンク・更新履歴を追加。

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

### Task 9: README.md にファイル構成を追記

**Files:**
- Modify: `docs/checks/README.md`(ファイル構成テーブルに 1 行追加)

- [ ] **Step 1: README.md のファイル構成テーブルに 05_js_framework_for_ionic.md を追記**

`docs/checks/README.md` の以下の表:

```markdown
| ファイル | 内容 |
|---|---|
| [`00_overview.md`](./00_overview.md) | サマリ + 端末ターゲット表 + 3種★点 |
| [`01_business_suitability.md`](./01_business_suitability.md) | 業務適正度 8 項目(B1〜B8) |
| [`02_learning_difficulty.md`](./02_learning_difficulty.md) | 習得難易度 8 項目(L1〜L8、5=易) |
| [`03_developer_experience.md`](./03_developer_experience.md) | 開発体験・開発環境 8 項目(D1〜D8) |
| [`04_language_features.md`](./04_language_features.md) | 言語仕様(事実情報、スコアなし) |
```

を以下のように置換(末尾に 1 行追加):

```markdown
| ファイル | 内容 |
|---|---|
| [`00_overview.md`](./00_overview.md) | サマリ + 端末ターゲット表 + 3種★点 |
| [`01_business_suitability.md`](./01_business_suitability.md) | 業務適正度 8 項目(B1〜B8) |
| [`02_learning_difficulty.md`](./02_learning_difficulty.md) | 習得難易度 8 項目(L1〜L8、5=易) |
| [`03_developer_experience.md`](./03_developer_experience.md) | 開発体験・開発環境 8 項目(D1〜D8) |
| [`04_language_features.md`](./04_language_features.md) | 言語仕様(事実情報、スコアなし) |
| [`05_js_framework_for_ionic.md`](./05_js_framework_for_ionic.md) | Ionic+Capacitor PWA 向け JS FW(React/Vue/Angular)5×5=25 項目 |
```

- [ ] **Step 2: README.md の更新履歴も追記**

`docs/checks/README.md` の末尾の更新履歴セクション:

```markdown
## 更新履歴

- **2026-04-28**: 初版作成
```

を以下に置換:

```markdown
## 更新履歴

- **2026-04-28**: 初版作成
- **2026-05-11**: `05_js_framework_for_ionic.md`(Ionic 採用後の JS FW サブ評価)を追加
```

- [ ] **Step 3: README.md の変更を確認**

確認: `Read docs/checks/README.md` でファイル構成テーブル・更新履歴 が更新されていることを目視確認。

- [ ] **Step 4: Commit**

```bash
git add docs/checks/README.md
git commit -m "docs(checks): reference 05_js_framework_for_ionic in README

ファイル構成テーブルと更新履歴に新シートへの参照を追加。

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>"
```

---

### Task 10: 最終検証(完成シート全体の通し読み)

**Files:**
- Read only: `docs/checks/05_js_framework_for_ionic.md`, `docs/checks/README.md`

- [ ] **Step 1: 完成シート全体を読み、整合性を確認**

確認項目:
- 25 項目(I1〜I5, P1〜P5, S1〜S5, D1〜D5, G1〜G5)すべてが「アンカー表 + スコア表 + 備考」の3要素で構成されている
- Section 1 のカテゴリ別平均が、Section 2〜6 の個別スコアの算術平均と一致する
- Section 1 の総合平均(React 4.28, Vue 4.28, Angular 4.24)が再計算で一致する
- 全リンクが正しく解決される(相対パス: `../framework_comparison/...`、`./README.md` 等)
- 「重み 1.0」がすべてのスコア行に記載されている
- ★換算が docs/checks/README.md の規約(4.5以上=★5, 3.5〜4.49=★4, 2.5〜3.49=★3)と一致

- [ ] **Step 2: 平均値の手計算検証**

| FW | I 合計 | P 合計 | S 合計 | D 合計 | G 合計 | 25 項目合計 | 総合 |
|---|---|---|---|---|---|---|---|
| React | 4+5+4+5+5=23 | 3+4+4+5+5=21 | 2+3+5+5+5=20 | 3+4+4+4+5=20 | 4+5+5+4+5=23 | 107 | 107/25=**4.28** ✓ |
| Vue | 4+5+4+5+5=23 | 4+5+5+5+5=24 | 5+5+4+5+4=23 | 5+5+4+4+5=23 | 2+2+3+3+4=14 | 107 | 107/25=**4.28** ✓ |
| Angular | 5+4+5+5+5=24 | 5+3+5+5+4=22 | 3+4+3+3+4=17 | 4+4+5+5+5=23 | 5+3+4+4+4=20 | 106 | 106/25=**4.24** ✓ |

すべて一致確認。

- [ ] **Step 3: Git log で 9 個のコミットを確認**

Run: `git log --oneline -n 12`
Expected: Task 1〜9 で計 9 コミットが順に並ぶ + 設計spec コミット(`72cf885`)が前にある。

検証完了。実装フェーズ終了。

---

## Self-Review チェック結果

- **Spec coverage**: 設計仕様の全章を以下のタスクで実装
  - Spec 2章(評価対象/評価軸/スコアリング規約)→ Task 1 Section 0
  - Spec 3章(ファイル構成)→ Task 1 ファイル作成
  - Spec 4章(評価項目の詳細 I/P/S/D/G)→ Task 2〜6
  - Spec 5章(初期スコア)→ Task 2〜6 のスコア表、Section 1 サマリは Task 7
  - Spec 6章(アンカー言語化方針)→ Task 2〜6 の各項目のアンカー表
  - Spec 7章(利用ワークフロー)→ Task 8 Section 7 の重み調整シナリオ
  - Spec 8章(候補絞り込みヒント)→ Task 8 Section 7
  - Spec 9章(一次データ参照)→ Task 8 Section 8
  - Spec 12章(実装フェーズの予定)→ Task 1〜9 全体
- **Placeholder scan**: Task 1 の「(Task 7 で確定値に置換)」は Task 7 で必ず置換、それ以外に TBD/TODO/後で実装 等の placeholder なし
- **Type consistency**: I1〜I5, P1〜P5, S1〜S5, D1〜D5, G1〜G5 の項目コードはタスク間で一貫
- **平均値一致**: Task 10 で手計算検証手順を明記、Section 1 と個別スコアの整合を保証
