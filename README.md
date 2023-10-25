# CMU10-708-PGM
 My codes and notes for CMU10-708 "Probabilistic Graphical Models" (ver 2019 Spring)

该仓库包含了我在CMU10-708公开课程（2019年春季学期版本）中的作业代码和笔记，以及一些课程相关的资料。

由于本人专业与研究方向所限，该课程的中后期部分可能不会过多关注，该仓库很可能无法包含该课程的全部内容、习题和作业代码，请谅解。
## Course Info
Website(2019 Spring): https://sailinglab.github.io/pgm-spring-2019/


## Course Modules
The origin schedule(English) is shown in: https://sailinglab.github.io/pgm-spring-2019/lectures/

分为六个模块
- 第1-10节：概论、表示方法、精确推断
- 第11-14节：近似推断
- 第15-19节：深度学习和生成式模型
- 第20-21节：强化学习和基于图模型推断的控制
- 第22-26节：非参数方法
- 第27-28节：模块化和可扩展的算法和系统

| No. | 章节内容                                             |
| --- | ---------------------------------------------------- |
| 1   | 概率图介绍                                           |
| 2   | 表示：有向图模型（贝叶斯网络）                       |
| 3   | 表示：无向图模型 （马尔科夫随机场）                  |
| 4   | 精确推断方法                                         |
| 5   | 完全可观测贝叶斯网络的参数学习                       |
| 6   | 部分可观测贝叶斯网络的参数学习                       |
| 7   | 无向图模型的最大似然学习                             |
| 8   | （Guest Lecture）因果发现和推断                      |
| 9   | 建立网络模型                                         |
| 10  | 序贯模型（HMM）                                      |
| 11  | 近似推断：平均场（MF）和环信念传播（BF）近似方法     |
| 12  | 变分推断理论：内部逼近和外部逼近                     |
| 13  | 近似推断：蒙特卡洛和序贯蒙特卡洛方法                 |
| 14  | 马尔科夫链蒙特卡洛（MCMC）                           |
| 15  | 深度学习的统计学和算法基础                           |
| 16  | 搭建深度学习的基本模块                               |
| 17  | 深度生成式模型（DGM）：理论基础总结，DGM模型间的联系 |
| 18  | 深度生成式模型：第二部分（如GAN）                    |
| 19  | 案例研究：文本生成                                   |
| 20  | 序贯决策：第一部分（框架）                           |
| 21  | 序贯决策：第二部分（算法）                           |
| 22  | 贝叶斯非参数方法                                     |
| 23  | 贝叶斯非参数方法（续）                               |
| 24  | 图模型的综合范式：正则化贝叶斯方法                   |
| 25  | 基于谱和基于核函数的图模型元素                       |
| 26  | 高斯过程（GP）和元学习（meta-learning）的要素        |
| 27  | 用于学习、推断和预测的可扩展算法与系统               |
| 28  | 从工程角度看AI                                       |

## Homeworks
Homeworks: https://sailinglab.github.io/pgm-spring-2019/homework/

- HW1: BN、MRF、Exact Inference、Parameter Learning
- HW1_code: Junction Tree, Baum-Welch Algorithm
- HW2: Sequence Model、Variational Inference 、Importance Sampling、MCMC
- HW2_code: HMM
- HW3: Variational Autoencoder, GAN
- HW3_colab: Wake-sleep algorithm, VAE
- HW4: Reinforcement Learning, Bayesian Nonparametrics
- HW4_colab: RL as Probabilistic Inference