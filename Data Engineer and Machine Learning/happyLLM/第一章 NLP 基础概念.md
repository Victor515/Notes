## NLP 任务

中文分词（Chinese Word Segmentation, CWS）其目的是将连续的中文文本切分成有意义的词汇序列

子词切分（Subword Segmentation）是 NLP 领域中的一种常见的文本预处理技术，旨在将词汇进一步分解为更小的单位，即**子词**。

词性标注（Part-of-Speech Tagging，POS Tagging）为文本中的每个单词分配一个词性标签，如名词、动词、形容词等

- 基于深度学习的循环神经网络 RNN 和长短时记忆网络 LSTM

文本分类（Text Classification）是 NLP 领域的一项核心任务，涉及到将给定的文本自动分配到一个或多个预定义的类别中

- 使用神经网络进行文本分类已经成为一种趋势

实体识别（Named Entity Recognition, NER）在自动识别文本中具有特定意义的实体，并将它们分类为预定义的类别，如人名、地点、组织、日期、时间等

关系抽取（Relation Extraction）它的目标是从文本中识别实体之间的语义关系

文本摘要（Text Summarization）文本摘要可以分为两大类：抽取式摘要（Extractive Summarization）和生成式摘要（Abstractive Summarization）

机器翻译（Machine Translation, MT）是 NLP 领域的一项核心任务，指使用计算机程序将一种自然语言（源语言）自动翻译成另一种自然语言（目标语言）的过程。

自动问答（Automatic Question Answering, QA）是 NLP 领域中的一个高级任务，旨在使计算机能够**理解自然语言提出的问题**，并根据给定的数据源自动提供准确的答案。检索式问答（Retrieval-based QA）、知识库问答（Knowledge-based QA）和社区问答（Community-based QA）



## 文本表示的发展历程

### 词向量

向量空间模型（Vector Space Model, VSM）通过将文本（包括单词、句子、段落或整个文档）转换为高维空间中的向量来实现文本的数学化表示

向量空间模型的应用极其广泛，包括但不限于文本相似度计算、文本分类、信息检索等自然语言处理任务

### 语言模型

N-gram模型的核心思想是基于马尔可夫假设，即一个词的出现概率仅依赖于它前面的N-1个词。

N-gram模型通过条件概率链式规则来估计整个句子的概率

### Word2Vec

Word2Vec是一种流行的词嵌入（Word Embedding）技术，由Tomas Mikolov等人在2013年提出。

Word2Vec的核心思想是利用词在文本中的上下文信息来捕捉词之间的语义关系，从而使得语义相似或相关的词在向量空间中距离较近。

相比于传统的高维稀疏表示（如One-Hot编码），Word2Vec生成的是**低维（通常几百维）的密集向量**

连续词袋模型CBOW(Continuous Bag of Words)是根据目标词上下文中的词对应的词向量, 计算并输出目标词的向量表示；Skip-Gram模型与CBOW模型相反, 是利用目标词的向量表示计算上下文中的词向量. 

### ELMo

ELMo（Embeddings from Language Models）实现了一词多义、静态词向量到动态词向量的跨越式转变

首先在大型语料库上训练语言模型，得到词向量模型，然后**在特定任务上对模型进行微调**，得到更适合该任务的词向量

预训练思想