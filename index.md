## Contents

* [기술 블로그](./technical_articles/index.md)
* [머신러닝 노트](./machine_learning_notes/index.md)
* [의료정보시스템](./medical_information_systems/index.md)

\begin{align} A(x) &:=\frac{x_{k+1}-x}{x_{k+1}-x_{k}} \
B(x) &:= 1 - A = \frac{x - x_{k}}{x_{k+1}-x_{k}} \
C(x) &:=\frac{1}{6} (A^3 - A) (x_{k+1}-x_{k})^2 \
D(x) &:=\frac{1}{6} (B^3 - B) (x_{k+1}-x_{k})^2 \
\end{align} 로 정의하겠습니다. 그리고 $$(x_k,f_k)$$에서의 2계 미분값은 $$f_{k}^{''}$$로 쓰겠습니다. 이렇게 쓰면, 우리는 $$x_k,x_{k+1}$$ 사이에서 보간할 함수는 $$F_{k}(x) = A(x) f_k + B(x) f_{k+1} + C(x) f_{k}^{''} + D(x) f_{k+1}^{''}$$ 가 됩니다. 왜 $$F_{k}(x)$$가 저렇게 생겼냐 하면, \begin{align} F_{k}(x_k) &= f_k \
F_{k}(x_{k+1}) &= f_{k+1} \
F_{k}^{''} (x_k) &= f_{k}^{''} \
F_{k}{''} (x_{k+1}) &= f_{k+1}^{''} \
\end{align} 