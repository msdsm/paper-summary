# DDIM : Denoising Diffusion Implicit Models
===== 論文ソース =====
  * [[https://arxiv.org/pdf/2010.11929.pdf|DENOISING DIFFUSION IMPLICIT MODELS]]
===== 参考 =====
  * https://henatips.com/page/46/#ddim
===== 概要 =====
  * DDPMの生成過程(逆拡散過程)のステップ数を減らした
    * DDPMの生成過程はT->T-1->...t->t-1->...->0という風にTから0まで連続していた
    * DDIMでは飛ばすことができる(たとえばT->T-10->...->0)
  * 実験した結果DDPMが1000ステップかけてやっていたのに対して、DDIMで200ステップでやっても結果かわらなかった
    * 大幅にステップ数を減らすことに成功してすごいという話

===== DDIM =====
  * 先に結論
  * $\{0, 1, 2, \cdots, T\}$の部分列$\tau = \{\tau_0, \tau_1, \cdots, \tau_I\}$を任意に取る
  * ただし、$\tau_0 = 0, \tau_I = T$
    * ここでいう部分列とは順番を入れ替えない部分列のこと
    * つまり、競技単調増加性は保持されたまま
  * $\tau$に含まれる任意の隣接する2つの要素$\left(s, t\right)=\left(\tau_{i-1}, \tau_{i}\right)$にたいして次のように定義される非Markov確率過程を考える

\begin{eqnarray}
q\left(x_T|x_0\right) &:=& p_{\mathcal{N}}\left(
x_T|
\alpha_T x_0, 
\left(
1-\alpha_T^2
\right)I
\right)
\\
q\left(x_s|x_t,x_0\right) &:=& p_{\mathcal{N}}\left(
x_s|
\alpha_s x_0 +
\sqrt{\sigma_s^2 - s_t^2}
\frac{x_t - \alpha_t x_0}{\sigma_t},
s_t^2I
\right)
\end{eqnarray}

  * ちなみに、$\alpha_t^2+\sigma_t^2=1, s_t^2=\frac{1-\alpha_s^2}{1-\alpha_t^2}\left(1-\frac{\alpha_t^2}{\alpha_s^2}\right)$とすればDDPMと一致
{{ :user:2022masuda:study:スクリーンショット_2024-05-03_14.57.59.png?600 |}}
  * 導出は下
==== 拡散過程の導出 ====
  * いつかやる
  * https://henatips.com/page/46/#ddim
==== 生成過程の導出 ====
  * いつかやる
  * https://henatips.com/page/46/#ddim
==== 損失関数の導出 ====
  * いつかやる
  * https://henatips.com/page/46/#ddim

==== 生成アルゴリズム ====
  * $\{0, 1, 2, \cdots, T\}$の部分列$\tau = \{\tau_0, \tau_1, \cdots, \tau_I\}$をとる。ただし$\tau_I=T, \tau_0=0$
  - $x_{\tau_I}=x_T$を$\mathcal{N}\left(0,I\right)$からサンプル
  - 以下の手順を$\left(t,s\right)=\left(\tau_I,\tau_{I-1}\right), \left(\tau_{I-1},\tau_{I-2}\right),\cdots,\left(\tau_1,\tau_0\right)$に対して繰り返し行う
    - $x_t, t$をニューラルネットワークに入力して出力$\epsilon_{\theta}$を得る
    - $n\sim\mathcal{N}\left(0,I\right)$を取る
    - $x_s := \frac{\alpha_s}{\alpha_t}\left(x_t-\sqrt{1-\alpha_t^2 \epsilon_{\theta}}\right)+\sqrt{1-\alpha_s^2-s_t^2}\epsilon_{\theta}+s_t n$
  - $x_0=x_{\tau_0}$を画像として出力