# 2 BASIC EQUATION(基础方程)
我们*将*审查(examine)通过观测单个平面得到的相机内部参数的约束。 我们从本文使用的表示法(notation)开始。

## 2.1 Notation(表示法)
二维点 $$m=\begin{bmatrix}u&v\end{bmatrix}^T$$
三维点 $$M=\begin{bmatrix}X&Y&Z\end{bmatrix}^T$$
扩展向量 $$\widetilde{m}=\begin{bmatrix}u&v&1\end{bmatrix}^T$$
$$\widetilde{M}=\begin{bmatrix}X&Y&Z&1\end{bmatrix}^T$$

相机由(usual)针孔模型建模：三维点M和投影点m的关系为$s\widetilde{m}=A\begin{bmatrix}R&t\end{bmatrix}\widetilde{M}$$$A=\begin{bmatrix}\alpha&\gamma&u_0\\0&\beta&v_0\\0&0&1\end{bmatrix}\tag{1}$$s是任意的比例因子，(R,t)是将世界坐标系与相机坐标系关联起来的旋转和平移矩阵，A被称为相机固有(intrinsic)矩阵，其中$(u\_0,v\_0)$是坐标原点，是像平面在u轴和v轴的缩放因子，γ描述了两个像平面轴夹角(skew)。我们使用缩写$A^{-T}$表示$(A^{-1})^T$或$(A^T)^{-1}$。

## 2.2 Homography between the Model Plane and Its Image(模板平面和像面的单应性变换)
不失一般性的，我们假设标定模型平面的世界坐标系坐标Z=0。用$r\_i$来表示旋转矩阵R的第i列。因此由式(1)我们可以得到$$s\begin{bmatrix} u\\  v\\  1 \end{bmatrix} =A\begin{bmatrix} r\_1 & r\_2 & r\_3 & t \end{bmatrix}\begin{bmatrix} X\\  Y\\  0\\  1 \end{bmatrix}=A\begin{bmatrix} r\_1 & r\_2 & t \end{bmatrix}\begin{bmatrix} X\\  Y\\  1 \end{bmatrix} $$
*By abuse of notation*与notation中不同的是，我们仍然使用M来表示标定模型平面的一个点，但是此时$M=\begin{bmatrix}
X&Y 
\end{bmatrix}^T$，这是因为Z永远为0。反过来，$\widetilde{M}=\begin{bmatrix}
X&Y&1 
\end{bmatrix}^T$。因此，一个标定模型上的点M和它成像的点m的关系通过单应性矩阵H表示如下：$$s\widetilde{m}=H\widetilde{M}$$ $$H=A\begin{bmatrix}
r\_1 & r\_2 & t 
\end{bmatrix} \tag{2}$$

## 2.3 Constraints on the Intrinsic Parameters(相机内参的约束)

## 2.4 Geometric Interpretation(几何解释)

