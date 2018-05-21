[Top](index.md)

# 머신러닝의 기본 개념 (Basic Concept of Machine learning)

머신러닝은 예측(prediction) 문제를 풀 수 있습니다. 예측 문제에는 회귀(regression)와 분류(classification) 문제가 있습니다. 회귀는 실수(real number)를 예측 합니다. 5세부터 15세까지 아동들의 나이와 몸무게를 주면 키를 예측하는 문제는 회귀 문제입니다. 분류 문제는 범주를 예측 합니다. 봉투에 쓰여진 손으로 쓴 우편번호를 분류하는 문제는 분류 문제 입니다.

5세부터 15세까지의 아이들의 나이와 몸무게를 주면 키를 예측하는 회귀 문제가 있습니다. 머신러닝에서 입력에 해당하는 나이와 몸무게는 피쳐라고 하며 $x$로 표현 합니다. 예측 결과인 키는 타겟이라고 하며 $t$나 $y$로 표현합니다. 이 블로그에서는 $y$를 사용하겠습니다.

아동들의 키 회귀 문제에서 피쳐에 해당하는 변수는 나이와 몸무게로 2가지 입니다. 키를 $x_1$로, 몸무게를 $x_2$로 표시 합니다. 피쳐가 $D$개가 있는 문제라면 $x_1, x_2, \cdots , x_D$으로 표시하며 피쳐를 벡터 $\mathbf{x}$로 표시 합니다.

$$
\mathbf{x}
= \begin{pmatrix}
x_1, x_2, \cdots , x_D
\end{pmatrix} ^T
$$

아동들의 키 회귀 문제에서 타겟에 해당하는 변수는 키로 1가지이며 $y$로 표시 합니다. 만일 타겟이 $K$개가 있는 문제라면 벡터 $\mathbf{y}$로 표시 합니다. 

$$
\mathbf{y}
= \begin{pmatrix}
y_1, y_2, \cdots , y_K
\end{pmatrix} ^T
$$

회귀문제에서 데이터셋은 쌍입니다. 아동이 4명이라면 피쳐 벡터도 4개이고 타겟 벡터도 4개 입니다. 4개의 피쳐 벡터는 $ \mathbf{x}_1, \mathbf{x}_2, \mathbf{x}_3, \mathbf{x}_4 $로 표시하며, 4개의 타겟 벡터는 $ \mathbf{y}_1, \mathbf{y}_2, \mathbf{y}_3, \mathbf{y}_4 $로 표시 합니다. 이것을 $ \mathbb{X} = \{ \mathbf{x}_1, \mathbf{x}_2, \mathbf{x}_3, \mathbf{x}_4 \} $ 그리고 $ \mathbb{Y} = \{ \mathbf{y}_1, \mathbf{y}_2, \mathbf{y}_3, \mathbf{y}_4 \} $로 집합으로 표시 합니다. $N$개의 데이터 셋이 있는 집합을

$$
\mathbb{X} = \{ \mathbf{x}_1, \mathbf{x}_2, \cdots, \mathbf{x}_N \}
$$

$$
\mathbb{Y} = \{ \mathbf{y}_1, \mathbf{y}_2, \cdots, \mathbf{y}_N \}
$$

으로 일반화 화하여 표시 합니다.

## 참조

- bishop 2017