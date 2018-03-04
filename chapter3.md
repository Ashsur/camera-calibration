# 3 SOLVING CAMERA CALIBRATION(相机标定处理)

该部分提供了如何高效的完成相机标定的具体操作细节。我们从一个解析解(analytial solution)开始，然后是基于最大似然准则的非线性优化技术。最后，我们考虑镜头失真，同时提供分析和非线性的解决方案。

---
##3.1 Closed-Form Solution(封闭形式的解决方案)
$$B=A^{-T}A^{-1}\equiv \begin{bmatrix}
B_{11} &B_{12}  &B_{13} \\ 
B_{21} &B_{22}  &B_{23} \\ 
B_{31} &B_{32}  &B_{33} 
\end{bmatrix}\\
=\begin{bmatrix}
 \frac{1}{\alpha^2}&  \frac{\gamma }{\alpha^2\beta }& \frac{v_0\gamma -u_0\beta}{\alpha^2\beta}\\ 
 -\frac{\gamma}{\alpha^2\beta}&  \frac{\gamma^2}{\alpha^2\beta^2}+\frac{1}{\beta^2}& -\frac{\gamma(v_0\gamma-u_0\beta)}{\alpha^2\beta^2}-\frac{v_0}{\beta^2}\\ 
\frac{v_0\gamma-u_0\beta}{\alpha^2\beta} & -\frac{\gamma(v_0\gamma-u_0\beta))}{\alpha^2\beta^2}-\frac{v_0}{\beta^2} & \frac{(v_0\gamma-u_0\beta)^2}{\alpha^2\beta^2}+\frac{v_0^2}{\beta^2}+1
\end{bmatrix}\tag5$$
注意到B是一个对称阵，由一个六维向量**b**确定$$b=\begin{bmatrix}
B_{11} &B_{12}  &B_{22}  &B_{13}  &B_{23}  &B_{33} 
\end{bmatrix}^T\tag6$$
用$h_i=\begin{bmatrix}
h_{i1} &h_{i2}  &h_{i3} 
\end{bmatrix}^T$表示矩阵**H**的第i列，则有$$h_i^TBh_j=v_{ij}^Tb \tag7$$其中$$v_{ij}=\begin{bmatrix}
h_{i1}h_{j1} &  h_{i1}h_{j2}+h_{i2}h_{j1}&h_{i2}h_{j2} &h_{i3}h_{j1}+h_{i1}h_{j3} &h_{i3}h_{j2}+h_{i2}h_{j3} &h_{i3}h_{j3} 
\end{bmatrix}^T$$因此，由给定的单应性变换(homography)，两个基本约束**(3)(4)**可以重写成用b来表示的等价方程(homogeneous equation)$$\begin{bmatrix}
v_{12}^T\\ 
(v_{11}-v_{12})^T
\end{bmatrix}b=0 \tag8$$
如果观测到n幅模板图像，则可以得到n个**(8)**中的方程，*用向量的形式可以表示为*$$Vb=0\tag9$$
上式中**V**是一个2n×6的矩阵。如果n≥3，我们将得到一个通常意义上的特解并将其定义为比例因子b(*译者注：原文为b defined up to a scale factor*)。如果n=2,我们可以施加不对称约束使γ=0，例如将[0,1,0,0,0,0]b=0,作为附加的方程添加至方程(9)中。（如果n=1,我们只能通过假设$u_0$，$v_0$已知以及$\gamma=0$——这正如我们在**[19]**中基于手眼共面的合理假设来为了头部姿势确定(head pose determination)所做的那样，得到相机的两个内部参数，例如α和β。事实上，**Tsai[23]**已经提到focal  length  from  one  plane  is possible, but incorrectly says that aspect ratio is not.）
求解方程(9)的方法是我们熟知的求解$V^TV$与最小特征值对应的的特征向量（等价的，求解与V最小特征值有关的右奇异向量(*right singular*)）。
一旦b被估计后(estimated)，我们可以通过以下方法得到相机的所有内部参数。3.1小节中所描述的矩阵B被估计时加上了一个比例因子(up to a scale factor)，例如$B=\lambda A^{-T}A$，其中λ是一个任意的比例因子。我们可以容易的从矩阵B中分离出每一个内部参数(uniquely extract the intrinsic parameter)
$$v_0=(B_{12}B_(13)-B_{11}B_{23})/(B_{11}B_{22}-B_{12}^2)$$
$$\lambda=B_{33}-[B_{13}^2+v_0(B_{12}B_{13}-B_{11}B_{23})]/B_{11}$$
$$\alpha=\sqrt{\lambda/B_{11}}$$
$$\beta=\sqrt{\lambda B_{11}/(B_{11}B_{12}-B_{12}^2)}$$
$$\gamma=-B_{12}\alpha^2\beta/\lambda$$
$$u_0=\gamma v_0/\alpha-B_{13}\alpha^2/\lambda$$
一旦A已知，由（2）式，每幅图像对应的外部参数可由以下方程求解：$$r_1=\lambda A^{-1}h_1,r_2=\lambda A^{-1}h_2,r_3=r_1×r_2，t=\lambda A^{-1}h_3$$
其中，$\lambda=1/\left \| A^{-1}h_1 \right \|=1/\left \| A^{-1}h_2 \right \|$。诚然，通常这样计算得到的矩阵R因为数据噪声的存在而不满足旋转矩阵的性质(properties),旋转矩阵最好通过奇异值分解的方法来获得**[10],[26]**。

---
##3.2 Maximum-Likehood Estimate(极大似然估计)
上面的解决方案是通过使代数距离最小化而获得的，而这在物理意义上是无意义的(is not physical meaningful)。 我们可以通过最大似然准则来推断来改进它。
我们给出n张模板——模板上有m个点的图像。假设图像上的点被独立且同分布的噪声所干扰(be corrupted)，则可以通过下面的方程(function)的到最大似然估计(maximum-likehood estimate):$$\sum_{i=1}^{n}\sum_{j=1}^{m}\left \| m_{ij}-\hat{m}(A,R_i,t_i,M_j) \right \|^2 \tag{10}$$其中，$\hat{m}(A,R_i,t_i,M_j)$ 是世界坐标下的点$M_j$在图像i上的投影。旋转矩阵R被一个含三个参数的矢量参数化(parameterized)。R和r通过罗德里格斯公式(**Rodrigues formula[5]**)关联起来。最小化式(10)是一个非线性最小化问题，这可以用**Minpack[17]**中实现的Levenberg-Marquardt算法来解决。这个算法需要通过前面章节提到的技术来初始化$A,\left \{ R_i,t_i|i=1..n \right \}$。
桌面相机(desktop cameras)通常会产生肉眼可见的畸变，特别是径向上的畸变(radial components)。我们已经在最小化式(10)时考虑到这一点。具体的技术细节可以参考**[26]**。

*(译者注：MINPACK is a library of FORTRAN subroutines for the solving of systems of nonlinear equations, or the least squares minimization of the residual of a set of linear or nonlinear equations.)*

---
##3.3 Summary(总结)
推荐的矫正过程如下：
1. 打印模板并粘贴到平板表面。
2. 通过移动模板或相机拍摄一些不同角度的模板照片。
3. 检测图像中的模板特征点。
4. 通过3.1节的(close-foem solution)估计五个内部参数和所有的外部参数。
5. 通过最小化式(10)进一步精确(refine)所有参数（考虑畸变）。
当模板平面相互平行时，我的技术(technique)会进行退化配置。参照技术文档**[26]**获得更多细节描述。
