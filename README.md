# Advanced Vision 2025

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/basiliskv/advanced-vision-2025/blob/main/advanced_vision_kadai2025.ipynb)

本リポジトリは，アドバンスドビジョン（2025年度）の課題として作成したものです。

『ゼロから作る Deep Learning ― Pythonで学ぶディープラーニング』
（斎藤 康毅，オライリー・ジャパン）に掲載されている
CNN のサンプル実装をベースに，ネットワーク構造を一部拡張しています。

---

## 概要

本課題では，基本的な CNN 構造に **スキップ接続（skip connection）** を追加した
画像分類モデルを実装しています。

スキップ接続を導入することで，

- 勾配消失の緩和  
- 浅い層の特徴と深い層の特徴の統合  

を意図しています。

---

## ネットワーク構造

本課題で使用したネットワークの構造を以下に示します。

<p align="center">
  <img src="fig/network.png" width="200">
</p>

- 入力：28 × 28 × 1  
- 畳み込み層：5×5 → 3×3 → 3×3  
- 最初の畳み込み層の出力を，後段の出力と加算するスキップ接続を導入 
- 加算後に ReLU，Max Pooling を適用
- 全結合層を経て，10 クラス分類  

---

## CNN の基本構造

本モデルは，CNN（畳み込みニューラルネットワーク）を基礎としています。
CNN は，畳み込み層，活性化関数，プーリング層，全結合層から構成されます。

---

### 畳み込み層

入力特徴マップ $\mathbf{X}$ とカーネル $\mathbf{W}$ を用いた畳み込みは，
次式で表されます。

$$
Z_{i,j} = \sum_{m,n} X_{i+m, j+n} W_{m,n} + b
$$

特徴マップの局所領域とフィルタを重ね合わせ，
対応する要素同士を掛け算して足し合わせた値を，
出力特徴マップの1画素として計算しています。

ここで，

- $i, j$ は 出力特徴マップ上の位置  
- $m, n$ は カーネル（フィルタ）内の各要素の位置

を表します  
なお，ストライドは出力位置 $(i,j)$ の移動間隔を決め，
パディングは入力特徴マップの周囲をどのように扱うかを決定します。

このような計算を画像全体に対して行うことで，
畳み込み層は画像中の特徴的なパターンに反応する部分を検出します。

---

### 活性化関数

本モデルでは，活性化関数として ReLU を用いています。

$$
\mathrm{ReLU}(x) = \max(0, x)
$$

ReLU により非線形性が導入され，学習時の勾配消失の緩和が期待できます。

---

### プーリング層

Max Pooling では，局所領域内で最も強く反応した特徴のみを残し, 特徴マップのサイズを小さくします。

$$
y = \max_{x \in \mathcal{R}} x
$$

---

### 全結合層と出力

抽出された特徴を全結合層に入力し，Softmax により各クラスごとの確率分布を出力します。

$$
p_k = \frac{\exp(z_k)}{\sum_{i=1}^{C} \exp(z_i)}
$$

ここで $z_k$ は全結合層の出力（スコア）を表し，
$C$ はクラス数を表します。

Softmax 関数により，各クラスの出力は 0 から 1 の値に正規化され，
全クラスの確率の総和が 1 となるため，
$p_k$ は，入力データがクラス $k$ に属する確率として表されます。

---

## 変更点（オリジナル実装からの拡張）

書籍に掲載されている基本的な CNN 実装に対して，以下の変更を行っています。

- 畳み込みブロックに **スキップ接続（加算）** を追加
- スキップ接続後に ReLU を適用する構造に変更 

---

## 実装環境

- Python 3.10  
- NumPy  

---

## License

This project is licensed under the MIT License.

This repository includes modified code based on examples from:

"ゼロから作る Deep Learning ― Pythonで学ぶディープラーニング"  
斎藤 康毅，オライリー・ジャパン  

The original code is provided under the MIT License.
