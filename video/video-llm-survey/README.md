# Video Understanding with Large Language Models: A Survey

## ソース
- https://arxiv.org/pdf/2312.17432
- https://github.com/yunlong10/Awesome-LLMs-for-Video-Understanding

## 概要
- LLMを利用したVideo UnderstandingのSurvey
  - Vid-LLMs
- Vid-LLMsのアプローチを3種に分類
  - Video Analyzer × LLM
  - Video Embedder × LLM
  - (Analyzer + Embedder) × LLM
- LLMの役割を5種に分類
  - Summarizer
  - Manager
  - Text Decoder
  - Regressor
  - Hidden Layer

## Video Understandingの歴史
- 以下の4段階
    1. Conventional Methods
    2. Early Neural Video Models
    3. Self-supervised Video Pretraining
    4. Large Language Models for Video Understanding
- 1のconventionalはSHIFTやHMMとか
- 2はCNN, ViT
- 3はMAEをvideoに適用したという話

## SSLについて
- self supervised learning(自己教師あり学習)
- Vision分野ではCL(Contrasive learning:対象学習)とMIM(Masked Image Modeling)が主流
- https://qiita.com/syunki_tacase/items/6d906041c78049d52636
- CLの代表的なSimCLRについて
  - ミニバッチに対してdata augmentationをする
  - 得られた新しいミニバッチも同じくencoderに通す
  - 得られた表現に対して、data augmentationで変換された画像同士の類似度を近く、そうでないものを遠くする
    - 全く関係ない画像A,Bがミニバッチ内にあるとする
    - data augmentationでA->C, B->Dになる
    - (A,C), (B,D)の類似度を高く、(A,B),(C,D)の類似度を低くするような対照学習
- MIMの代表的なMAE(Masked Auto Encoder)について
  - ViTの構造
  - Encoderに入れる前に画像をパッチ分割
  - いくつかのパッチをマスクする
  - マスクしていないパッチのみをencoderに入力する
  - decoderの出力をマスクしたパッチになるように学習するというもの
- 詳しくは上のqiita参照

## 動画タスク一覧
### Video Classification
### Action Recognition
### Text Video Retrieval
- テキストと複数の動画を入力として出力はテキストに合致した動画
- 候補として動画が与えられるということ
### Video to Text Summarization
### Video Captioning
### Video QA
- Video Question Answering
### Video Summarization
- video to video
### Video Highlight Detection
- segments選択
### Temporal Action Proposal Generation
- 与えられたactionやeventを含むsegmentsを出力
### Moment Retrieval
### Generic Event Boundary Detection
### Generic Event Boundary Captioning & Grounding
### Dense Video Captioning
### Object Tracking
### Re-Identification
### Video Saliency Detection
### Video Object Segmentation
### Video Object Referring Segmentation
### Spatiotemporal Grounding

## 英単語
- burgeoning : 急成長している
- harness : 利用する
- glanularity : 粒度
- versatility : 多用途性
- predominant : 支配的な
- prevalence : 普及率
- surveillance : 監視
- prominence : 著名、卓越
- groundbreaking : 画期的な
- delve : 掘り下げる