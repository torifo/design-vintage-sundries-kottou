# 古美術 一徳 — vintage-sundries/kottou Spec

**Status:** Approved
**Author:** torifo
**Created:** 2026-05-23
**Updated:** 2026-05-23

---

## 1. Overview

### Problem Statement
和骨董を真剣に蒐集する50代以上のコレクターは、商業性が強すぎるEC的なデザインを「信頼に値しない」と感じる。美術館図録・茶道具会のような「格調」の Web 翻訳が市場に少なく、本物を扱う古美術商が新規顧客と出会う Web 上の場が乏しい。

### Goal
京都・上京区の老舗古美術商（三代目）という設定で、江戸後期〜大正の和骨董を扱う架空店舗「古美術 一徳（IKKOKU）」を、漆黒×渋金×群青×白磁の重厚配色、Yuji Syuku＋ Shippori Mincho B1 の縦書き多用、款記・朱印モチーフ、巻物的UIで実装し、美術館・図録のような格調を Web で再現する。

### Non-Goals
- カート、即時購入、ポイント機能
- SNS シェアボタン
- ライトモード、軽い装飾

### Background
- 古美術 一徳（IKKOKU）は本デザイン研究のための架空店舗
- 同シリーズ4作の中で「ダーク・縦書き多用・格調」が唯一のサイト

---

## 2. User Stories

| ID | Persona | Want to | So that |
|----|---------|---------|---------|
| US-01 | 美術史講師の50代コレクター（佐藤 雅彦想定） | ページを開いた瞬間に「美術館図録の格」を感じたい | 信頼に値する店だと直感できる |
| US-02 | 同上 | 商品の来歴 ・ 状態 ・ 寸法を詳細に確認したい | 真贋を判断する材料が揃う |
| US-03 | 同上 | 店主の鑑識観を読みたい | 価格より前に商売の姿勢を理解したい |
| US-04 | 同上 | 来店予約フローを確認したい | 完全予約制の店をしっかり訪問できる |

### Acceptance Criteria

**US-01:**
- WHEN ページが読み込まれた THEN 床の間（軸物）風の縦書き＋大型明朝主題が現れる
- WHEN 表示後 THEN 漆黒の背景＋渋金のアクセントで美術館的「格」が感じられる

**US-02:**
- WHEN 逸品列伝セクションに到達した THEN 4品の SVG＋名称＋時代＋寸法＋状態＋付属＋価格が確認できる
- WHEN 商品の説明文を読んだ THEN 鑑定書付・本歌・修理なしなどの語彙が使われている

**US-03:**
- WHEN 「鑑識の言」セクションに到達した THEN 縦書き引用＋本文＋朱印で店主の哲学が示される

**US-04:**
- WHEN 「来店」セクションに到達した THEN 完全予約制 ・ 鑑定相談フロー ・ 古物商番号が表示される

---

## 3. Functional Requirements

| ID | Requirement | Priority | Notes |
|----|-------------|----------|-------|
| FR-01 | 床の間ヒーロー（軸物風縦書き） | P0 | 上下に渋金の表装 |
| FR-02 | 逸品列伝（4商品 SVG＋カード） | P0 | 寸法・状態・箱・価格 |
| FR-03 | 鑑識の言（縦書き＋本文＋朱印） | P0 | 店主哲学 |
| FR-04 | 当店の来歴（年表5項目） | P1 | 縦のタイムライン |
| FR-05 | 当代主人（プロフィール＋勲功バッジ） | P0 | 美術倶楽部所属 |
| FR-06 | 御来店＋鑑定予約 | P0 | 古物商番号必須 |
| FR-07 | スクロールリビール（shizuka） | P1 | 10px、1.4s |
| FR-08 | ヘッダー（朱印＋ブランド明朝） | P0 | sticky＋backdrop-filter |
| FR-09 | モバイル対応（375px） | P0 | 縦書きを横書きにフォールバック |

---

## 4. Architecture

```
kottou/index.html
├── <header>                       # 朱印＋明朝ロゴ
├── <section class="tokonoma">     # 床の間ヒーロー
├── <section id="ippin">           # 逸品列伝 4品
├── <section id="kanshiki">        # 鑑識の言
├── <section id="raireki">         # 来歴 年表
├── <section id="shujin">          # 当代主人
├── <section id="annai">           # 御来店＋鑑定予約
└── <footer>                       # 4店舗ナビ
```

### Key Design Decisions

| Decision | Chosen | Rationale | Rejected |
|----------|--------|-----------|----------|
| テーマ | ダーク（漆黒） | 美術館図録の格 | ライト |
| 主書体 | Yuji Syuku | 落款的明朝 | Hina Mincho（軽い） |
| 装飾 | 朱印 ・ 識箱 ・ 軸物 | 古美術の語彙 | テープ ・ ポラロイド |
| アニメ | shizuka（静か） | 茶事的所作 | 強いスライド |
| 縦書き | 多用 | 縦組み図録 | 全横書き |

---

## 5. Design System

### Color Palette
```css
--shikkoku:#0c0a08;        /* 漆黒 */
--kuro:#1a1612;
--gunjo:#1f2e4a;            /* 群青 */
--gunjo-asa:#3a4a6a;
--shibukin:#b88a3a;         /* 渋金 */
--shibukin-koi:#8a6325;
--hakuji:#f4f0e6;           /* 白磁 */
--hakuji-uss:#d8d0c0;
--shuin:#a8302a;            /* 朱印 */
--kobi:#6a4a30;
```

### Typography
```css
--font-yu:'Yuji Syuku','Shippori Mincho B1',serif;            /* 見出し */
--font-shippori:'Shippori Mincho B1','Noto Serif JP',serif;    /* サブ見出し ・ キャプション */
--font-honbun:'Noto Serif JP',serif;                            /* 本文 */
```

### Texture & Motion
```css
/* 漆の微細質感 */
body::before {
  background: url("...feTurbulence baseFrequency=.95...");
  mix-blend-mode: overlay;
  opacity: .5;
}

/* shizuka リビール */
@keyframes shizuka{ from{ opacity:0; transform: translateY(10px) } to{ opacity:1; transform: none } }
.reveal{ animation: shizuka 1.4s cubic-bezier(.2,.7,.2,1) both }
```

---

## 9. Testing Strategy

| Layer | Scenarios |
|-------|-----------|
| Desktop (1280px) | 軸物縦書き表示、商品 SVG の暗色フレーム、縦組キャプション可読 |
| Mobile (375px) | 軸物縦書きはカラム上に積み、商品カードは縦並びに自動再構成 |
| アニメ | shizuka 1回のみ、`prefers-reduced-motion` で停止 |
| ホバー | nav 下線が左→右へスライド、商品カードは静止 |
| フォント | Yuji Syuku ・ Shippori Mincho B1 ・ Noto Serif JP 正常 |

---

## 10. Implementation Notes

- **床の間軸物**: 縦書きテキストを上下に渋金の表装帯（`border-top:8px / border-bottom:8px`）で挟み、上下に丸付ピンを `::before/::after` で配置
- **朱印モチーフ**: 全セクションで `background: var(--shuin)` の四角＋白文字「徳/印」を統一マークとして使用
- **款記キャプション**: 商品名の下に小サイズの英字 ・ 時代バッジ ・ 寸法 ・ 状態 ・ 箱を `dl` で構造化
- **来歴年表**: 縦線＋斜め四角のマーカーで「巻物的」連続性を表現
- **完全予約制**: 来店ボタンを「来店予約のお問い合わせ」と書字、商業的 CTA を回避

---

## 11. Open Questions

| # | Question | Status |
|---|----------|--------|
| 1 | 商品 SVG を実写へ差し替えるか | Open |
| 2 | 鑑定書 PDF のサンプルを掲載するか | Open |
| 3 | 来店予約をフォーム化するか、書面のみとするか | Open |

---

## References

- [navigator README](../README.md)
- Font: [Yuji Syuku](https://fonts.google.com/specimen/Yuji+Syuku), [Shippori Mincho B1](https://fonts.google.com/specimen/Shippori+Mincho+B1)
- Inspiration: 五島美術館図録、根津美術館 Web、出光美術館図録、京都美術倶楽部関連書籍
