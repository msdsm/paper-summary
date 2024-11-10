# 3D Gaussian Splatting for Real-Time Radiance Field Rendering
## 論文ソース
- [3D Gaussian Splatting for Real-Time Radiance Field Rendering](https://arxiv.org/pdf/2308.04079)


## qiita
- 論文読むよりこの2つ読んだ方がよさそう
  - https://qiita.com/scomup/items/d5790da25a846e645de1
  - https://qiita.com/scomup/items/92716342a3ef0b915e0c

## 概要
- 以下の3点
  - 3D Gaussianを導入
  - 3D Gaussianのpropertiesの最適化とdensity controlを組み合わせた
  - 高速で微分可能なGPUレンダリング手法

## 前提知識
### NVS(Novel View Synthesis)
- 複数の視点から撮影された画像を手掛かりに新たな視点の画像を合成する技術のこと

### Rasterization
- 3Dシーンの情報を2D平面に投影して画像としてレンダリングするプロセス

### SfM
- Structure from Motion
- ある対象を撮影した複数枚の写真から対象の形状を復元する技術の総称
- つまり、複数の2D画像から3Dシーンの構造を推定する
- カメラが異なる位置や角度から撮影した複数の画像を解析し、視点の変化に基づいてシーン内の特徴点の3D座標とカメラの位置・姿勢を同時に推定

### Camera Calibration
- カメラ内部および外部のパラメータを求めるプロセス
- カメラの内部パラメータは行列$K$であり以下で定まる

$$
\mathbf{K} = \begin{pmatrix} f_x & 0 & c_x \\ 0 & f_y & c_y \\ 0 & 0 & 1 \end{pmatrix}
$$
- ただし、$f_x, f_y$は焦点距離, $c_x, c_y$は画像平面の主点(光学中心)の座標
- 外部パラメータは回転行列$\mathbf{R}$と平行移動ベクトル$\mathbf{t}$でカメラの位置を表す
- 外部パラメータと内部パラメータを用いてワールド座標系の3D点$(X,Y,Z)$を2D画像平面上の点$(u,v)$に投影する式は以下のように書ける

$$
\begin{pmatrix} u \\ v \\ 1 \end{pmatrix} = \mathbf{K} \begin{pmatrix} \mathbf{R} | \mathbf{t} \end{pmatrix} \begin{pmatrix} X \\ Y \\ Z \\ 1 \end{pmatrix}
$$

### MVS
- Multi View Stereo

### spherical harmonic
- 球面調和関数

### クォータニオン
- 任意軸回転を表現する
- 3次元空間内の任意の回転を表現できる
- 3次元空間において方向ベクトル$(\lambda_x, \lambda_y, \lambda_z)$を回転軸として角度$\theta$だけ回転するときクォータニオンは4次元ベクトルとして以下のようにあらわされる

$$
\mathbf{q} = \left(
    \lambda_x \sin \frac{\theta}{2},
    \lambda_y \sin \frac{\theta}{2},
    \lambda_z \sin \frac{\theta}{2},
    \cos \frac{\theta}{2},
    \right)
$$

## 3DGS
### Gaussianの導入
- ワールド空間で定義された完全な3D共分散行列$\Sigma$で定義
- 平均$\mu$を中心とする
- よって以下のように書ける

$$
G\left(\mathbf{x}\right) = \exp \left(- \frac{1}{2}\mathbf{x}^{\top}\Sigma^{-1}\mathbf{x}\right)
$$
- このガウシアンはblending processで$\alpha$と掛け算される
- この$\alpha$が不透明度
- 3Dガウシアンを2Dに投影することを考える
- 視点変換$W$が与えられたときにカメラ座標系での共分散行列$\Sigma^{\prime}$は

$$
\Sigma^{\prime} = JW\Sigma W^{\top} J^{\top}
$$
- ここで$J$は射影変換のアフィン近似のヤコビアン
- この$\Sigma^{\prime}$の第3行と第3列を無視して得られる$2 \times 2$行列がカメラ座標系での共分散行列


### Initialization
- 点群からGuassianを構築
- 共分散行列は以下、未理解
  - We estimate the initial covariance matrix as an isotropic Gaussian
with axes equal to the mean of the distance to the closest three points.

### Gaussianの最適化の準備
- 共分散行列$\Sigma$を直接最適化するとGaussianではなくなる可能性がある
  - 共分散行列$\Sigma$は半正定値行列(positive semidefinite matrix)という制約をもつ
  - ただ最適化するだけだと当然この制約を満たすとは限らない
- そこで以下のように分解する

$$
\Sigma^{\prime} = RSS^{\top}R^{\top}
$$
- $S$はスケーリング行列、$R$は回転行列
- $S$をスケーリングのための3Dベクトル$\mathbf{s}$で表現
- $R$を4Dベクトルクォータニオン$\mathbf{q}$で表現
- この$\mathbf{s},\mathbf{q}$を最適化する

### Optimization with adaptive density control
- ガウシアンのパラメータの最適化とガウシアンの密度のコントロールが交互に行われる
- 最適化するガウシアンのパラメータは以下
  - $\mathbf{p}$ : 位置
  - $\mathbf{\alpha}$ : 不透明度
  - $\Sigma$ : 共分散行列
    - 実際はスケール$\mathbf{s}$とクォータニオン$\mathbf{q}$
  - $c$ : 色(SH係数が色を表す)
    - 実際は球面調和パラメータ$\mathbf{h}$
- よってガウシアンのパラメータは5つのベクトル(世界座標におけるガウシアンの分布点、クォータニオン(回転)、スケール、球面調和パラメータ、不透明度)
  - これら5つのベクトルがガウシアンの個数分だけ存在する
- 不透明度$\alpha$にはシグモイド関数を活性化関数として適用して$[0,1]$にする
- 共分散行列には指数関数を活性化関数として適用
- 損失関数は最終的に得られた画像に対してL1ロスとD-SSIMで比較

$$
\mathcal{L} = \left(1 - \lambda\right)\mathcal{L}_1 + \lambda \mathcal{L}_{\mathrm{D-SSIM}}
$$
- Adaptive density Controlについては以下
  - 100回の反復ごとに透明なガウシアンを削除する
    - 不透明度$\alpha$が閾値$\epsilon_{\alpha}$より小さいもの
  - 視点空間における位置勾配の平均大きさ(average magnitude of view-space position gradients)が閾値$\tau_{pos}$を上回るものを探す
  - このaverage magnitude of view-space position gradientsは$\frac{\partial\mathcal{L}}{\partial\mathbf{u}}$のこと
    - $\mathbf{u} = \mathbf{K} \begin{pmatrix} \mathbf{R} | \mathbf{t} \end{pmatrix}\mathbf{p}$(ワールド座標系からカメラ座標系への変換)
    - under-reconstructed regionsにあるような小さいガウシアンは自身と同じサイズのガウシアンを複製する(Clone)
    - over-reconstructed regionsにあるような大きいガウシアンは自身を2分割してスケールを小さくする(Split)

### Fast Differentiable Rasterizer for Gaussians
- section 6
- 未理解

## 英単語メモ
- Radiance Field methods : NeRF(Neural Radiance Field)のこと
- interleave : 交互配置する
- anisotropic : 異方性の
- opacity : 不透明度
- spherical : 球状の
- harmonic : 調和した
- flickering : ちらつき
- drawback : 欠点
- regime : 政権、体制