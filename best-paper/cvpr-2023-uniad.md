# CVPR 2023:UniAD

> 来源：[求求你们别学了](https://www.zhihu.com/people/qiu-qiu-ni-men-bie-xue-liao)@知乎
>
> 原文：[https://zhuanlan.zhihu.com/p/642373931](https://zhuanlan.zhihu.com/p/642373931)

### Abstract

* 现代自动驾驶系统通常以模块化任务的顺序进行，即感知、预测和规划。部署单独的模型来处理各个任务，或者设计具有独立头部的多任务范式。然而，这些方法可能会受到累积误差或任务协调不足的困扰。
* 一个理想的框架应该为追求自动驾驶汽车的终极目标（即规划）而精心设计和优化。因此，我们介绍了（UniAD），这是一个最新的综合框架，将全栈驾驶任务整合到一个网络中。它精心设计，以利用每个模块的优势，并从全局角度提供互补的特征抽象以进行物体交互。任务通过统一的查询接口进行通信，以相互促进规划。
* 我们在具有挑战性的nuScenes基准上实例化UniAD。通过广泛的消融研究，使用这种理念的有效性得到证明，因为在各个方面都大大超过了以前的最新水平。

### 核心：

* 多组查询向量的全 Transformer 模型：UniAD利用多组 query 实现了全栈 Transformer 的端到端模型，我们可以从具体 Transformer 的输入输出感受到信息融合。在 TrackFormer 中，Track query 通过与 BEV 特征通过 attention 的方式进行交互，输出特征 QA。类似的，Map query 经过 MapFormer 的更新后，得到特征 QM 。MotionFormer 使用 Motion query 与 QA 、 QM 以及 BEV 特征进行交互，得到未来轨迹以及特征 QX。OccFormer 以密集的 BEV 特征为 Q 和稀疏的特征 QA 对应的位置信息 PA和 QX作为 K和 V 来构建实例级别的占据栅格。
* 基于最终“规划”为目标: 在 TrackFormer 中，Track query 中包含一个特定的 ego-vehicle query 用来表示自车属性。规划模块 (Planner) 将 MotionFormer 更新后的 ego-vehicle query 与 BEV 特征进行交互，此时 ego-vehicle query 包含对整个环境的感知与预测信息，因此能更好的学习 planning 任务。为了减少碰撞，我们还利用占据栅格预测模块 OccFormer 的输出对自车路径进行优化，避免行驶到未来可能有物体占用的区域。在这个过程中，全部的模块通过输出特定的特征来帮助实现最终的目标“规划”。
