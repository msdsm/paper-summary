# Mip-NeRF: A Multiscale Representation for Anti-Aliasing Neural Radiance Fields

## 概要
- mip-NeRFの提案
- mipはmipmapからきている
- タスクはNVS
- NeRFはrayだけどmip-NeRFはconeを打つ
    - NeRFは異なるrayでも同じ座標なら同じ密度が出る
    - 視点によるvolumeのサイズなどを考慮していないからconeにするという発想

## method
- conical frustumの式まで追えた
- そこからまだ

## 英単語
- ameliorate : 改善する
- perpendicular : 垂直
- infinitesimally : 無限小, 限りなく小さい
- conical : 円錐
- frustum : 錐台(おそらく錐体を水平な平面で切ったときの下側の立体部分)
- conical frustum : 円錐体を水平な平面で切ったときの下側の部分(底面も上面も円の立体で横から見ると台形になるやつ)