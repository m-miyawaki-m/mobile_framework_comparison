# Ionic+Capacitor PWA 向け JS フレームワーク (React/Vue/Angular) 評価スコアシート 設計仕様

- **作成日**: 2026-05-11
- **対象成果物**: `docs/checks/05_js_framework_for_ionic.md`(単一ファイル)
- **既存の関連資料**:
  - [`docs/checks/README.md`](../../checks/README.md) — スコアリング規約(1〜5整数、★換算、重み列)
  - [`docs/checks/00_overview.md`](../../checks/00_overview.md) — 13 FW サマリ(本シートは 5 番 Ionic + 13 番 PWA の細分化)
  - [`docs/framework_comparison/profile_ionic_capacitor.md`](../../framework_comparison/profile_ionic_capacitor.md) — Ionic/Capacitor 一次データ
  - [`docs/framework_comparison/selection_criteria.md`](../../framework_comparison/selection_criteria.md) — 観点 A〜H の母集団(F3 Bus Factor、F5 LTS、F6 破壊的変更 等を参照)

---

## 1. 目的

「PWA を Ionic + Capacitor で構築する」前提が確定した後の **JS フレームワーク層(React / Vue / Angular)選定** に使う汎用スコアシートを整備する。docs/checks/ 本体は 13 FW のマクロ比較を扱うが、本シートはその下位(Ionic 採用後の「中の JS FW 選び」)を専門に扱う **サブ評価**。

### 設計原則

| 原則 | 内容 |
|---|---|
| docs/checks/ 規約継承 | 採点スケール(1〜5整数、5=望ましい)、★換算、重み列、N/A 規約は `docs/checks/README.md` をそのまま継承 |
| プロジェクト中立 | jQuery 移行・Android HT・Windows 開発などのプロジェクト固有事情は持ち込まず、Ionic+Capacitor PWA 開発の一般論で採点 |
| JS FW 専用軸 | docs/checks/ の B/L/D 3 軸ではなく、JS FW 選定固有の 5 カテゴリで設計(Ionic 統合度・PWA 適性・学習人材・JS FW 固有開発体験・長期保守) |
| アンカー方式 | 1〜5 各点に言語化された判定基準を付与(docs/checks/ 本体と同じ方式) |
| 一次データ参照 | スコアの根拠は `profile_ionic_capacitor.md` および `selection_criteria.md` の F3/F5/F6/F8 等を引用 |

### 非目標

- 「Ionic vs 他 FW」の比較は再掲しない(docs/checks/ 本体で扱い済)
- 「Ionic + Capacitor 以外の JS FW 採用」(例: Vanilla JS、Stencil 単体)の評価は対象外
- 詳細なコード例・ライブラリ実装ガイドは含めない(プロファイルや別資料で扱う)

---

## 2. スコープ

### 評価対象 (3 FW)

| # | JS FW | バージョン | Ionic バインディング |
|---|---|---|---|
| 1 | React | 18+ / 19 想定 | `@ionic/react` |
| 2 | Vue | 3.x | `@ionic/vue` |
| 3 | Angular | 17+ / 18+ 想定 | `@ionic/angular` |

### 評価軸 (5 カテゴリ × 5 項目 = 計 25 項目)

| カテゴリ | コード | 項目数 |
|---|---|---|
| Ionic/Capacitor 統合度 | I1〜I5 | 5 |
| PWA 適性 | P1〜P5 | 5 |
| 学習・人材 | S1〜S5 | 5 |
| 開発体験(JS FW 固有) | D1〜D5 | 5 |
| 長期保守・ガバナンス | G1〜G5 | 5 |

### 採点ルール(docs/checks/README.md 継承)

- **1〜5 整数**(0.5 刻み不可)、**5 = 望ましい**
- **★ 換算**: 4.5 以上=★★★★★/3.5〜4.49=★★★★/2.5〜3.49=★★★/1.5〜2.49=★★/1.5 未満=★
- **集計**: 単純平均 / 重み付き平均
- **重み**: デフォルト 1.0、利用者が 0.0〜5.0 で調整
- **N/A**: 分母から除外

---

## 3. 成果物のファイル構成

```
docs/checks/
└── 05_js_framework_for_ionic.md   ← 本仕様で新規作成
```

**単一ファイル**で構成する。docs/checks/ 本体(01〜03)は 1 軸=1 ファイルだが、本シートは下位サブ評価で項目数が小さい(各カテゴリ 5 項目)ため分割不要。

### ファイル内構成

```markdown
# Ionic+Capacitor PWA 向け JS フレームワーク 評価スコアシート

## 0. 評価対象とスコアリング規約
   - 3 FW 一覧、採点スケール、★換算、重み列、N/A 規約(README.md 参照)

## 1. サマリ
   - カテゴリ別平均と総合★(React / Vue / Angular)
   - 一行所感

## 2. Ionic/Capacitor 統合度 (I1〜I5)
   - 項目ごとに 1〜5 アンカー + スコア表 + 備考

## 3. PWA 適性 (P1〜P5)
   - 同上

## 4. 学習・人材 (S1〜S5)
   - 同上

## 5. 開発体験 - JS FW 固有 (D1〜D5)
   - 同上

## 6. 長期保守・ガバナンス (G1〜G5)
   - 同上

## 7. 候補絞り込みのヒント
   - 「学習速度優先なら Vue」「長期保守優先なら React」「型安全・大規模なら Angular」等

## 8. 参照リンク
   - 一次データへのリンク

## 更新履歴
```

---

## 4. 評価項目の詳細

### 4.1 I. Ionic/Capacitor 統合度

| # | 項目 | 評価観点 |
|---|---|---|
| I1 | 公式バインディング成熟度 | `@ionic/<fw>` の歴史・実装完成度・GitHub stars/issue 応答性 |
| I2 | Live Reload 品質 | `ionic serve` / `cap run --livereload` の安定度・HMR 速度 |
| I3 | Router/Navigation 統合 | Ionic の Stack-based Navigation と FW Router の親和性 |
| I4 | コンポーネント網羅性 | Web Components ラッパが ion-* 全要素を露出しているか |
| I5 | Capacitor Plugin 相性 | プラグイン公式例・ドキュメント・サンプルの FW 別充実度 |

### 4.2 P. PWA 適性

| # | 項目 | 評価観点 |
|---|---|---|
| P1 | Service Worker 統合 | 公式 SW モジュール / Workbox 統合の容易度 |
| P2 | Bundle size | プロダクションビルドの初期ロードサイズ典型値 |
| P3 | PWA ビルドツール | manifest.json / SW 自動生成の公式 schematic / plugin |
| P4 | オフライン構築容易度 | IndexedDB / Workbox / 永続化ライブラリの統合 |
| P5 | SSR/SSG 対応 | Next.js (React) / Nuxt (Vue) / Angular Universal の充実度 |

### 4.3 S. 学習・人材

| # | 項目 | 評価観点 |
|---|---|---|
| S1 | 立ち上げ期間 | 一般プログラマ(他言語経験あり)が実務 OK までの期間(5=短) |
| S2 | HTML/CSS 資産親和性 | 既存 HTML テンプレ流用容易度(テンプレート vs JSX) |
| S3 | 国内人材プール | 採用容易度・求人数 |
| S4 | 日本語資料 | 書籍・Qiita/Zenn・コース・公式翻訳の充実度 |
| S5 | AI 補完精度 | Copilot / Claude の学習データ量に基づく補完精度 |

### 4.4 D. 開発体験(JS FW 固有)

| # | 項目 | 評価観点 |
|---|---|---|
| D1 | リアクティビティ表現力 | `useState` / `ref` / `Signals` の直感性・記述量 |
| D2 | 状態管理 | 公式・デファクトの確立度(Pinia / Redux / NgRx 等) |
| D3 | Form/Validation | フォームバインド・検証ライブラリの公式 / デファクト充実度 |
| D4 | 型安全 (TS 統合度) | strict TS との親和性・テンプレート/JSX 内の型推論 |
| D5 | Lint/Formatter | ESLint plugin・公式ガイドラインの充実度 |

### 4.5 G. 長期保守・ガバナンス

| # | 項目 | 評価観点 |
|---|---|---|
| G1 | LTS | 明示 LTS の有無・期間(selection_criteria.md F5 参照) |
| G2 | 破壊的変更頻度 | メジャーアップグレード時の影響度(F6 参照、5=少ない) |
| G3 | Bus Factor | 開発元の堅牢性(F3 参照) |
| G4 | 商用サポート | ベンダー直契約・SLA(F8 参照) |
| G5 | エコシステム健全性 | デファクトライブラリの保守状況・更新追従 |

---

## 5. 初期スコア(試案)

各項目の初期スコアと一行根拠。**最終値はアンカー言語化後にレビューで微調整する**。

### 5.1 I. Ionic/Capacitor 統合度

| # | React | Vue | Angular | 根拠概要 |
|---|---|---|---|---|
| I1 | 4 | 4 | **5** | Angular は Ionic 1 (2013) からの歴史的本家、React/Vue は Ionic 4 (2019) から |
| I2 | 5 | 5 | 4 | Vite ベースの React/Vue は HMR が瞬時、Angular CLI は若干重い |
| I3 | 4 | 4 | **5** | Angular Router + Ionic Routing が最も成熟、React/Vue は Stack Navigation 制約あり |
| I4 | 5 | 5 | 5 | Web Components 実装、3 FW 同等 |
| I5 | 5 | 5 | 5 | Capacitor Plugin は FW 非依存、3 FW 同等 |

### 5.2 P. PWA 適性

| # | React | Vue | Angular | 根拠概要 |
|---|---|---|---|---|
| P1 | 3 | 4 | **5** | Angular Service Worker 公式モジュール、Vue は vite-plugin-pwa、React は Workbox 手動寄り |
| P2 | 4 | **5** | 3 | Vue 3 最軽量(< 30 KB)、Angular CLI は最適化必要 |
| P3 | 4 | 5 | 5 | `@angular/pwa` schematic / `vite-plugin-pwa` が標準、React は CRA 非推奨化で手動寄り |
| P4 | 5 | 5 | 5 | Workbox/IndexedDB は FW 非依存 |
| P5 | **5** | **5** | 4 | Next.js / Nuxt が最強、Angular Universal は設定複雑 |

### 5.3 S. 学習・人材

| # | React | Vue | Angular | 根拠概要 |
|---|---|---|---|---|
| S1 | 2 | **5** | 3 | Vue 3 は HTML テンプレ近似で最短、React は Hooks/JSX 学習負荷大、Angular は DI/RxJS |
| S2 | 3 | **5** | 4 | Vue は HTML 直書き、Angular もテンプレ、React は JSX 変換要 |
| S3 | **5** | 4 | 3 | 国内求人は React >> Vue > Angular(selection_criteria.md B4) |
| S4 | 5 | 5 | 3 | React/Vue は日本語資料豊富、Angular は中程度 |
| S5 | 5 | 4 | 4 | React は学習データ最大、Vue/Angular も TS 型情報で補完精度高 |

### 5.4 D. 開発体験(JS FW 固有)

| # | React | Vue | Angular | 根拠概要 |
|---|---|---|---|---|
| D1 | 3 | **5** | 4 | Vue ref/reactive 最直感、Angular Signals は v17+、React useState は依存配列負担 |
| D2 | 4 | **5** | 4 | Pinia は公式デファクト、React は Redux/Zustand/Jotai で選択肢多、NgRx は重厚 |
| D3 | 4 | 4 | **5** | Angular Reactive Forms 最強、Vue/React はライブラリ依存 |
| D4 | 4 | 4 | **5** | Angular TS 必須・最厳密、React JSX 型推論時々苦戦、Vue は Volar で改善中 |
| D5 | 5 | 5 | 5 | ESLint plugin 3 FW とも充実 |

### 5.5 G. 長期保守・ガバナンス

| # | React | Vue | Angular | 根拠概要 |
|---|---|---|---|---|
| G1 | 4 | 2 | **5** | Angular 6m Active+12m LTS 明示、Vue は明示 LTS なし(Vue 2 EOL 前例)、React は事実上後方互換 |
| G2 | **5** | 2 | 3 | React 最後方互換重視、Vue 2→3 は再書、Angular は `ng update` 補助あり中程度 |
| G3 | **5** | 3 | 4 | React は Meta+Vercel+巨大エコ、Vue は Evan You+小チーム、Angular は Google |
| G4 | 4 | 3 | 4 | Angular は Google + パートナー、React は Meta 直契約なしだがコンサル多、Vue は限定的 |
| G5 | **5** | 4 | 4 | npm エコ規模最大は React、3 FW とも健全 |

### 5.6 サマリ(初期スコアでのカテゴリ別平均と総合★)

| FW | I | P | S | D | G | **総合** | **★** |
|---|---|---|---|---|---|---|---|
| **React** | 4.6 | 4.2 | 4.0 | 4.0 | 4.6 | **4.28** | ★★★★ |
| **Vue** | 4.6 | 4.8 | 4.6 | 4.6 | 2.8 | **4.28** | ★★★★ |
| **Angular** | 4.8 | 4.4 | 3.4 | 4.6 | 4.0 | **4.24** | ★★★★ |

総合★は 3 FW とも ★★★★(差はカテゴリ別重み付けで明確化)。

### 5.7 一行所感

- **Vue**: 学習・人材・開発体験で圧倒、長期保守(LTS / 破壊的変更)が明確な弱点
- **React**: バランス型、長期保守と人材で強い、JSX/Hooks の学習負荷大
- **Angular**: Ionic 統合度トップ・TS 型安全強・Form 強い、学習と人材プールで劣る

---

## 6. アンカー言語化方針

各項目で 1〜5 各点に対応する状態を 1 行で言語化する。例(I1 公式バインディング成熟度):

| 点 | 状態 |
|---|---|
| 5 | Ionic 1 系から継続的に維持され、公式チュートリアル・ドキュメントが最厚 |
| 4 | Ionic 4 以降の対応で十分成熟、公式サポートあり |
| 3 | バインディングは存在するが機能差や API ラグあり |
| 2 | コミュニティ実装中心、Ionic 公式の支援が薄い |
| 1 | 非公式 / 実用困難 / 開発中断 |

25 項目すべてに対しこの形式でアンカーを記載する。実装フェーズで一次データ(profile_ionic_capacitor.md、selection_criteria.md F3〜F8)を引用しながら確定。

---

## 7. 利用ワークフロー

1. `docs/checks/00_overview.md` で「PWA + Ionic + Capacitor」を方針確定済の前提
2. 本シート(`05_js_framework_for_ionic.md`)を開く
3. プロジェクトに応じて重み列を調整(例: 学習速度を優先 → S1/S2 を 3.0 に、長期保守 SLA 必須 → G1/G4 を 3.0 に)
4. 重み付き平均で順位を再計算
5. 上位 FW を最終候補とし、PoC で検証

---

## 8. 候補絞り込みのヒント(シート末尾に記載)

汎用観点でのざっくり指針:

- **学習速度 / 国内人材容易性を最優先** → **Vue**
- **長期保守 / 後方互換 / 大エコシステムを最優先** → **React**
- **TS 型安全 / Form 強度 / Ionic 統合度を最優先** → **Angular**
- **3 すべて中庸が必要** → 案件規模・チーム既存スキルで決定

---

## 9. 一次データ参照方針

各項目のスコア根拠は本シート内では概要のみ記載し、詳細は以下にリンク:

- Ionic 内部の詳細 → `docs/framework_comparison/profile_ionic_capacitor.md`(セクション 2.4 / 9 が中心)
- F3 Bus Factor、F5 LTS、F6 破壊的変更、F8 商用サポート → `docs/framework_comparison/selection_criteria.md`
- npm エコシステム / TypeScript / 人材市場 → `selection_criteria.md` B/C/D 節
- AI 補完相性 → `selection_criteria.md` D7 / 各 profile_*.md の H10 節

---

## 10. 更新ポリシー

- React 19、Angular 18+ などメジャーアップデート発生時に再評価
- Vue / Angular の LTS / EOL 通知時に G1 を再確認
- Ionic 本体メジャー(Ionic 9 想定)で I1〜I5 を再確認
- 本シートは派生資料。更新は profile_ionic_capacitor.md と selection_criteria.md を先に修正し、本シートは再生成

---

## 11. 将来の拡張ポイント

- **Solid.js / Svelte の追加**: 4 番目以降の評価対象として候補(Ionic 公式バインディングは現状なしのため当面参考扱い)
- **Stencil 単体評価**: Vanilla TS + Stencil パターンを参考列として追加(I/P 軸で高得点想定)
- **重み付きシナリオ集**: 「業務 PWA」「コンシューマ PWA」「Wedge HT」等のテンプレ重みパターンを別ファイルで配布

---

## 12. 実装フェーズの予定

1. `docs/checks/05_js_framework_for_ionic.md` を新規作成
2. 0〜8 章をマークダウンで記述
3. 25 項目それぞれにアンカー(5 段階の言語化)を記載
4. スコア表(R/V/A)を本仕様の 5.1〜5.5 を元に投入
5. サマリ(5.6)と所感(5.7)、絞り込みヒント(8 章)を記載
6. `docs/checks/README.md` の「ファイル構成」に本シートを追記
7. `docs/checks/00_overview.md` から本シートへの導線を追加(オプション)

---

## 更新履歴

- **2026-05-11**: 初版作成
