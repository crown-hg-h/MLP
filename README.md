# MLP

这个教程介绍了如何有效地将描述符作为输入，用于预测能量和力的机器学习模型。在构建机器学习力场时，你需要做出几个设计选择，例如选择哪个机器学习模型、哪个描述符等。在本教程中，我们将使用以下非常简单的设置：

->一个包含两个通过伦纳德-琼斯势相互作用的原子的数据库。这是尽可能简单的情况。真实的系统会复杂得多，因此需要更复杂的机器学习模型和更长的训练时间。
->直接计算两个原子之间的 SOAP 描述符。这是为了简化问题，在实际系统中，你会有更多的中心，可能位于每个原子上方。
->我们将使用一个全连接的神经网络来进行预测。原则上，任何机器学习方法都可以，但神经网络可以非常方便地计算输出相对于输入的解析导数。这使我们能够训练一个能量预测模型，只要我们知道描述符对原子位置的导数，我们就可以自动获得力。



---

我们将使用包含特征向量 \(\mathbf{D}\)、它们的导数 \(\nabla_{\mathbf{r_i}} \mathbf{D}\) 以及关联的系统能量 \(E\) 和力 \(\mathbf{F}\) 的数据集进行训练。我们将使用一个神经网络 \(f\) 来预测能量：\(\hat{E} = f(\mathbf{D})\)。这里，带有“帽子”的变量表示预测的量，以区别于实际值。预测的力可以直接计算为相对于原子位置的负梯度。例如，原子 \(i\) 的力可以计算为（使用行向量）：

\[
\hat{\mathbf{F}}_i = - \nabla_{\mathbf{r_i}} f(\mathbf{D})
                  = - \nabla_{\mathbf{D}} f \cdot \nabla_{\mathbf{r_i}} \mathbf{D}
                  = - \begin{bmatrix}
                        \frac{\partial f}{\partial D_1} & \frac{\partial f}{\partial D_2} & \dots
                      \end{bmatrix}
                    \begin{bmatrix}
                        \frac{\partial D_1}{\partial x_i} & \frac{\partial D_1}{\partial y_i} & \frac{\partial D_1}{\partial z_i}\\
                        \frac{\partial D_2}{\partial x_i} & \frac{\partial D_2}{\partial y_i} & \frac{\partial D_2}{\partial z_i}\\
                        \vdots & \vdots & \vdots \\
                      \end{bmatrix}
\]

在这些方程中，\(\nabla_{\mathbf{D}} f\) 是机器学习模型输出相对于输入描述符的导数。正如前面提到的，神经网络通常可以解析地输出这些导数。 \(\nabla_{\mathbf{r_i}} \mathbf{D}\) 是描述符相对于原子位置的导数。DScribe 提供了这些 SOAP 描述符的导数。

神经网络的损失函数将包含能量和力的均方误差之和。为了更好地平衡这两种属性在损失函数中的贡献，它们的值会根据训练集中的方差进行缩放。

---
## 数据集与模型简介

![image](https://github.com/user-attachments/assets/618b5729-644f-446b-92e7-9bf3cb40f964)
