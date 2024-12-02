# Diffusion Models, Image Super-Resolution And Everything: A Survey

## ソース
- https://arxiv.org/pdf/2401.00736

## 概要
- Diffusion image super resolutionのsurvey論文

## 論文リスト
- https://arxiv.org/abs/2209.13131 : 別のSR survey論文
- diffusion
  - DDPM
  - SGM
  - SDE

## SR
- SISR(Simple Image Super Resolution)とMISR(Multi Image Super Resolution)に分類される
- SISRは1枚のLRから1枚のHR
- MISRは複数のLRから1枚のHRあるいは複数のLRから複数のHR

## SISR
- LR image $x \in \mathbb{R}^{w \times h \times c}$, HR image $y \in \mathbb{R}^{W \times H \times c} \ h < H, \ w < W$は以下の関係
$$
\mathbf{x} = \mathcal{D}\left(\mathbf{y}; \mathbf{\Theta}\right) = \left(\left(\mathbf{y}\otimes \mathbf{k}\right)\downarrow_s + \mathbf{n}\right)_{JPEG_q}
$$
- $\mathcal{D}$はdegradatioin mapで$\mathcal{R}^{w \times h \times c} \to \mathcal{R}^{W \times H \times c}$
  - $\Theta$はdegradation parametersで$\mathbf{k}, \mathbf{n}, s$を含む
  - それぞれ順にblur, noise, scalingを意味する
- このときSISRは以下を解くタスク
$$
\theta^* = \argmin_{\theta}\mathcal{L}\left(f_{\theta}\left(x \right), y\right) + \lambda\phi\left(\theta\right)
$$
- $\phi$は正則化項
## 英単語
- articulate : 明確に言う
- cohesive : 密着する
