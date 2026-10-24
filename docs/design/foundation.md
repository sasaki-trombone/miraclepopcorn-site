# Foundation v0.1

## 方針と既存構成

[デザイン方針](../design.md)と[Issue #2](https://github.com/sasaki-trombone/miraclepopcorn-site/issues/2)に従い、最初の簡易モックと既存UIを視覚的な基準とする。WCAG 2.2 AA を目標にするが、準拠済みとは宣言しない。

Home / Blog / Profile は BaseLayout を共有し、そこから global.css を読み込む。Header / Footer に独自CSSはない。global.css の :root にあった7変数を、同名・同値で tokens.css に移した。global.css の先頭の @import から読み込み、BaseLayout やページ構造は変更しない。既存の文字サイズ・行高・フォントもトークンとして参照する。余白の一括置換、新規UI、依存パッケージは追加しない。

header / nav / main / footer、h1、セクションの aria-labelledby、日本語の lang と viewport は既存実装にある。現時点ではコンテンツ画像、フォーム、JavaScript、アニメーション、トランジション、スムーズスクロールはない。

## 採用値

採用とは「v0.1で既存の見た目を維持するための実装値」であり、将来の変更を禁止する意味ではない。

| トークン | 値 | 用途・採用理由 |
| --- | --- | --- |
| --color-background | #fbfaf7 | 既存のページ背景 |
| --color-surface | #ffffff | 既存のヘッダー背景の混色用 |
| --color-text | #1f1f1f | 既存の基本文字・見出し・ロゴ |
| --color-muted | #66645f | 既存の段落・ナビゲーション・フッター |
| --color-border | #e5e0d8 | 既存のヘッダー・フッターの装飾区切り専用 |
| --color-border-control | var(--color-muted) | 今後の意味のあるUI境界用。現在の装飾線は置換しない |
| --color-focus | var(--color-text) | 明るい背景上のフォーカス用 |
| --font-family-body | system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif | 既存のシステムフォント列。日本語は環境のフォールバックを利用 |
| --font-size-small | 0.9rem | 既存のフッター |
| --font-size-label | 0.95rem | 既存のナビゲーション・kicker。意味のある文字にも使用 |
| --font-size-heading | clamp(2rem, 8vw, 4.25rem) | 既存の h1 |
| --line-height-body | 1.8 | 既存の本文行高 |
| --line-height-heading | 1.18 | 既存の h1 行高 |
| --content-width | 760px | 既存のコンテナ最大幅 |
| --space-page | clamp(1.25rem, 4vw, 2.5rem) | 既存の左右余白 |

本文サイズはブラウザーの既定値（通常16px）を継承し、固定しない。段落幅34rem、導入部幅40rem、本文領域の上下余白 clamp(4rem, 12vw, 8rem) などは既存CSSに残す。未使用の階層・スケールは先回りして追加しない。

### 仮値・未決定事項

Issueの背景 #F3F0E8、本文 #22211E、補助文字 #6B655E、アクセント #A25737、装飾線 #C9C4BA はすべて未採用の提案値。既存の色と異なるため、変更する場合は別途視覚的なレビューを行う。アクセントの用途も未決定。

本文16–17px、補助14px、記事タイトル22–28px、セクション28–36px、Hero40–64px、装飾ラベル12px、余白4 / 8 / 16 / 24 / 40 / 64 / 96px、最大幅960px、本文約68ch、左右余白の上限4remも候補であり、採用値ではない。未実装UIで必要になった時点で検討する。12pxの装飾ラベルに意味のある情報を担わせない。Webフォントは追加しない。

## 使用方法とアクセシビリティ

通常のCSSから `color: var(--color-text)`、`font-family: var(--font-family-body)` のように参照する。今後の入力部品など、境界が判別に必要なら `border: 1px solid var(--color-border-control)` を使用し、実際の隣接色と各状態で再検証する。装飾用 --color-border を操作部品や選択状態の唯一の手掛かりにしない。

### コントラスト

不透明なsRGB値をWCAGの相対輝度式で計算し、小数第2位まで丸めた値：

| 前景 | 背景 #fbfaf7 | 背景 #ffffff | 用途 |
| --- | --- | --- | --- |
| #1f1f1f | 15.79:1 | 16.48:1 | 通常文字・フォーカス |
| #66645f | 5.66:1 | 5.91:1 | 補助文字・意味のあるUI境界 |
| #e5e0d8 | 1.26:1 | 1.31:1 | 装飾線のみ |

通常文字4.5:1以上、大きな文字3:1以上、意味のある非テキストUIは原則3:1以上を確認する。装飾線は3:1未満であり、意味のある境界には転用しない。ヘッダー背景はページ背景92%と白の混色で、上記2背景色の間にある。濃い背景、写真、透明度、選択・エラー等を追加したら実際の組合せで確認する。トークン単独ではコントラストを保証しない。

### フォーカス

共通の :focus-visible は --color-focus の2px実線と3pxの外側余白を使い、通常のボックス寸法には影響しない。outline: none は追加せず、ブラウザーのフォーカス挙動を消さない。今後の部品はフォーカスをクリップ・遮蔽せず、濃い背景では個別に適切な色を選ぶ。

### 動き

現状は動きがないため、animation / transition の一括無効化は行わない。prefers-reduced-motion: reduce ではルートの scroll-behavior を auto にする。今後の装飾的な動きやスムーズスクロールは原則 `@media (prefers-reduced-motion: no-preference)` 内でのみ有効にし、動きなしでも状態変化の意味を伝える。JavaScriptによる動きや機能上必要な動きは個別に代替手段を設計・検証する。共通CSSだけで対応済みとはしない。

## 検証範囲と残る確認

2026-10-24、Node v24.19.0 / npm 11.9.0 で確認した結果：

- 変更前後とも `ASTRO_TELEMETRY_DISABLED=1 npm run build` 成功（3ページ生成）。環境内でホーム配下の設定フォルダーを作れないため、テレメトリーを無効にして実行した。
- `git diff --check` 成功。独立した lint / test / check スクリプトはなく、astro check 用の追加依存は導入していない。
- Chromium / Playwrightで、ビルド済みHTMLに生成CSSをインライン展開し、全3ページ × 幅320 / 375 / 768 / 1440px（高さ900px）を比較。通常表示のスクリーンショットは変更前後でバイト単位で一致し、横方向のオーバーフローもなかった。HTTP配信での確認ではない。
- Homeのロゴ、Home / Blog / Profileの順にTab移動し、:focus-visibleの2px実線・3pxオフセットを確認。Shift+Tabで逆移動も確認した。
- reduced-motionをエミュレートし、ルートのscroll-behaviorがautoであることを確認した。現状にアニメーションはなく、将来の動きの低減まで検証したものではない。

今後の部品設計では、キーボード操作・フォーカス可視性と非遮蔽、セマンティックHTML、アクセシブルネーム、画像の代替テキスト、200%文字拡大・400%ズーム・320 CSS pxでのリフロー、文字間隔変更、動きの低減を確認する。スクリーンリーダー、強制カラー、OSごとの日本語フォールバック、実機での操作性・ターゲットサイズは別途手動確認が必要。自動テストだけでWCAG 2.2 AA準拠とは判断しない。

## 参考資料

いずれも考え方の参照であり、パッケージや数値をそのまま導入しない。

- [デジタル庁デザインシステム](https://design.digital.go.jp/dads/)：設計とアクセシビリティの確認観点。
- [Radix Colors](https://www.radix-ui.com/colors/docs/palette-composition/understanding-the-scale)：背景・文字・装飾線・UI境界の用途の区別。
- [Open Props](https://open-props.style/)：サイズ・余白スケールの検討用。既存の余白は維持する。
- [Utopia](https://utopia.fyi/)：流動的サイズの検討用。既存の clamp() を維持する。
- [WCAG 2.2](https://www.w3.org/TR/WCAG22/)：特に1.4.3、1.4.10、1.4.11、2.4.7、2.4.11。
- [Astro: Styles and CSS](https://docs.astro.build/en/guides/styling/)：通常CSSの読み込み方法。
