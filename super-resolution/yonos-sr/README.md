# You Only Need One Step: Fast Super-Resolution with Stable Diffusion via Scale Distillation
## 論文ソース
- [You Only Need One Step: Fast Super-Resolution with Stable Diffusion via Scale Distillation](https://arxiv.org/pdf/2401.17258)

## 概要
- SF(Scale Factor)が大きくなると計算速度が遅くなる問題に取り組んだ
- Scale Distillationという手法を提案
- これは教師モデルの出力を正解とする蒸留の手法
- 生徒モデルのタスクよりもSFが小さいタスクの教師モデルを利用する
  - SFを$N$とする(縦横それぞれ$N$倍にする超解像タスク)
  - このとき、$\frac{N}{2}$倍のタスクで学習済みのモデルをもってきて教師とする
  - 正解画像に対して、解像度を$\frac{1}{N}$, $\frac{2}{N}$倍に落としたものを用意
  - 前者の画像を生徒モデルに入力して、後者の画像を教師モデルに入力
  - この出力結果の誤差をとって生徒を学習


## 前提
### 条件付き拡散モデルによるSR

## 提案手法


## 英単語メモ
- drastically : 劇的に
- exacerbate : 悪化させる
- prohibitive : 法外な
- alleviate : 軽減する、緩和する
- catastrophic : 壊滅的な
- differentiating : 区別する
- peculiarity : 特殊性
- dubbed : 
- progressive : 進歩的な
- supervisory
- threehold
- degradation
- shortcoming : 欠点、短所
- tandem