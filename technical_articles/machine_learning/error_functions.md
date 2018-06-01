# 오차함수 (error functions)

## RSS


$$
E(\mathbf{w})
=
\frac{1}{2}
\sum _{n=1} ^N
\{
	y(x_n, \mathbf{w}) - t_n
\}^2
\tag{1.2}
$$

# RMS (root-mean-square)


$$
E_\text{RMS} = \sqrt{2\frac{E(\mathbf{w}^*)}{N}}
$$

## 능선회귀 (ridge regression)

$$
\tilde{E}(\mathbf{w})
=
\frac{1}{2}
\sum_{n=1}^N
	\{
		y(x_n, \mathbf{w}) - t_n
	\}^2
	+
	\frac{\lambda}{2} \| \mathbf{w} \| ^2
	
\tag{1.4}
$$



## 참조

- bishop 2007

