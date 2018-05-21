## Contents

* [기술 블로그](./technical_articles/index.md)
* [머신러닝 노트](./machine_learning_notes/index.md)
* [의료정보시스템](./medical_information_systems/index.md)

$$
\begin{align} A(x) &=\frac{x_{k+1}-x}{x_{k+1}-x_{k}} \\
B(x) &:= 1 - A = \frac{x - x_{k}}{x_{k+1}-x_{k}} \\
C(x) &:=\frac{1}{6} (A^3 - A) (x_{k+1}-x_{k})^2 \\
D(x) &:=\frac{1}{6} (B^3 - B) (x_{k+1}-x_{k})^2 \\
\end{align}
$$