# 2 BASIC EQUATION(基础方程)
我们*将*审查(examine)通过观测单个平面得到的相机内部参数的约束。 我们从本文使用的表示法(notation)开始。

## 2.1 Notation(表示法)
二维点 $$m=\begin{bmatrix}u&v\end{bmatrix}^T$$
三维点 $$M=\begin{bmatrix}X&Y&Z\end{bmatrix}^T$$
扩展向量  $\widetilde{m}=\begin{bmatrix}u&v&1\end{bmatrix}^T$
$\widetilde{M}=\begin{bmatrix}X&Y&Z&1\end{bmatrix}^T$
相机由(usual)针孔模型建模：三维点M和投影点m的关系为$s\widetilde{m}=A\begin{bmatrix}R&t\end{bmatrix}\widetilde{M} $ ,with $$A=\begin{bmatrix}\alpha&\gamma&u_0\\0&\beta&v_0\\0&0&1\end{bmatrix}$$**(1)**

## 2.2 Homography between the Model Plane and Its Image(模板平面和像面的单应性变换)

## 2.3 Constraints on the Intrinsic Parameters(相机内参的约束)

## 2.4 Geometric Interpretation(几何解释)
