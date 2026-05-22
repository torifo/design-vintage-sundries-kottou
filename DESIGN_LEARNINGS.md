# IKKOKU Design Learnings

## 目的

kottouペルソナ（50代美術史講師の真剣コレクター）は、商業的なECのトーンを「信頼に値しない」と判断する層。本サイトは「美術館図録の格調を Web に翻訳する」を到達目標に、漆黒・縦書き・朱印という、書物としての和骨董店を構築した。

このブランドは架空店舗であり、実在の店舗・商品ではない。

## 設計したこと

- ヒーローを「床の間に掛けた軸物」に見立て、上下を渋金の表装で挟んだ縦書き引用にした。
- 商品（逸品列伝）には来歴・寸法・状態・箱・付属を `dl` で完全構造化し、図録のフォーマットを踏襲した。
- 「鑑識の言」セクションで店主の哲学を縦書き引用＋本文＋朱印で語る——商品より先に思想を読ませる。
- 「来歴」は5項目の年表で、創業の昭和五十二年から現代の三代目襲名まで、商家としての継承性を提示。
- 「来店」は完全予約制とし、商業的な CTA を一切排した。

## CSS

主要なCSS判断:

```css
:root {
  --shikkoku:#0c0a08;       /* 漆黒 */
  --shibukin:#b88a3a;        /* 渋金 */
  --gunjo:#1f2e4a;           /* 群青 */
  --hakuji:#f4f0e6;          /* 白磁 */
  --shuin:#a8302a;           /* 朱印 */
  --font-yu:'Yuji Syuku','Shippori Mincho B1',serif;
}

/* 床の間ヒーロー — 軸物 */
.jiku {
  border-top: 8px solid var(--shibukin-koi);
  border-bottom: 8px solid var(--shibukin-koi);
}
.jiku::before, .jiku::after {
  content: "";
  position: absolute; left: 50%;
  width: 30px; height: 14px;
  background: var(--shibukin);
}

/* 縦書きキャッチ */
.jiku-text { writing-mode: vertical-rl; line-height: 2.2; letter-spacing: .6em }

/* 渋金グラデ文字 */
.gold {
  background: linear-gradient(120deg,
    var(--shibukin-koi), var(--shibukin) 50%, var(--shibukin-koi));
  -webkit-background-clip: text; background-clip: text;
  color: transparent;
}
```

## アニメーション

- `shizuka` リビールを 10px / 1.4s で。茶事のような「ゆっくりと現れる」所作。
- `prefers-reduced-motion: reduce` で全アニメーション停止。
- ホバー時のアニメは nav の下線スライドのみ。商品カードは静止。

## フォント

- 主見出し: `Yuji Syuku`（落款的、書道の伝統に最も近い和フォント）
- サブ見出し: `Shippori Mincho B1`（しっぽり明朝）
- 本文: `Noto Serif JP` w300-400（薄めの本文で品格を維持）

英字書体は明示的に不採用。「江戸後期の和骨董」を扱う店に Didone 系の英字は不適切。

## ボタン

ボタンは「ご案内」の体裁。

- 「来店予約のお問い合わせ」は線のみのリンク。`background` も `box-shadow` もない。
- ホバー時のみ `background: var(--shibukin)` で塗り、`color` を漆黒に反転。

## UI/UX

- ナビ4項目（逸品列伝 / 鑑識の言 / 主人 / 来店）。「販売」「カート」「お気に入り」のような商業UIは存在しない。
- 商品カードは「第 壹 號」「第 貳 號」と號数で並べ、商品 No ではなく作品 No として扱う。
- 古物商番号、所属（京都美術倶楽部）、当代主人の経歴（東京国立博物館 客員研究員）を明記し、商売の正当性を担保する。

## レイアウト

横書きと縦書きを混在させ、見出し・章番号・引用には縦書きを多用。本文は横書きにすることで、可読性と図録的格調の両立を図った。

商品カードは2カラムで4品。各カードは左に SVG（暗色フレーム＋渋金縁＋内側ボーダー）、右に款記キャプションの「款記レイアウト」を採用。

ダーク背景の上に和紙的なグレインを `mix-blend-mode: overlay` で重ね、漆器の経年で生じる微細な質感を表現した。
