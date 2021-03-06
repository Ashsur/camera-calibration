# 1 MOTIVATIONS
相机校准是三维计算机视觉中的一个必要步骤，以从2D图像中提取度量信息。 许多工作已经完成，从摄影测量界开始（参见[2]，[4]引用一些），最近在计算机视觉（[9]，[8]，[23]，[7]，[ 25]，[24]，[16]，[6]举几个例子）。 我们可以将这些技术大致分为两类：摄影测量校准和自我校准。

- 基于三维参照物的校准。
摄像机通过观察一个三维空间中的具有高精度并且已知几何形状的校准参照物(calibration object)来执行校准。校准可以高效地完成[5]。 校准参照物通常由两个或三个彼此正交的平面组成。 有时也会使用经过精确测量(translation )的平面[23]。 这些方法需要昂贵的校准设备和精密的操作(setup)。

- 自动校准(self-calibration)。
该类别的技术不使用任何校准参照物。 仅仅通过在静态场景(static scene)中移动摄像机，场景的刚性(rigidity of the scene)一般通过单独使用图像信息就可以从一个摄像机位移提供摄像机内部参数的两个约束[16]，[14]。 因此，如果图像是由具有固定内部参数的相同摄像机拍摄的，则三幅图像之间的对应关系足以恢复内部和外部参数，这使得我们能够重建三维结构以达到相似性(up to a similarity )[15]，[12]。 虽然这种方法非常灵活，但还不成熟[1]。 由于需要估计的参数很多，我们不能获持续得可靠的结果。

其他技术：正交方向的消失点[3]，[13]以及纯旋转的校准[11]，[20]。

　　我们目前的研究集中在桌面视觉系统（DVS）上，因为使用DVS的潜力很大。 相机正在变得便宜和无处不在。 DVS针对的是不精通(no experts)计算机视觉的普通大众。 通常计算机用户只会不时地(time to time)执行视觉任务，所以他们不愿意为昂贵的设备投资。 因此，灵活性，稳健性和低成本显得非常重要。本文中描述的相机校准技术是在考虑到这些考虑因素的情况下开发的。
　　所提出的技术仅需要照相机观测少数（至少两个）不同视角的平面图案。该图案可以由激光打印机打印并粘贴到“合理的”平面表面（例如，硬书本封面）。相机或平面图案可以手动移动并且不需要知道。本文所提出的使用2D度量信息的方法 介于(lies between)使用显式3D模型的**摄影测量校准**和使用运动刚度或等效隐式3D信息的**自校准**之间。本文所提出的技术在计算机仿真和真实数据测试中已经获得非常好的结果。与传统技术相比，本文所提出的技术相当灵活：任何人都可以自己制作标定板(calibration pattern)，并且设置非常简单。与自校准相比，它具有可观的(considerable degree)稳健性。我们相信新技术将进一步推进三维计算机视觉的应用从实验室环境过渡至现实世界。
　　需要注意到 Triggs [22]最近利用(译者自添)不少于五个视图的平面场景(plannar scene)开发了一种自校准技术。 他的技术比我们的技术更灵活，**但很难初始化**。 Liebowitz和Zisserman [13]描述了使用度量信息（例如已知角度，两个相等但未知的角度和已知的长度比）来度量校正(metric rectification)平面透视图像的技术。 他们还提到，相机内部参数是可能通过提供不少于三个这样的整流平面(rectified planes)来的校准的，尽管没有显示实验结果。
　　在本文的修订过程中，我们注意到**Sturm**和**Maybank**发表了一个独立但相似的工作[21]。他们使用简化的相机模型（图像轴彼此正交(orthogonal)），并详细研究了一个和两个平面情况下的退化配置(degenerate configurations) ，如果只有一个或两个视图用于相机校准，这在实践中非常重要。（*这里的语序可以考虑替换*）
　　本文组织如下：第2节描述了观察单个平面的基本约束。 第3节描述了校准程序。 我们从封闭式解决方案开始(closed-form solution)，然后进行非线性优化。 径向透镜畸变也被建模。 第4节提供了实验结果。 计算机仿真和实际数据都被用来验证所提出的技术。 在附录中，我们提供了许多细节，包括估计模型平面与其图像之间单应性(homography)的技术。

*译者注：单应性变换就是指一个平面到另一个平面的映射关系*