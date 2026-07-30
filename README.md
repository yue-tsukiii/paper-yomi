# paper-yomi 📄

**論文を渡すだけで、プロ級の精読ノートが完成する Claude Skill。**

arXiv URL、PDF、タイトルのいずれかを渡すと、3 つの学術的精読メソッドを統合した**英日対訳 HTML ノート**を自動生成します。数式はオフライン SVG に変換済み。ネット環境がなくても崩れません。

<p align="center">
  <img src="docs/screenshots/sample-hero.png" alt="出力サンプル：ヘッダーとTL;DR" width="720" />
</p>

---

## 特長

🔬 **3 メソッド統合の深い精読**
Keshav「3 パス法」、Andrew Ng「4 つの問い」、Jennifer Raff 精読法を 1 つのワークフローに統合。表面的な要約ではなく、数式の導出、ハイパーパラメータ、再現条件まで踏み込む 3rd pass 相当の分析。

🌏 **英日対訳 2 カラム**
重要箇所の英語原文と日本語解説を並べて表示。専門用語は英語 + 日本語訳 + 解説の対訳表付き。「この単語、原文ではどう書いてあったっけ？」がすぐに確認できます。

⚖️ **批判的評価が標準装備**
褒めるだけのノートは生成しません。弱点・限界を最低 3 つ、具体的な根拠とともに指摘。Sim-to-Real ギャップ、評価の誠実さ、再現可能性など Physical AI 固有のチェックリストも自動適用します。

📐 **数式はオフライン SVG**
LaTeX 数式を MathJax でビルド時に SVG 変換。CDN 依存ゼロ。記号の定義も必ず添えるため、数式だけ見て意味が取れます。

📦 **1 論文 = 1 ファイルで完結**
出力は自己完結型の HTML ファイル。外部依存なし。ローカルに保存してそのまま読めます。複数論文をまとめて依頼すれば、横断比較表付き `index.html` も生成。

---

## サンプル出力

> 架空の論文を使ったフォーマット確認用デモです。実在の論文の解析結果ではありません。

| ヘッダー・TL;DR・対訳ペア | 数式ブロック・批判的評価 |
|:---:|:---:|
| ![サンプル：ヘッダー](docs/screenshots/sample-hero.png) | ![サンプル：数式](docs/screenshots/sample-math.png) |

➡️ **[サンプル全文を見る（HTML）](https://htmlpreview.github.io/?https://github.com/yue-tsukiii/paper-yomi/blob/main/docs/sample.html)**

---

## 使い方

Claude Code または Skills 対応の Claude にインストールした状態で、次のように話しかけるだけです。

```
この論文を解析して：https://arxiv.org/abs/2503.xxxxx
```

```
この PDF を日本語で精読ノートにして
```

```
"RMA: Rapid Motor Adaptation for Legged Robots" を精読ノートにして
```

出力形式（Markdown / PDF）や分析の深さ（1st pass のみ、など）を変えたい場合は、そのまま伝えてください。

---

## インストール

```bash
git clone https://github.com/yue-tsukiii/paper-yomi.git ~/.claude/skills/paper-yomi
```

プロジェクト単位で使う場合は、`.claude/skills/` 配下に配置してください。

### 数式レンダリングの準備（初回のみ）

```bash
mkdir -p /tmp/mjx && cd /tmp/mjx && npm init -y
npm install mathjax-full@3.2.2
```

---

## 精読メソッド

3 つの確立された論文精読法を段階的に適用します。

| メソッド | 役割 |
|---|---|
| **Keshav の 3 パス法** | 1st pass（鳥瞰・5 つの C）→ 2nd pass（手法の解剖）→ 3rd pass（仮想再実装） |
| **Andrew Ng の 4 つの問い** | 達成目標、鍵となる要素、自分が使える部分、次に読むべき文献 |
| **Jennifer Raff の精読法** | 用語の棚卸し、実験の再構成、全前提を疑う批判的読解、Abstract との照合 |

ロボティクス・Physical AI 分野では、Sim-to-Real ギャップ、ハードウェア依存性、データの再現可能性を追加チェックします。

---

## ノートに含まれるもの

- 英日対訳ペア（重要箇所のみ抽出）
- 専門用語の対訳表（英語 + 日本語訳 + 解説）
- 全図表の読み解き
- 仮想再実装メモ（数式、ハイパーパラメータ、ハードウェア構成）
- Physical AI 固有の批判チェックリスト
- 参考文献リンク集（次に読むべき論文 3-5 本、理由付き）

---

## リポジトリ構成

```
paper-yomi/
├── SKILL.md                   # スキル本体
├── reference/
│   ├── method.md              # 統合メソッド全文
│   └── glossary.md            # ロボティクス用語 EN→JA 対訳辞書
├── assets/
│   └── template.html          # 出力 HTML テンプレート
├── scripts/
│   └── render_math.mjs        # LaTeX → SVG 変換スクリプト
└── docs/
    ├── sample.html            # サンプル出力
    └── screenshots/           # README 用スクリーンショット
```

---

## 品質チェック

生成後、以下を自動検証してから出力します。

- 未置換プレースホルダ（`{{...}}`）の残存
- 生の LaTeX（`$$`）の残存（数式レンダリング漏れ）
- CDN 依存の混入（オフライン動作を保証）
- HTML タグの閉じ忘れ
- 数値の原文照合（節番号・表番号の正確性）
- 弱点の指摘が 3 つ以上あるか

---

## ライセンス

MIT
