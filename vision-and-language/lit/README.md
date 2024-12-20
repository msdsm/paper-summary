# LiT : Zero-Shot Transfer with Locked-image text Tuning

- https://arxiv.org/pdf/2111.07991

## 概要
- vision and languageのpre-trainingでcontrastive leraningの際にencoderを固定するか学習するか調査した
- LiT(Locked-image text Tuning)が一番良いと提案

## LiT
- image encoder, text encoderに対してそれぞれ以下の3通りある
    - L : locked variables and initialized from a pre-trained model
    - U : unlocked and initialized from a pre-trained model
    - u : unlocked and randomly initialized
- 実験の結果、(image, text)をLuにするのがよかった
- つまりimage encoderは事前学習されたモデルを固定して使って、text encoderはスクラッチで学習
- このパターンをLiT(Locked-image text Tuning)と名付けた

## 英語
- insofar as : ~する限りにおいて
- halve : 半分にする
