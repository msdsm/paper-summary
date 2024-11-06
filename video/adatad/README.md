# End-to-End Temporal Action Detection with 1B Parameters Across 1000 Frames
- CVPR2024
- AdaTAD
- Adapter tuning for temporal action detection

## ソース
- https://arxiv.org/pdf/2311.17241v2
- https://github.com/sming256/AdaTAD


## 概要
- TAD(Temporal Action Detection)タスクに対してTIA(Temporal Informative Adapter)を提案

## TAD
- Temporal Action Detectionというタスク
- Temporal Action Localizatinoともいう
- 定式化すると以下
- 動画を$\boldsymbol{X} \in \mathbb{R}^{3 \times H \times W \times T}$
  - $H,W,T$はそれぞれ高さ、幅、フレーム数
- temporal action annotationsは以下のような集合で表現される

$$
\Psi_g = \{ \varphi_i=\left(t_s, t_e, c\right)^N_{i=1}\}
$$
- $t_s,t_e$は開始時刻、終了時刻
- $c$はcategory of action
- $N$はnumber of ground truth actions
- このとき、TADは以下を予測するタスク
$$
\Psi_p = \{ \hat{\varphi}_i=\left(\hat{t}_s, \hat{t}_e, \hat{c}, s\right)^M_{i=1}\}
$$
- $s$はconfidence score