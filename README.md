# pytorch-pruning
## このリポジトリについて

- このリポジトリは、[GitHub - jacobgil/pytorch-pruning](https://github.com/jacobgil/pytorch-pruning)の日本語による解説を目的としています。
- [[1611.06440] Pruning Convolutional Neural Networks for Resource Efficient Inference](https://arxiv.org/abs/1611.06440)のpytorch実装です。

## 概要

- 畳み込み層のフィルタをpruningしてネットワークモデルを軽量化します。
  - 重要度の低いn枚のフィルタを削除し、ファインチューニングを行います。
- VGG-16ベースの犬猫2クラス分類モデルをpruningした結果

|フィルタ枚数  |4224  |1664  | 896 | 128 |
|---|---:|---:|---:|---:|
|認識精度 |0.9875  |0.9613  | 0.9625 | 0.72875 |
|実行時間 |12.01 s  |6.26s  | 3.36s | 2.04s |

- 実行環境: Intel(R) Xeon(R) CPU E5-2690 v3 @ 2.60GHz, Tesla K40c, メモリ128GB
- テストデータ800枚，バッチサイズ32

## 使い方

### データセットの準備

PyTorchのImageFolderを使用しているため、分類クラスごとディレクトリ作成し、その中に画像ファイルを保存します。

```
train/
+---dogs/
+---cats/
test
+---dogs/
+---cats/
```

画像は[Dogs vs. Cats | Kaggle](https://www.kaggle.com/c/dogs-vs-cats)からダウンロードします。

[元リポジトリのブログ](https://jacobgil.github.io/deeplearning/pruning-deep-learning)に倣い、そのうち各クラス1000枚ずつをトレーニングデータ、400枚ずつをテストデータとしました。


### 実行

**学習**
``` bash
python finetune.py --train --save model
```

**pruning**
```bash
python finetune.py --prune --pruned_filters_per_iter 512 --pruning_iter 5 --model model --save prunned_model
```

**テスト**

``` bash
python finetune.py --test --model model
python finetune.py --test --model prunned_model
```


### Jupyter Notebookによるソースコードの解説

[pytorch-pruning/tutorial.ipynb](https://github.com/i4e/pytorch-pruning/blob/master/tutorial.ipynb)
