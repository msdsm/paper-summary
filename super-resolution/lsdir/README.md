# LSDIR: A Large Scale Dataset for Image Restoration


## ソース
- https://openaccess.thecvf.com/content/CVPR2023W/NTIRE/papers/Li_LSDIR_A_Large_Scale_Dataset_for_Image_Restoration_CVPRW_2023_paper.pdf



## 概要
- NNを深くすることに注目しがちでデータセットを軽視していることを問題視
- NNが大きくなるにつれてデータセットも大きくしたほうが精度上がることをしめした
- 最近のデータセットは数千枚で少ない
- 一方でImageNetなどは大きいがSRに適していない
  - blurされた画像が含まれているなど、高品質であることが保証されていないから
- そこで以下のデータセットLSDIRを提供
  - train : 84991
  - validation : 1000
  - test : 1000

## 作成方法
- FlickrからAPIを使用して大量取得
- blur detectorとflat detectorを用いてぼかし画像と平坦領域が多い画像を除外
- 人間がlow quality, high qualityで評価(アノテーション)


## blur detector
- ラプラシアンの分散をindicatorとする
$$
\begin{align}
&\begin{split}
L\left(x, y\right) &= \frac{\partial^2I}{\partial x^2} + \frac{\partial^2 I}{\partial y^2} \\
\mathcal{S}_{blur} &= \mathrm{Var}\left(L\left(x, y\right)\right)
\end{split}\end{align} 
$$
- このスコアの上限と下限として$\mathcal{S}^h_{blur}, \mathcal{S}^l_{blur}$を設定してスコアが$[\mathcal{S}^l_{blur}, \mathcal{S}^h_{blur}]$の範囲に入っているものを選択

## Flat region detection
$$
\begin{align}
&\begin{split}
G\left(x, y\right) &= \sqrt{\left(\frac{\partial I}{\partial x}\right)^2 + \left(\frac{\partial I}{\partial y}\right)^2}\\
\mathcal{S}_{flat} &= \mathrm{Var}\left(G\left(x,y\right)\right)
\end{split}\end{align}
$$
- このスコアに上限として$\mathcal{S}_{flat}$をもうけてこの値以下のものはflat regionとして除外
- 画像を$240 \times 240$のパッチに分割してそれぞれで判定


## 英語
- degradation : 劣化
- raindrop : 雨粒
- haze : 霧


