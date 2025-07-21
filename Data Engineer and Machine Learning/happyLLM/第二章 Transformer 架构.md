从 计算机视觉（Computer Vision，CV）为起源发展起来的神经网络，其核心架构有三种

前馈神经网络（Feedforward Neural Network，FNN），即每一层的神经元都和上下两层的每一个神经元完全连接

卷积神经网络（Convolutional Neural Network，CNN），即**训练参数量远小于前馈神经网络的卷积层**来进行特征提取和学习

循环神经网络（Recurrent Neural Network，RNN），能够使用历史信息作为输入、包含环和自重复的网络



由于 NLP 任务所需要处理的文本往往是序列，因此专用于处理序列、时序数据的 RNN 往往能够在 NLP 任务上取得最优的效果

在注意力机制横空出世之前，RNN 以及 RNN 的衍生架构 **LSTM** 是 NLP 领域当之无愧的霸主

两个难以弥补的缺陷：

1. 序列依序计算的模式能够很好地模拟时序信息，但**限制了计算机并行计算的能力**。由于序列需要依次输入、依序计算，图形处理器（Graphics Processing Unit，GPU）并行计算的能力受到了极大限制，导致 RNN 为基础架构的模型虽然参数量不算特别大，但**计算时间成本却很高**；
2. RNN 难以捕捉**长序列**的相关关系。在 RNN 架构中，距离越远的输入之间的关系就越难被捕捉，同时 RNN 需要将整个序列读入内存依次计算，也限制了序列的长度。虽然 LSTM 中通过门机制对此进行了一定优化，但对于较远距离相关关系的捕捉，RNN 依旧是不如人意的。

Vaswani 等学者参考了在 CV 领域被提出、被经常融入到 RNN 中使用的**注意力机制**（Attention）（注意，虽然注意力机制在 NLP 被发扬光大，但其确实是在 CV 领域被提出的），创新性地搭建了完全由注意力机制构成的神经网络——Transformer

注意力机制有三个核心变量：**Query**（查询值）、**Key**（键值）和 **Value**（真值）



### 深入理解注意力机制

给不同 Key 所赋予的不同权重，就是我们所说的注意力分数，也就是为了查询到 Query，我们应该赋予给每一个 Key 多少注意力

Q：如何能够找到一个合理的、能够计算出正确的注意力分数的方法呢

词向量能够表征语义信息，从而让语义相近的词在向量空间中距离更近

根据词向量的定义，语义相似的两个词对应的词向量的点积应该大于0，而语义不相似的词向量点积应该小于0。

得到的向量就能够反映 Query 和每一个 Key 的相似程度，同时又相加权重为 1，也就是我们的注意力分数了

```python
'''注意力计算函数'''
def attention(query, key, value, dropout=None):
    '''
    args:
    query: 查询值矩阵
    key: 键值矩阵
    value: 真值矩阵
    '''
    # 获取键向量的维度，键向量的维度和值向量的维度相同
    d_k = query.size(-1) 
    # 计算Q与K的内积并除以根号dk
    # transpose——相当于转置
    # -2, -1 表示转换最后两个维度
    scores = torch.matmul(query, key.transpose(-2, -1)) / math.sqrt(d_k)
    # Softmax
    p_attn = scores.softmax(dim=-1)
    if dropout is not None:
        p_attn = dropout(p_attn)
        # 采样
     # 根据计算结果对value进行加权求和
    return torch.matmul(p_attn, value), p_attn
```

### 自注意力

在 Transformer 的 Encoder 结构中，使用的是 注意力机制的变种 —— 自注意力（self-attention，自注意力）机制

计算本身序列中每个元素对其他元素的注意力分布，即在计算过程中，Q、K、V 都由**同一个输入通过不同的参数**矩阵计算得到

通过自注意力机制，我们可以找到一段文本中每一个 token 与其他所有 token 的相关关系大小，从而建模文本之间的依赖关系

### 掩码自注意力

Mask Self-Attention

掩码的作用是遮蔽一些特定位置的 token，模型在学习的过程中，会忽略掉被遮蔽的 token。

使用注意力掩码的核心动机是让模型只能使用历史信息进行预测**而不能看到未来信息**。

对一个文本序列，不断**根据之前的 token** 来预测下一个 token，直到将整个文本序列补全。

例如，我们待学习的文本序列仍然是 【BOS】I like you【EOS】，我们使用的注意力掩码是【MASK】，那么模型的输入为：

```
<BOS> 【MASK】【MASK】【MASK】【MASK】
<BOS>    I   【MASK】 【MASK】【MASK】
<BOS>    I     like  【MASK】【MASK】
<BOS>    I     like    you  【MASK】
<BOS>    I     like    you   </EOS>
```

上述输入不再是串行的过程，而可以一起**并行地**输入到模型中，模型只需要每一个样本根据未被遮蔽的 token 来预测下一个 token 即可

### 多头注意力

多头注意力机制（Multi-Head Attention）

同时对一个语料进行多次注意力计算，每次注意力计算都能拟合不同的关系，将最后的多次结果拼接起来作为最后的输出，即可更全面深入地拟合语言信息。



## Encoder-Decoder

在 Transformer 中，使用注意力机制的是其两个核心组件——Encoder（编码器）和 Decoder（解码器）

从 Transformer 所针对的 Seq2Seq 任务出发



**Seq2Seq**，即序列到序列，是一种经典 NLP 任务。具体而言，是指模型输入的是一个自然语言序列 input=(x1,x2,x3...xn) ，输出的是一个可能不等长的自然语言序列 output=(y1,y2,y3...ym) 

对于 Seq2Seq 任务，一般的思路是对自然语言序列进行编码再解码。

所谓编码，就是将输入的自然语言序列通过隐藏层编码成能够**表征语义的向量**（或矩阵），可以简单理解为更复杂的词向量表示。而解码，就是对输入的自然语言序列编码得到的向量或矩阵通过隐藏层输出，再解码成对应的自然语言目标序列。



Transformer 由 Encoder 和 Decoder 组成，每一个 Encoder（Decoder）又由 6个 Encoder（Decoder）Layer 组成。输入源序列会进入 Encoder 进行编码

next: Encoder 和 Decoder 内部传统神经网络的经典结构

### 前馈神经网络

Feed Forward Neural Network

```python
class MLP(nn.Module):
    '''前馈神经网络'''
    def __init__(self, dim: int, hidden_dim: int, dropout: float):
        super().__init__()
        # 定义第一层线性变换，从输入维度到隐藏维度
        self.w1 = nn.Linear(dim, hidden_dim, bias=False)
        # 定义第二层线性变换，从隐藏维度到输入维度
        self.w2 = nn.Linear(hidden_dim, dim, bias=False)
        # 定义dropout层，用于防止过拟合
        self.dropout = nn.Dropout(dropout)

    def forward(self, x):
        # 前向传播函数
        # 首先，输入x通过第一层线性变换和RELU激活函数
        # 然后，结果乘以输入x通过第三层线性变换的结果
        # 最后，通过第二层线性变换和dropout层
        return self.dropout(self.w2(F.relu(self.w1(x))))
```

Transformer 的前馈神经网络是由**两个线性层**中间加一个 **RELU 激活函数**组成的，以及前馈神经网络还加入了一个 Dropout 层来防止过拟合。

### 层归一化

神经网络主流的归一化一般有两种，批归一化（**Batch Norm**）和层归一化（**Layer Norm**）。

归一化核心是为了让不同层输入的取值范围或者分布能够比较一致。

批归一化存在一些缺陷，例如：

- 当显存有限，mini-batch 较小时，Batch Norm 取的样本的均值和方差不能反映全局的统计分布信息，从而导致效果变差；
- 对于在时间维度展开的 RNN，不同句子的同一分布大概率不同，所以 Batch Norm 的归一化会失去意义；
- 在训练时，Batch Norm 需要保存每个 step 的统计信息（均值和方差）。在测试时，由于变长句子的特性，测试集可能出现比训练集更长的句子，所以对于后面位置的 step，是没有训练的统计量使用的；
- 应用 Batch Norm，每个 step 都需要去保存和计算 batch 统计量，耗时又耗力

相较于 Batch Norm 在每一层统计所有样本的均值和方差，Layer Norm 在每个样本上计算其所有层的均值和方差，从而使每个样本的分布达到稳定。Layer Norm 的归一化方式其实和 Batch Norm 是完全一样的，只是**统计统计量的维度不同**。



### 残差连接 (residual connection)

为了**避免模型退化**，Transformer 采用了残差连接的思想来连接每一个子层。

残差连接，即下一层的输入不仅是上一层的输出，还包括上一层的输入。残差连接允许最底层信息直接传到最高层，让高层专注于残差的学习。

例如，在 Encoder 中，在第一个子层，输入进入多头自注意力层的同时会**直接传递到该层的输出**，然后该层的输出会与原输入相加，再进行标准化



### Encoder

Encoder 由 N 个 Encoder Layer 组成，每一个 Encoder Layer 包括一个注意力层和一个前馈神经网络

一个 Encoder，由 N 个 Encoder Layer 组成，在最后会加入一个 Layer Norm 实现规范化



### Decoder

和 Encoder 不同的是，Decoder 由两个注意力层和一个前馈神经网络组成

第一个注意力层是一个掩码自注意力层，即使用 Mask 的注意力计算，保证每一个 token 只能使用该 token 之前的注意力分数

第二个注意力层是一个多头注意力层，该层将使用第一个注意力层的输出作为 query，使用 Encoder 的输出作为 key 和 value，来计算注意力分数

最后，再经过前馈神经网络



## 搭建一个 Transformer

### Embedding 层

Embedding 层其实是一个存储固定大小的词典的嵌入向量查找表

而 Embedding 内部其实是一个可训练的（Vocab_size，embedding_dim）的权重矩阵，词表里的每一个值，都对应一行维度为 embedding_dim 的向量。对于输入的值，会对应到这个词向量，然后拼接成（batch_size，seq_len，embedding_dim）的矩阵输出。



### 位置编码

在注意力机制的计算过程中，对于序列中的每一个 token，其他各个位置对其来说都是平等的，即“我喜欢你”和“你喜欢我”在注意力机制看来是完全相同的，但无疑这是注意力机制存在的一个巨大问题。因此，**为使用序列顺序信息，保留序列中的相对位置信息**，Transformer 采用了位置编码机制

位置编码，即根据序列中 token 的相对位置对其进行编码，再将**位置编码加入词向量编码中**