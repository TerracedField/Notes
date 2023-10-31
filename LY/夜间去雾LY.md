# Single nighttime image dehazing based on unified variational decomposition model and multi-scale contrast enhancement

## abstract

用XX模型将有雾图像分成结构层，细节层，噪音层

具体地，采用𝓁1范数对结构分量进行约束，采用𝓁0稀疏项对细节层与修正层之间的梯度残差进行分段连续

利用Frobenius范数估计噪声层。

然后雾就被物理模型去除了，并且纹理层的细节被加强同时噪声的放大也被抑制了

## introduction

夜间有雾场景下获取的户外图像或视频通常会受到雾霾干扰、纹理模糊、发光效应、颜色失真、噪声干扰等更为复杂的退化因素的影响