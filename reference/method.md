# 論文精読メソッド — 英語圏の定番手法の統合版

このファイルは、英語圏で広く参照されている 4 つの論文読解メソッドを調査し、
ロボティクス／Physical AI 論文の精読用に統合したものです。

---

## 参照した原典（一次資料）

| # | 手法 | 著者 | 出典 |
|---|------|------|------|
| A | **The Three-Pass Approach** | S. Keshav (Univ. of Waterloo) | *How to Read a Paper*, ACM SIGCOMM CCR 37(3), 2007 |
| B | **The Three-Question Method** | Andrew Ng (Stanford) | Stanford CS230 Lecture, "Career Advice / Reading Research Papers" |
| C | **Bottom-Up Close Reading** | Jennifer Raff (Univ. of Kansas) | *How to read and understand a scientific paper: a guide for non-scientists*, LSE Impact Blog, 2016 |
| D | **Literature Survey Loop** | S. Keshav | 同上 §3 |

---

## A. Keshav の 3 パス法（骨格として採用）

> "The key idea is that you should read the paper in **up to three passes**, instead of
> starting at the beginning and plowing your way to the end."

### 1st pass（5〜10分）— 鳥瞰
1. Title / Abstract / Introduction を丁寧に読む
2. 節・小節の見出しだけを読む（本文は無視）
3. Conclusion を読む
4. References をざっと眺め、既読のものにチェックを入れる

終了時に **5 つの C** に答えられること：

| C | 問い |
|---|------|
| **Category** | どういう種類の論文か？ 計測論文？ 既存システムの分析？ 研究プロトタイプの記述？ |
| **Context** | どの論文群と関係するか？ どの理論的基盤を使っているか？ |
| **Correctness** | 前提は妥当に見えるか？ |
| **Contributions** | 主要な貢献は何か？ |
| **Clarity** | 論文としてよく書けているか？ |

### 2nd pass（最大1時間）— 内容把握（証明は飛ばす）
1. **図・グラフ・表を注意深く見る**。軸にラベルはあるか？ エラーバーは付いているか？
   → Keshav 曰く「こういう凡ミスが、雑な仕事と本当に優れた仕事を分ける」
2. 未読の関連文献に印を付ける

終了時：「論文の主眼を、根拠とともに他人に要約して説明できる」状態。

### 3rd pass（初心者で4〜5時間）— 完全理解
> "The key to the third pass is to attempt to **virtually re-implement** the paper."

- 著者と同じ前提を置いて、頭の中で研究を再現する
- **すべての文のすべての前提を特定し、疑う**
- 自分ならこのアイデアをどう提示するかを考え、実際の論文と比較する
- 将来の仕事のアイデアを書き留める

終了時：論文の構造を記憶から再構成でき、
**暗黙の前提・欠落した引用・実験/解析手法の潜在的な問題点**を指摘できる。

---

## B. Andrew Ng の 4 つの問い（3rd pass の出力チェックリストとして採用）

論文を読み終えたとき、次の 4 問に答えられること：

1. **What did the authors try to accomplish?** — 著者は何を達成しようとしたか
2. **What were the key elements of the approach?** — 手法の鍵となる要素は何か
3. **What can you use yourself?** — 自分が使える部分はどこか
4. **What other references do you want to follow?** — 次に追うべき参考文献はどれか

読む順序についての Ng の助言：
- **Title → Abstract → Figures** から入る（図を先に見る）
- 次に Introduction → Conclusion → 主要な図 → 残り（Related Work は飛ばしてよい）
- **数式は最初は飛ばす。意味の分からない箇所も飛ばす**
  — 「優れた研究とは知識の境界で発表されるものだから、分からない部分があって当然」
- 分野の基礎を掴むには 5〜10 本、専門家になるには 50〜100 本

---

## C. Jennifer Raff のボトムアップ精読（用語・図の扱いとして採用）

1. **Abstract から読み始めない。Introduction から読む。**
   — Abstract だけで論を組み立てるのは最悪の慣習
2. **分からない単語をすべて書き出して調べる**（語彙が分からなければ論文は分からない）
3. **実験を図解する** — 著者が実際に何をしたのかを、自分で図に描く
4. **結果を要約する** — 各実験・各図・各表について段落を書く
5. **著者の解釈を評価する** — 著者は結果が何を意味すると考えているか。自分は同意するか。
   **別の解釈は可能か**
6. **Abstract は最後に読む** — 論文本体の記述と一致しているか、自分の解釈と合うかを検証
7. **他の専門家の評価を調べる** — この論文への批判・支持を確認

---

## D. 統合ワークフロー（本スキルが実行する 7 ステップ）

Keshav の 3 パスを骨格に、Ng のチェックリストと Raff の精読テクニックを差し込む。

```
STEP 0  メタ情報の確定      … 書誌・受賞・コード/プロジェクトページの有無
STEP 1  1st pass → 5 Cs     … 鳥瞰。Category / Context / Correctness / Contributions / Clarity
STEP 2  用語の棚卸し (Raff②) … 専門用語を全部拾い、EN→JA 対訳＋解説を作る
STEP 3  2nd pass → 手法解剖  … 図・表を1つずつ読み解く。Raff③④で実験を再構成
STEP 4  3rd pass → 仮想再実装 … 数式導出・ハイパラ・ハード構成。再現に必要な条件を洗い出す
STEP 5  批判的評価 (Raff⑤)   … 全前提を疑う。別解釈・弱点・欠落引用
STEP 6  Ng の4問に回答       … 特に「自分が使える部分」を具体化
STEP 7  Abstract 再読 (Raff⑥) … 本文と齟齬がないか検算し、TL;DR を確定
```

---

## E. ロボティクス／Physical AI 論文に固有の読みどころ

汎用メソッドに加え、この分野では以下を必ず検査する。

### E-1. Sim-to-Real ギャップ
- シミュレータは何か（Isaac Gym / MuJoCo / SAPIEN / Genesis…）
- **domain randomization** の対象と範囲は明記されているか
- 「シミュレーションのみ」の結果と「実機」の結果が混在していないか
- 実機評価の**試行回数**は書いてあるか（n=10 と n=100 では意味が違う）

### E-2. 評価の誠実さ
- 成功率(success rate)の **成功判定基準**が定義されているか
- ベースラインは同じ条件（同じデータ量・同じ計算量）で走らせているか
- タスクは著者が自作したものか、既存ベンチマークか
- **cherry-picking**：デモ動画のタスクと定量評価のタスクは一致しているか

### E-3. ハードウェア依存性
- 特定のロボット（Unitree G1 / Franka / ALOHA…）に固有の仮定はどこか
- センサ構成（force sensor の有無、カメラ台数、触覚）
- 制御周波数、アクチュエータ帯域

### E-4. データ
- デモンストレーション数、収集時間、収集者の人数
- データセットは公開されているか
- **学習データと評価環境の重なり**（同じ部屋？ 同じ物体？）

### E-5. 再現可能性
- コード公開の有無とライセンス
- 学習に要した計算資源（GPU 時間）
- 乱数シード数、分散（平均だけ報告して分散を隠していないか）

---

## F. 「読まない」判断も成果である

Keshav の指摘どおり、1st pass の後に読むのをやめてよい。やめる正当な理由：

- 自分の関心と違う
- 前提知識が足りず理解できない（→ 先に背景文献を読む）
- **著者の前提が明らかに無効**

本スキルは、1st pass の 5 Cs の Correctness で赤信号が出た場合、
その旨を出力に明記した上で精読を続行する（読者が判断できるようにするため）。

---

## 出典リンク

- Keshav, *How to Read a Paper* — https://web.stanford.edu/class/ee384m/Handouts/HowtoReadPaper.pdf
- ACM DL 版 — https://dl.acm.org/doi/10.1145/1273445.1273458
- Andrew Ng の助言まとめ — https://www.kdnuggets.com/2019/09/advice-building-machine-learning-career-research-papers-andrew-ng.html
- Jennifer Raff, LSE Impact Blog — https://blogs.lse.ac.uk/impactofsocialsciences/2016/05/09/how-to-read-and-understand-a-scientific-paper-a-guide-for-non-scientists/
