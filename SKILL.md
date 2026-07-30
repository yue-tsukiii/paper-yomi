---
name: paper-yomi
description: 英語の論文（特にロボティクス／Physical AI／機械学習）を日本語で精読解析し、英日対訳2カラムのHTMLノートを生成する。Keshavの3パス法＋Andrew Ngの4つの問い＋Jennifer Raffの精読法を統合したメソッドで、専門用語は英語＋日本語解説を併記する。arXiv URL・PDFファイル・論文タイトルのいずれからでも起動。「この論文を解析して」「論文を日本語でまとめて」「精読ノートを作って」等で使う。
---

# paper-yomi — 英語論文の日本語精読スキル

英語論文を **Keshav の 3 パス法** で読み解き、**英日対訳 2 カラムの HTML 精読ノート**を出力する。
対象は主にロボティクス／Physical AI／機械学習。

## 出力の約束（デフォルト）

| 項目 | 既定値 |
|---|---|
| 出力形式 | 対訳 2 カラム HTML（1 論文 = 1 ファイル、自己完結） |
| 深さ | **3rd pass 相当の精読**（数式の導出・ハイパラ・再現条件まで） |
| 本文言語 | 日本語 |
| 英語原文 | **重要箇所のみ引用**（Abstract 全文＋各節のキー文）。全文対訳はしない |
| 専門用語 | 英語 + 日本語訳 + 解説を Glossary 表に。本文初出も `英語（日本語）` |

ユーザが別の形式（Markdown / docx / PDF）や別の深さ（1st/2nd pass 相当）を指定したら、
それに従う。指定がなければ上表のとおり。

---

## 手順

### STEP 0 — 論文を取得する

- **arXiv URL / ID を渡された場合**: `WebFetch` で `https://arxiv.org/pdf/<ID>` を取得。
  本文が長いと結果はファイルに保存されるので、`Read` で **先頭から順に全チャンク読む**。
  途中で止めない。読めなかった範囲があれば出力の footer に明記する。
- **ローカル PDF の場合**: `pdf` スキル、または `pdftotext -layout` でテキスト化。
- **タイトルだけ渡された場合**: `WebSearch` で arXiv ID を特定してから取得。
- 併せて **プロジェクトページ・GitHub・OpenReview** を探す。
  OpenReview のレビューは STEP 5（批判的評価）と Raff ⑦（他の専門家の評価）の一次資料になる。

> Sandbox の bash から `arxiv.org` へ直接 curl するのは proxy で 403 になる。必ず `WebFetch` を使う。

### STEP 1 — 1st pass → 5 つの C

Title / Abstract / Introduction / 節見出し / Conclusion / References **だけ**を見て、
`reference/method.md` §A の 5 つの C に答える。1 項目 2〜3 文。

Correctness で赤信号（前提が明らかに怪しい）が出たら、その旨を明記した上で精読を続行する。

### STEP 2 — 用語の棚卸し（Raff ②）

論文中の専門用語・略語をすべて拾い、`reference/glossary.md` の訳出三原則に従って対訳表を作る。
**手法名・モデル名・データセット名は訳さない。** 20〜35 語程度が目安。
その論文を読むのに実際に必要な語だけに絞る（一般的すぎる語は入れない）。

### STEP 3 — 2nd pass → 手法の解剖

- 図・表を **1 つずつ** 読み解く。Figure N が何を示しているかを日本語で説明する
- パイプライン全体を段階に分けて記述する
- **軸ラベル・エラーバー・試行回数**をチェックする（Keshav が明示的に要求している点）
- 各節の**キー文だけ**英語原文で引用し、`.pair` の 2 カラムで対訳を付ける

### STEP 4 — 3rd pass → 仮想再実装

「自分がゼロから実装するなら何が必要か」を書き出す：

- **数式**：主要な式を LaTeX で `.math` ブロックに書き、記号の定義（`.where`）を必ず添える
- **ネットワーク構成**：入力次元・出力次元・層数・活性化
- **学習設定**：アルゴリズム、環境数、学習ステップ数、報酬項とその重み、GPU 時間
- **ハードウェア**：ロボット機種、センサ構成、制御周波数
- **論文に書かれていない情報**は「論文に記載なし」と明記する。これが再現性の評価そのもの

### STEP 5 — 批判的評価（Raff ⑤ + Keshav 3rd pass）

`reference/method.md` §E の Physical AI 固有チェックリストを全項目走査する：

- E-1 Sim-to-Real ギャップ / E-2 評価の誠実さ / E-3 ハードウェア依存性
- E-4 データ / E-5 再現可能性

さらに：暗黙の前提、結果の**別解釈**、欠落した引用、著者自身が認めている限界。
**褒めるだけのノートは失敗**。最低 3 つは具体的な弱点を挙げる。

### STEP 6 — Andrew Ng の 4 つの問い

1. 著者は何を達成しようとしたか
2. 手法の鍵となる要素は何か
3. **自分が使える部分はどこか** ← ここを最も具体的に書く
4. 次に追うべき参考文献はどれか（3〜5 本、なぜ読むかを添えて）

### STEP 7 — Abstract 再読・検算（Raff ⑥）

Abstract の主張と本文の内容が一致しているか照合する。
食い違い（Abstract が過大に主張している等）があれば批判的評価に追記。
最後に TL;DR を 2〜3 文で確定する。

### STEP 8 — 図とリンクを集める

論文の図は**著者が公開しているものにリンク参照**する（再配布しない）。取得先の優先順：

1. **arXiv の HTML 版**：`https://arxiv.org/html/<ID>` を `WebFetch` して画像 URL を探す
   - LaTeX 由来の図：`https://arxiv.org/html/<ID>/x1.png`, `x2.png`, …
   - 埋め込み画像：`https://arxiv.org/html/<ID>/extracted/<num>/figures/.../name.png`
   - **HTML 版が無い論文もある**（PDF から変換されたテキストに `_page_0_Picture_3.jpeg` のような
     ダミーパスが出たら HTML 版は存在しない）。その場合は 2 か 3 へ
2. **プロジェクトページ**：`WebFetch` して `<img>` の URL を拾う
3. **GitHub リポジトリ**：`raw.githubusercontent.com/<user>/<repo>/main/images/teaser.gif` など。
   README を `WebFetch` して確認する

**リンク集を必ず作る。** `WebSearch` で `github <手法名> <著者名> code` を検索し、
公式リポジトリ・プロジェクトページ・OpenReview・PMLR 予稿・動画を集めて
`.repo` ブロックに並べる。**見つからなかった場合は「公開が確認できませんでした」と明記する**
（無いことも読者にとって重要な情報）。

```html
<div class="repo">
  <a href="..."><span class="ic">📄</span> arXiv:XXXX.XXXXX <span class="sub">論文</span></a>
  <a href="..."><span class="ic">💻</span> github.com/user/repo <span class="sub">コード（ライセンス）</span></a>
  <a href="..."><span class="ic">🌐</span> project.page <span class="sub">プロジェクトページ・動画</span></a>
</div>
```

図は `<figure class="pf">` で入れ、**キャプションを日本語で書き直す**（原文の直訳ではなく、
その図から何を読み取るべきかを書く）。出典は必ず明記する。

### STEP 9 — HTML を生成してビルドする

`assets/template.html` の `{{...}}` を埋める。

**必ず数値を原文と照合してから書く。** 成功率・改善幅・試行回数は捏造しやすいので、
本文に出てくる数字だけを使い、出典の節/表番号を添える。

生成後、**必ず数式レンダリングを走らせる**（前掲の `scripts/render_math.mjs`）。
これを飛ばすと生の LaTeX が表示されて読めない。

---

## HTML の書き方

### 対訳 2 カラム（このスキルの中核）

```html
<div class="pair q">
  <div class="en"><span class="lbl">Original (EN)</span>
    We propose the first unified policy for legged robots that jointly models
    force and position control learned without relying on force sensors.
    <span class="cite">— Abstract</span>
  </div>
  <div class="ja"><span class="lbl">日本語訳</span>
    力センサに依存せずに、力制御と位置制御を同時にモデル化する、脚型ロボット向けの
    初の統合方策を提案する。
    <span class="cite">— Abstract</span>
  </div>
</div>
```

`.pair` は左に英語、右に日本語。`q` クラスを付けると英語側がセリフ体になる（直接引用用）。
860px 以下では自動で縦積みになる。

**引用の粒度**：1 引用 = 1〜3 文。段落まるごとは引用しない。
「重要箇所のみ」という設計なので、1 論文あたり **5〜10 個** の `.pair` が適量。

### 数式 — LaTeX で書き、**ビルド時に SVG へ焼き込む**

数式は必ず LaTeX で書く。`$...$`（インライン）と `$$...$$`（ディスプレイ）を使う。

```html
<div class="math">
  <span class="tag">式 (3) — 目的関数</span>
  $$\mathcal{L} = \mathbb{E}\left[\sum_t \gamma^t r_t\right] - \lambda\,\mathrm{KL}\!\left(\pi_\theta \,\|\, \pi_{ref}\right)$$
  <span class="where"><b>記号</b>：$\gamma$ は割引率、$r_t$ は時刻 $t$ の報酬、
  $\lambda$ は正則化係数、$\pi_{ref}$ は参照方策。</span>
</div>
```

**記号定義のない数式は書かない。** `.where` に必ず記号の意味を添える。

#### 重要：CDN の MathJax を使ってはいけない

`<script src="https://cdn.jsdelivr.net/...mathjax...">` を埋め込む方式は、
**ユーザがローカルで HTML を開いたときにネットワークが無ければ数式が生の LaTeX のまま表示される**。
これは実際に起きる。必ず以下の手順で**ビルド時に SVG へ変換**し、
出力を完全にオフラインで完結させること。

```bash
# 初回のみ
mkdir -p /tmp/mjx && cd /tmp/mjx && npm init -y >/dev/null
npm install mathjax-full@3.2.2 --no-audit --no-fund

# レンダリング（$...$ と $$...$$ をインライン SVG に置換、ファイルを上書き）
cp <このスキルのパス>/scripts/render_math.mjs /tmp/mjx/
node /tmp/mjx/render_math.mjs /path/to/*.html
```

`scripts/render_math.mjs` は以下を行う：

- `<script>` / `<style>` / `<pre>` / `<code>` の中は**保護して触らない**
- `$$...$$` → `<span class="mjx-d">…SVG…</span>`（ディスプレイ）
- `$...$` → `<span class="mjx-i">…SVG…</span>`（インライン）
- `fontCache: 'none'` なので各 SVG が**単独で完結**する（フォント不要）
- SVG は `fill="currentColor"` を使うので**ダークモードで自動的に色が反転する**

レンダリング後、CDN の `<script>` タグを削除し、次の CSS を `<style>` に追加する：

```css
.mjx-d{display:block;text-align:center;margin:.65em 0;overflow-x:auto;
       overflow-y:hidden;max-width:100%;padding:2px 0}
.mjx-d svg{max-width:100%;height:auto;color:var(--ink)}
.mjx-i svg{vertical-align:-0.25ex;color:inherit}
td .mjx-d{margin:.2em 0;text-align:left}
```

#### コードや擬似コードは数式にしない

PDDL・アルゴリズム・設定ファイルなどは `<pre>` に入れ、
親要素に `class="math no-mathjax"` を付けて数式変換の対象外にする。

```html
<div class="math no-mathjax">
  <span class="tag">PDDL 演算子</span>
<pre style="margin:0;font-family:var(--mono);font-size:13px;line-height:1.9">(:action PickTool
  :parameters (?tool - tool)
  ...
)</pre>
</div>
```

### 注釈ブロック

| クラス | 用途 |
|---|---|
| `.note.fig` | 図・表の読み解き |
| `.note.good` | 評価できる点 |
| `.note.warn` | 注意点・限界 |
| `.note.crit` | 明確な問題点 |

### 用語表の行

```html
<tr><td><span class="term-en">loco-manipulation</span></td>
    <td>ロコマニピュレーション</td>
    <td>移動(locomotion)と操作(manipulation)の合成語。歩きながら物を操る能力を指す。定訳がないため英語のまま使う。</td></tr>
```

---

## 複数論文をまとめて処理する場合

1. 論文ごとに独立した HTML を 1 ファイルずつ作る（`<slug>.html`）
2. 全部終わったら **index.html** を作り、カード一覧＋横断比較表を載せる
   - 比較軸：タスク領域 / ロボット / 学習手法 / 実機評価 / コード公開 / 主な弱点
3. ファイル名は `<第一著者姓 or 手法名>-<短いスラッグ>.html`（小文字ケバブケース）

---

## 品質チェック（出力前に必ず）

内容：

- [ ] 数値はすべて原文に存在するか。節/表番号を添えたか
- [ ] 手法名・モデル名を日本語に訳してしまっていないか
- [ ] 弱点を 3 つ以上、具体的に挙げたか
- [ ] 「論文に記載なし」を、書かれていない項目に正しく付けたか
- [ ] `.pair` の左右で内容が対応しているか（訳し漏れ・意訳しすぎがないか）
- [ ] 全チャンクを読んだか。読めなかった範囲を footer に明記したか

ビルド（スクリプトで機械的に確認できる）：

```bash
# 1. 未置換プレースホルダ
grep -o '{{[A-Z_]*}}' out.html

# 2. 生の LaTeX が残っていないか（$$ が残っていたらレンダリング漏れ）
grep -c '\$\$' out.html          # 0 であること

# 3. CDN 依存が残っていないか
grep -c 'cdn\.\|MathJax-script' out.html   # 0 であること

# 4. タグの閉じ忘れ
python3 -c "
from html.parser import HTMLParser
import sys
class V(HTMLParser):
    VOID={'br','img','hr','meta','link','input','source','path','use','rect','circle','line','polygon','stop'}
    def __init__(self): super().__init__(); self.st=[]
    def handle_starttag(self,t,a):
        if t not in self.VOID: self.st.append(t)
    def handle_endtag(self,t):
        if t in self.VOID: return
        if self.st and self.st[-1]==t: self.st.pop()
        else: print('MISMATCH', t)
v=V(); v.feed(open(sys.argv[1],encoding='utf-8').read())
print('unclosed:', v.st or 'none')
" out.html
```

**4 つすべてが通ってから納品する。** 特に 2 と 3 は、ユーザがオフラインで開いたときに
数式が読めるかどうかを直接決めるので飛ばさない。

---

## 参考ファイル

- `reference/method.md` — 統合メソッドの全文（Keshav / Ng / Raff の原典要約と、Physical AI 固有チェックリスト）
- `reference/glossary.md` — ロボティクス用語 EN→JA 対訳辞書と訳出三原則
- `assets/template.html` — 出力 HTML テンプレート
- `scripts/render_math.mjs` — LaTeX → インライン SVG の変換スクリプト（Node + mathjax-full）
