# ロボティクス／Physical AI 用語 EN→JA 対訳辞書

出力 HTML の用語集（Glossary）を書くときの**訳語の統一基準**。
迷ったらこの表に従う。表にない語は「英語をそのまま残し、初出時に日本語で説明を付す」。

## 訳出の三原則

1. **定訳がある語は定訳を使う** — 例: reinforcement learning → 強化学習
2. **定訳が無い/揺れている語は英語のまま残し、丸括弧で説明を添える**
   例: `affordance（物体が行為者に提供する行為可能性）`
3. **手法名・モデル名・データセット名は絶対に訳さない** — `Diffusion Policy` を「拡散方策」と書かない

## 表記ルール

- 初出: `英語 Term（日本語訳）` → 以降は日本語訳のみ、または英語のみで統一
- 略語は初出で必ず展開: `VLA（Vision-Language-Action、視覚-言語-行動）モデル`
- 単位・数値は原文のまま（`50 Hz`, `0.5 m/s`, `39.5%`）

---

## 1. 学習・制御の基礎

| English | 日本語 | メモ |
|---|---|---|
| reinforcement learning (RL) | 強化学習 | |
| imitation learning (IL) | 模倣学習 | |
| behavior cloning (BC) | 行動クローニング | 「模倣」と混同しない |
| policy | 方策（ポリシー） | ロボット文脈では「ポリシー」が通りやすい |
| reward shaping | 報酬設計／報酬整形 | |
| on-policy / off-policy | 方策オン型／方策オフ型 | PPO は on-policy |
| actor-critic | Actor-Critic | 訳さない |
| rollout | ロールアウト | 訳さない |
| horizon | ホライズン（予測地平） | long-horizon → 長期ホライズン |
| teacher-student distillation | 教師-生徒蒸留 | privileged learning とセット |
| privileged information | 特権情報 | シミュレータのみで得られる真値 |
| curriculum learning | カリキュラム学習 | |
| domain randomization | ドメインランダム化 | 訳さないことも多い |
| sim-to-real transfer | Sim-to-Real 転移 | 「実機転移」 |
| zero-shot / few-shot | ゼロショット／フューショット | |
| generalization | 汎化 | |
| out-of-distribution (OOD) | 分布外 | |

## 2. 制御・力学

| English | 日本語 | メモ |
|---|---|---|
| loco-manipulation | ロコマニピュレーション | 移動(locomotion)+操作(manipulation) の合成語。訳さない |
| locomotion | 移動運動／歩行制御 | |
| manipulation | 操作／マニピュレーション | |
| whole-body control (WBC) | 全身制御 | |
| compliance / compliant control | コンプライアンス制御／柔軟制御 | 「受動的に力に従う」制御 |
| impedance control | インピーダンス制御 | |
| admittance control | アドミッタンス制御 | インピーダンスの逆写像 |
| force control | 力制御 | |
| position control | 位置制御 | |
| hybrid force/position control | ハイブリッド力/位置制御 | |
| torque | トルク | |
| end-effector (EE) | エンドエフェクタ | 手先 |
| joint / actuator | 関節／アクチュエータ | |
| PD gain | PD ゲイン | |
| proprioception | 固有受容感覚 | 関節角・IMU 等、自己状態の知覚 |
| exteroception | 外受容感覚 | カメラ・LiDAR 等、外界の知覚 |
| contact-rich | 接触が支配的な／接触リッチな | contact-rich manipulation |
| quasi-static | 準静的 | |
| kinematics / dynamics | 運動学／動力学 | |
| inverse kinematics (IK) | 逆運動学 | |
| trajectory optimization | 軌道最適化 | |
| model predictive control (MPC) | モデル予測制御 | |
| centroid / CoM | 重心 | Center of Mass |
| ZMP | ZMP（ゼロモーメントポイント） | |
| gait | 歩容 | |
| retargeting | リターゲティング | 人の動きをロボットの体格に写す |

## 3. 知覚・表現

| English | 日本語 | メモ |
|---|---|---|
| point cloud | 点群 | |
| occupancy grid | 占有格子 | |
| heightmap / elevation map | 高さマップ／標高マップ | |
| SLAM | SLAM | 訳さない |
| monocular | 単眼 | |
| depth estimation | 深度推定 | |
| 6-DoF pose | 6自由度姿勢 | DoF = Degrees of Freedom |
| mesh reconstruction | メッシュ再構成 | |
| Gaussian Splatting | Gaussian Splatting | 訳さない |
| NeRF | NeRF | 訳さない |
| affordance | アフォーダンス | 「操作可能性」と説明を添える |
| segmentation | セグメンテーション（領域分割） | |
| keypoint | キーポイント | |
| SMPL / SMPL-X | SMPL / SMPL-X | 人体パラメトリックモデル。訳さない |

## 4. 基盤モデル・生成モデル

| English | 日本語 | メモ |
|---|---|---|
| foundation model | 基盤モデル | |
| vision-language model (VLM) | 視覚言語モデル | |
| vision-language-action (VLA) | VLA（視覚-言語-行動）モデル | 略語のまま使うのが主流 |
| diffusion model | 拡散モデル | |
| flow matching | Flow Matching | 訳さない |
| action chunk | アクションチャンク | 複数ステップ分の行動をまとめて出力 |
| tokenizer | トークナイザ | |
| autoregressive | 自己回帰 | |
| co-training | 共同学習（co-training） | 異種データを混ぜて同時学習 |
| high-level / low-level policy | 上位方策／下位方策 | 階層方策 |
| chain-of-thought | 思考の連鎖（Chain-of-Thought） | |
| latent space | 潜在空間 | |
| fine-tuning | ファインチューニング | |
| post-training | ポストトレーニング | |

## 5. システム・評価

| English | 日本語 | メモ |
|---|---|---|
| teleoperation | 遠隔操作（テレオペ） | |
| demonstration | デモンストレーション（実演データ） | |
| success rate | 成功率 | |
| ablation study | アブレーション（除去実験） | |
| baseline | ベースライン | |
| benchmark | ベンチマーク | |
| throughput | スループット | |
| latency | レイテンシ（遅延） | |
| fixture | 治具（フィクスチャ） | 組立で部品を固定する道具 |
| precedence constraint | 先行制約 | 組立順序の制約 |
| assembly sequence planning | 組立順序計画 | |
| motion planning | 動作計画 | |
| grasp planning | 把持計画 | |
| collision-free | 衝突回避された | |
| caregiver | 介護者／介助者 | |
| user study | ユーザスタディ | |
| in-the-wild | 実環境での（in-the-wild） | 実験室外 |
| personalization | パーソナライズ（個別適応） | |
