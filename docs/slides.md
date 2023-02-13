---
---

# ニューラルネットワーク

---

# 1. ニューラルネットワークとは

人間の脳の神経回路を模した数理モデル

## 数理モデル

ある現象を数式で記述したもの

→ ここでは**関数**と捉えておくと良い。なんらかの数字が入力されると、なんらかの数字を出力するもの。

---

## 機械学習モデル

前提知識として、機械学習モデルについて説明する。ニューラルネットワークもこの中に含まれる。
知識のある人は飛ばして良い。

ニューラルネットワーク同様、このモデルもなんらかの入力に対してなんらかの出力を行う。
そして賢い機械学習モデルは、複雑なタスクをこなすことができる=入力に対して適切な出力が出来る

例

| タスク | 入力 | 出力 |
| --- | --- | --- |
| 画像分類 | 猫の画像 | 猫 |
| 将棋 | 盤面 | 次の一手 |
| 会話 | 質問 | 回答 |

---

機械学習モデルは入出力を数値として扱うため、機械学習モデルが適応できるタスクは入出力を数値として表現できるタスクに限られる。またここでの数値は厳密にはテンソルである。

先程の例で考えると、例えば画像は各画素の色を表す数値の集合で表せる。また画像分類の結果は、分類する種類の数だけ整数を割り当てれば良い。犬と猫の分類なら、どちらかに0、どちらかに1を割り当てれば良い。

---

## 機械学習

賢い機械学習モデルは、機械学習によって作られる。

機械学習モデルは関数であった。ここで、「関数内の数式」は**事前に決まっている**。しかし、式の中の具体的な数値は決まっていない。つまり、「入力に$a$をかけて$b$を足す」といった処理の内容は決まっているが、$a$や$b$の具体的な値は決まっていないということ。この$a$や$b$のことをパラメータといい、最適なパラメータを求める手法の一つが機械学習である。

---

話をニューラルネットワークに戻そう。

ニューラルネットワークは他の機械学習モデルに比べて表現力に長けており、多様な入力に対して適切な出力を行うことができる。また拡張性が非常に高く、近年話題のAIには全てニューラルネットワークが使われていると言っても過言ではない。この資料ではそんなニューラルネットワークについて学んでいく。

---

# 2. ニューラルネットワークの構造

NN（ニューラルネットワーク）のイメージはこんな感じ

図

---

## パーセプトロン

NNを構成する要素。図の〇がパーセプトロン。  
パーセプトロンも1つのモデルとみることが出来て、入力に対してパラメータに基づいた出力を行う。

パーセプトロンは複数の入力値を受け取り、0か1を出力する。  
パラメータは二種類あり、重みとバイアスである。入力値に重みをかけたものの和（線形和）とバイアスを足したものが0以上なら1、0未満なら0を出力する。入力を$x$、出力を$y$、重みを$w$、バイアスを$b$とすると、以下の式で表せる。

$$
y = \left\{
\begin{array}{ll}
1 & (x_1 w_1 + x_2 w_2 + \cdots + x_n w_n \geq b) \\
0 & (x_1 w_1 + x_2 w_2 + \cdots + x_n w_n  < b)
\end{array}
\right.
$$

まとめると以下。
入力をベクトル$x$、重みをベクトル$w$で表す。あと今後分かりやすくするために$b$を移行する。

$$
y = \left\{
\begin{array}{ll}
1 & (x \cdot w - b \geq 0) \\
0 & (x \cdot w - b < 0)
\end{array}
\right.
$$

$x = (x_1, x_2, \cdots , x_n)$  
$w = (w_1, w_2, \cdots , w_n)$


---

Pythonで実装してみよう。入力する値は二つで、パラメータは適当。

```python
def perceptron(x1, x2):
    x = [x1, x2] # 入力をベクトル(1次元配列)に変換
    w = [0.5, 0.5] # 重み
    b = 0.7 # バイアス

    if np.dot(x, w) - b >= 0: # 内積がバイアス以上のとき
        return 1 # 1を出力
    else:
        return 0
```

適当に値を入れてみると

```python
y = perceptron(2, -1)
print(y)
>>> 0
```

$(2 \times 0.5) + (-1 \times 0.5) = 0.5 < 0.7$ と、内積(0.5)がバイアス(0.7)を超えなかったので0が出力された

---

## 人工ニューロン

人間の脳の神経細胞（ニューロン）を模した数理モデル。  
パーセプトロンの出力を実数全体に拡張したもの。内積にバイアスを足した値を**活性化関数**と呼ばれる関数に入れたときの値を出力する。

実装してみる

```python
activation = lambda x: 2*x # 活性化関数

def neuron(x1, x2):
    x = [x1, x2]
    w = [0.5, 0.5]
    b = 0.7

    y = activation(np.dot(x, w) + b)
    return y
```

適当に値を入れてみると

```python
y = neuron(2, -3)
print(y)
>>> 0.3999999999999999
```

$((2 \times 0.5) + (-3 \times 0.5) + 0.7) \times 2 = 0.4$と、（誤差が生じているが）正しい値が出力された。

---

## ニューラルネットワーク

人工ニューロンを何個も組み合わせたモデル。先程の図を再掲。

図

各人工ニューロンが別々のパラメータを持っており、NNの学習は、それらすべてを最適化することとなる。

---

### 演算の流れ

それぞれの人工ニューロンは、前の人工ニューロンからの出力を入力にとり、演算結果(出力)を次の人工ニューロンに渡す。これを繰り返して最終的に出力された値がそのニューラルネットワークの出力となる。

---

### 層

NNは層をひとつの単位として扱う。層はNNの縦一列を指す。

最初の層は入力層、最後の層は出力層、それ以外の層は中間層または隠れ層と呼ぶ。

図

---

活性化関数は各人工ニューロンに対して設定できるが、NNを考える場合は層ごとに設定する。というか活性化関数を一つの層として考える。人工ニューロンは内積とバイアスの和をそのまま出力するだけ。

図

---

### 深層学習

2つ以上の中間層を持つものはディープニューラルネットワークとも呼ばれる。このディープニューラルネットワークの学習は**ディープラーニング**と呼ばれ、これを和訳したものが**深層学習**である。

図

---

## 色々な層



---

### 全結合層

人工ニューロンを縦に並べて構成した層。

$$
y = x \cdot W + b
$$

```python
class Linear:
    def __init__(self, input_size, output_size):
        self.W = np.random.randn(input_size, output_size)
        self.b = np.random.randn(output_size)

    def forward(self, x):
        return np.dot(x, self.W) + self.b
```

全結合層には他にも様々な呼び方がある。

- 線形層
- linear
- fc (fully connected)
- dense
- affine

全結合層はNNを構築する上での最も基本的な層となる。

---

### Dropout

NNの学習において、過学習を防ぐために用いられる層。入力された値を確率で0にして出力する。

$$
y = \left\{
\begin{array}{ll}
x & (確率p) \\
0 & (確率1-p)
\end{array}
\right.
$$

```python
class Dropout:
    def __init__(self, p):
        self.p = p

    def forward(self, x):
        return np.where(np.random.rand(*x.shape) < self.p, x, 0)
```

---

### Batch Normalization

バッチ正規化。Dropout同様、過学習防止に役立つ。また学習を安定させる面でも有効。

$$
y = \frac{x - \mu}{\sqrt{\sigma^2 + \epsilon}}
$$

```python
class BatchNormalization:
    def __init__(self, gamma, beta):
        self.gamma = gamma
        self.beta = beta

    def forward(self, x):
        mu = np.mean(x, axis=0)
        sigma = np.std(x, axis=0)
        return self.gamma * (x - mu) / (sigma + 1e-7) + self.beta
```

---

### 畳み込み層

---

### プーリング層

---

### 再帰層

---


---

## 活性化関数

各層の出力を次の層へ渡す際にかける関数。

---