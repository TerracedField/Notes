# Basic Information:

- Title: Node-wise Difusion for Scalable Graph Learning (可扩展图学习的节点级扩散)
- Authors: Keke Huang, Jing Tang, Juncheng Liu, Renchi Yang, Xiaokui Xiao
- Affiliation: National University of Singapore, The Hong Kong University of Science and Technology (Guangzhou), Hong Kong Baptist University, CNRS@CREATE, Singapore
- Keywords: graph neural networks, scalability, semi-supervised classification
- URLs: [Paper](https://doi.org/10.1145/3543507.3583408), [GitHub: None]

# 论文简要 :

- 本文提出了一种可扩展的图学习方法，通过节点级扩散模型来捕捉每个节点的独特特征，从而生成高质量的节点表示。通过在半监督学习中应用该方法，设计了一个高效的图卷积网络模型，并在各种类型的网络数据集上进行了广泛的实验验证。

# 背景信息:

- 论文背景: 近年来，图神经网络在半监督学习中表现出卓越的性能，广泛应用于诸如网络服务和页面分类、在线社交网络分析和电子商务推荐等众多网络应用中。然而，现有的方法在扩散过程中没有区分节点的独特性，导致在模型训练中标记节点所占比例较小，并且不同节点位于不同的图局部上下文中，如果在扩散过程中不加以区分，会降低表示质量。
- 过去方案: 过去的方法中，Graph Convolutional Network (GCN) 是最早用于半监督分类的图神经网络模型，但由于其递归执行特征传播和转换，并且以全批量方式训练，因此存在严重的可扩展性问题。为了解决这个问题，已经提出了许多采样和预处理技术，以及解耦特征传播和转换的方法。
- 论文的Motivation: 然而，现有的方法要么计算大量未标记节点的节点表示，要么在表示生成过程中忽略每个标记节点的独特拓扑结构。因此，在效率和效果方面仍有改进的空间。为了解决这个问题，本文提出了一种节点级扩散模型，通过定制每个节点的独特扩散方案，捕捉节点的拓扑特征，并设计了一个基于此模型的高效图卷积网络模型。通过在各种真实世界的网络数据集上进行实验，验证了该模型在半监督分类中的卓越性能和可扩展性。

# 方法:

- a. 理论背景:
  - 本文介绍了图神经网络（GNNs）在各种网络应用中的日益流行，如分类、社交网络分析和推荐系统。本文的重点是半监督分类，由于标记数据的有限性，这是一个具有挑战性的问题。本文提到了图卷积网络（GCN）作为半监督分类的一种流行的GNN模型，但指出了其可扩展性问题。本文旨在通过提出一种节点级扩散模型（NDM）和一种可扩展的GNN模型NIGCN来解决当前模型的效率和有效性限制。
- b. 技术路线:
  - 本文介绍了GCN模型在可扩展性方面的局限性，并提出了两个改进方向：解耦模型和基于采样的模型。解耦模型，如SGC和APPNP，旨在简化特征聚合并考虑节点接近性。基于采样的模型，如GraphSAGE和FastGCN，利用采样和预处理技术来提高效率。本文提到了几种其他解耦模型，并强调了NDM和NIGCN在解决现有模型限制方面的贡献。

# 结果:

- a. 详细的实验设置:
  - 本文未提供有关实验设置的信息。
- b. 详细的实验结果:
  - 本文未提供有关实验结果的信息。

# Note:

- 本总结源自于LLM的总结，请注意数据判别. Power by ChatPaper. End.****