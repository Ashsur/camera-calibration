# 2 BASIC EQUATION(基础方程)
我们*将*审查(examine)通过观测单个平面得到的相机内部参数的约束。 我们从本文使用的表示法(notation)开始。

## 2.1 Notation(表示法)
二维点 $$m=\begin{bmatrix}u&v\end{bmatrix}^T$$
三维点 $$M=\begin{bmatrix}X&Y&Z\end{bmatrix}^T$$
扩展向量 $$\widetilde{m}=\begin{bmatrix}u&v&1\end{bmatrix}^T$$
$$\widetilde{M}=\begin{bmatrix}X&Y&Z&1\end{bmatrix}^T$$

相机由(usual)针孔模型建模：三维点M和投影点m的关系为$$s\widetilde{m}=A\begin{bmatrix}R&t\end{bmatrix}\widetilde{M}$$$$A=\begin{bmatrix}\alpha&\gamma&u_0\\0&\beta&v_0\\0&0&1\end{bmatrix}\tag{1}$$s是任意的比例因子，(R,t)是将世界坐标系与相机坐标系关联起来的旋转和平移矩阵，A被称为相机固有(intrinsic)矩阵，其中$(u_0,v_0)$是坐标原点，是像平面在u轴和v轴的缩放因子，γ描述了两个像平面轴夹角(skew)。我们使用缩写$A^{-T}$表示$(A^{-1})^T$或$(A^T)^{-1}$。

## 2.2 Homography between the Model Plane and Its Image(模板平面和像面的单应性变换)
不失一般性的，我们假设模板平面在世界坐标系Z=0的平面上。用$r_i$来表示旋转矩阵R的第i列。因此由式(1)我们可以得到$$s\begin{bmatrix} u\\  v\\  1 \end{bmatrix} =A\begin{bmatrix} r_1 & r_2 & r_3 & t \end{bmatrix}\begin{bmatrix} X\\  Y\\  0\\  1 \end{bmatrix}=A\begin{bmatrix} r_1 & r_2 & t \end{bmatrix}\begin{bmatrix} X\\  Y\\  1 \end{bmatrix} $$
*By abuse of notation*与notation中不同的是，我们仍然使用M来表示模板平面的一个点，但是此时$M=\begin{bmatrix}
X&Y 
\end{bmatrix}^T$，这是因为Z永远为0。反过来，$\widetilde{M}=\begin{bmatrix}
X&Y&1 
\end{bmatrix}^T$。因此，一个模板上的点M和它成像的点m的关系通过单应性矩阵H表示如下：$$s\widetilde{m}=H\widetilde{M}$$ $$H=A\begin{bmatrix}
r_1 & r_2 & t 
\end{bmatrix} \tag{2}$$
正如上式表示的那样，3*3的矩阵H被定义为一个缩放因子。

## 2.3 Constraints on the Intrinsic Parameters(相机内参的约束)
一旦给定一个模板的成像投影(image)，则可以估计出单应性变换(见附录)。我们用$H=\begin{bmatrix}
h_1 & h_2 & h_3 
\end{bmatrix}$表示单应性变换，则由(2)式可以得到$$\begin{bmatrix}
h_1 & h_2 & h_3 
\end{bmatrix}==\lambda 
\begin{bmatrix}
r_1 & r_2 &t 
\end{bmatrix},$$
其中λ是任意的缩放因子。由先验知识可知$r_1$和$r_2$是正交的,因此我们可以进一步得到$$h_1^TA^{-T}A^{-1}h_2=0\tag{3}$$$$h_1^TA^{-T}A^{-1}h_1=h_2^TA^{-T}A^{-1}h_2\tag{4}$$这是由一个单应性变换得到的相机内参的两个基本约束。由于一个单应性变换由八个自由度，并且存在六个外部参数(三个旋转、三个平移)，因此我们只能得到两个相机内部参数约束方程。注意到$A^{-T}A^{-1}$实际上描述了图像(image)的**绝对圆锥曲线**(absolute conic)。在下一节我们将给出基于几何的解释。

## 2.4 Geometric Interpretation(几何解释)
我们现在将(3)(4)与绝对圆锥曲线**[16][15]**联系起来。
按照惯例(under our convention)不难证明模板平面在相机坐标系下可以用以下方程描述：$$\begin{bmatrix}
r_3\\ 
r_3^Tt
\end{bmatrix}^T\begin{bmatrix}
x\\ 
y\\ 
z\\ 
w
\end{bmatrix}=0,$$其中平面上无穷远的点对应$w=0$，其余点对应$w=1$。
