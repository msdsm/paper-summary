# NTIRE 2024 Challenge on Light Field Image Super-Resolution: Methods and Results

## 概要
- NTIRE2024のLight Field Image Super Resolutionコンペの結果
- 2部門用意
    - 精度向上だけを目的としたもの(PSNRなどで評価)
    - それにくわえて制約を加えたもの(モデルサイズの上限とFLOPSの上限)

## Light Field Image Super Resolution
- Light Fieldの画像のそれぞれをsuper resolutionするというタスク
- このとき、Light Field imagesが$3 \times 3$枚の形状で与えられるときにこれをAngular resolutionという

## Related Work
- LF SRはTraditional -> CNN based -> Transformer basedと発展してきた

## result
- 結果は、EPITを改良したものが1位
- EPITの層を深くした
- channel of feature mapsを大きくした
- data augmentation
    - random horizontal flipping
    - random vertical flipping
    - 90-degree rotaion
    - このときLF structuresが崩壊しないようにflippingとrotationをやる

## dataset
- Training set
    - EPFL
    - HCInew
    - HCIold
    - INRIA
    - STFgantry