# 3D transformation

## Translation:

Right handed:
$$
\begin{bmatrix}
x' \\
y' \\
z' \\
1 \\
\end{bmatrix}
=
\begin{bmatrix}
1 & 0 & 0 & t_x \\
0 & 1 & 0 & t_y \\
0 & 0 & 1 & t_z \\
0 & 0 & 0 & 1 \\
\end{bmatrix}
\begin{bmatrix}
x \\
y \\
z \\
1 \\
\end{bmatrix}
$$
Left handed:
$$
\begin{bmatrix}
x' \\
y' \\
z' \\
1 \\
\end{bmatrix}
=
\begin{bmatrix}
1 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 \\
0 & 0 & 1 & 0 \\
t_x & t_y & t_z & 1 \\
\end{bmatrix}
\begin{bmatrix}
x \\
y \\
z \\
1 \\
\end{bmatrix}
$$


## Scaling:

Right handed & Right:
$$
\begin{bmatrix}
x' \\
y' \\
z' \\
1 \\
\end{bmatrix}
=
\begin{bmatrix}
s_x & 0 & 0 & 0 \\
0 & s_y & 0 & 0 \\
0 & 0 & s_z & 0 \\
0 & 0 & 0 & 1 \\
\end{bmatrix}
\begin{bmatrix}
x \\
y \\
z \\
1 \\
\end{bmatrix}
$$

## Rotation:

#### About x-axis:

Right handed:
$$
\begin{bmatrix}
x' \\
y' \\
z' \\
1 \\
\end{bmatrix}
=
\begin{bmatrix}
1 & 0 & 0 & 0 \\
0 & \cos(\theta) & -\sin(\theta) & 0 \\
0 & \sin(\theta) & \cos(\theta) & 0 \\
0 & 0 & 0 & 1 \\
\end{bmatrix}
\begin{bmatrix}
x \\
y \\
z \\
1 \\
\end{bmatrix}
$$
Left handed:
$$
\begin{bmatrix}
x' \\
y' \\
z' \\
1 \\
\end{bmatrix}
=
\begin{bmatrix}
1 & 0 & 0 & 0 \\
0 & \cos(\theta) & \sin(\theta) & 0 \\
0 & -\sin(\theta) & \cos(\theta) & 0 \\
0 & 0 & 0 & 1 \\
\end{bmatrix}
\begin{bmatrix}
x \\
y \\
z \\
1 \\
\end{bmatrix}
$$

#### About y-axis:

Right handed:
$$
\begin{bmatrix}
x' \\
y' \\
z' \\
1 \\
\end{bmatrix}
=
\begin{bmatrix}
\cos(\theta) & 0 & \sin(\theta) & 0 \\
0 & 1 & 0 & 0 \\
-\sin(\theta) & 0 & \cos(\theta) & 0 \\
0 & 0 & 0 & 1 \\
\end{bmatrix}
\begin{bmatrix}
x \\
y \\
z \\
1 \\
\end{bmatrix}
$$
Left handed:
$$
\begin{bmatrix}
x' \\
y' \\
z' \\
1 \\
\end{bmatrix}
=
\begin{bmatrix}
\cos(\theta) & 0 & -\sin(\theta) & 0 \\
0 & 1 & 0 & 0 \\
\sin(\theta) & 0 & \cos(\theta) & 0 \\
0 & 0 & 0 & 1 \\
\end{bmatrix}
\begin{bmatrix}
x \\
y \\
z \\
1 \\
\end{bmatrix}
$$

### About z-axis:

Right handed:
$$
\begin{bmatrix}
x' \\
y' \\
z' \\
1 \\
\end{bmatrix}
=
\begin{bmatrix}
\cos(\theta) & -\sin(\theta) & 0 & 0 \\
\sin(\theta) & \cos(\theta) & 0 & 0 \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1 \\
\end{bmatrix}
\begin{bmatrix}
x \\
y \\
z \\
1 \\
\end{bmatrix}
$$
Left handed:
$$
\begin{bmatrix}
x' \\
y' \\
z' \\
1 \\
\end{bmatrix}
=
\begin{bmatrix}
\cos(\theta) & \sin(\theta) & 0 & 0 \\
-\sin(\theta) & \cos(\theta) & 0 & 0 \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1 \\
\end{bmatrix}
\begin{bmatrix}
x \\
y \\
z \\
1 \\
\end{bmatrix}
$$

### shear:

Right handed:
$$
\begin{bmatrix}
x' \\
y' \\
z' \\
1 \\
\end{bmatrix}
=
\begin{bmatrix}
1 & sh_x^y & sh_x^z & 0 \\
sh_y^x & 1 & sh_y^z & 0 \\
sh_z^x & sh_z^y & 1 & 0 \\
0 & 0 & 0 & 1 \\
\end{bmatrix}
\begin{bmatrix}
x \\
y \\
z \\
1 \\
\end{bmatrix}
$$
Left handed:
$$
\begin{bmatrix}
x' \\
y' \\
z' \\
1 \\
\end{bmatrix}
=
\begin{bmatrix}
1 & sh_y^x & sh_z^x & 0 \\
sh_x^y & 1 & sh_z^y & 0 \\
sh_x^z & sh_y^z & 1 & 0 \\
0 & 0 & 0 & 1 \\
\end{bmatrix}
\begin{bmatrix}
x \\
y \\
z \\
1 \\
\end{bmatrix}
$$

## Reflection

Right handed reflection about x-axis:
$$
\begin{aligned}
x' &= x\\
y' &= -y\\
z' &= -z
\end{aligned}
\\
\begin{bmatrix}
1 & 0 & 0 & 0\\
0 & -1 & 0 & 0\\
0 & 0 & -1 & 0\\
0 & 0 & 0 & 1\\
\end{bmatrix}
$$
Right handed reflection about y-axis:
$$
\begin{aligned}
x' &= y\\
y' &= -x\\
z' &= -z
\end{aligned}
\\
\begin{bmatrix}
-1 & 0 & 0 & 0\\
0 & 1 & 0 & 0\\
0 & 0 & -1 & 0\\
0 & 0 & 0 & 1\\
\end{bmatrix}
$$
Right handed reflection about z-axis:
$$
\begin{aligned}
x' &= -x\\
y' &= -y\\
z' &= z
\end{aligned}
\\
\begin{bmatrix}
-1 & 0 & 0 & 0\\
0 & -1 & 0 & 0\\
0 & 0 & 1 & 0\\
0 & 0 & 0 & 1\\
\end{bmatrix}
$$
