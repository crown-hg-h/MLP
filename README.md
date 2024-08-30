# MLP

这个教程介绍了如何有效地将描述符作为输入，用于预测能量和力的机器学习模型。在构建机器学习力场时，你需要做出几个设计选择，例如选择哪个机器学习模型、哪个描述符等。在本教程中，我们将使用以下非常简单的设置：

->一个包含两个通过伦纳德-琼斯势相互作用的原子的数据库。这是尽可能简单的情况。真实的系统会复杂得多，因此需要更复杂的机器学习模型和更长的训练时间。
->直接计算两个原子之间的 SOAP 描述符。这是为了简化问题，在实际系统中，你会有更多的中心，可能位于每个原子上方。
->我们将使用一个全连接的神经网络来进行预测。原则上，任何机器学习方法都可以，但神经网络可以非常方便地计算输出相对于输入的解析导数。这使我们能够训练一个能量预测模型，只要我们知道描述符对原子位置的导数，我们就可以自动获得力。



##数据集与模型简介

![image](https://github.com/user-attachments/assets/618b5729-644f-446b-92e7-9bf3cb40f964)
