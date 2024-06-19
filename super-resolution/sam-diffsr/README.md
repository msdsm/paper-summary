# SAM-DiffSR : Structure-Modulated Diffusion Model for Image Super-Resolution

## ソース
- https://arxiv.org/pdf/2402.17133
- https://github.com/lose4578/SAM-DiffSR
## 概要
- SAM(Segment Anythin Model)をdiffusionに取り入れてSuper Resolution
- 低画質画像のsegmentation maskを学習時(拡散過程)のノイズに加える

## 提案手法
- $\boldsymbol{x}_0$をSAMに入力して得られるsegmentation maskを$\boldsymbol{M}_{\mathrm{SAM}}$とする
- $\boldsymbol{M}_{\mathrm{SAM}}$に対してencoder, embeddingを加えたものを$E_{\mathrm{SAM}}$とする
### 拡散過程
拡散過程を以下のように定義する
$$
\begin{align}
q\left(\boldsymbol{x}_t \mid \boldsymbol{x}_{t-1}, \boldsymbol{E}_{\mathrm{SAM}}\right) = \mathcal{N}\left(\boldsymbol{x}_t; \sqrt{1-\beta_t}\boldsymbol{x}_{t-1} + \sqrt{\beta}_t\boldsymbol{E}_{\mathrm{SAM}}, \beta_t \boldsymbol{I}\right)
\end{align}
$$
このとき、$q\left(\boldsymbol{x}_t \mid \boldsymbol{x}_0, \boldsymbol{E}_{\mathrm{SAM}}\right)$を計算すると
$$
\begin{align}
q\left(\boldsymbol{x}_t \mid \boldsymbol{x}_0, \boldsymbol{E}_{\mathrm{SAM}}\right) &= \mathcal{N}\left(\boldsymbol{x}_t; \sqrt{\bar{\alpha}_t}\boldsymbol{x}_0 + \phi_t \boldsymbol{E}_{\mathrm{SAM}}, \left(1-\bar{\alpha}_t\right)\boldsymbol{I}\right) \\
\alpha_t &= \beta_t \\
\bar{\alpha}_t &= \prod_{i=1}^t \alpha_i \\
\phi_t &= \sum_{i=1}^{t}{\sqrt{\bar{\alpha}_t\frac{\beta_i}{\bar{\alpha}_i}}}
\end{align}
$$

### 生成過程