在基于深度学习的代码漏洞检测领域，提出一种基于多表征融合的代码漏洞检测方法。 为保证漏洞检测的粒度，选取函数而非整个程序作为基本分析单元。 首先，通过对函数的词法和语法解析，从中构建token序列，属性控制流图（ACFG）和抽象语法树，作为函数的三种不同的原始表征结构； 然后，分别利用针对序列的神经网络TextCNN，图注意力网络(GAT)和树状扩展递归神经网络(Extended RvNN) 从中抽取深层次的代码语义特征； 最后，考虑到token序列,ACFG和抽象语法树分别是从不同角度对同一函数的语义进行的互补性描述，因此通过直观的向量拼接操作，将从三者中抽取的特征向量进行有机融合，并送入分类层，实现函数级漏洞的检测。

词嵌入方式：Word2Vec

训练模型：TextCNN,GAT,Extended RvNN

开源的源码分析工具Joern（https://joern.readthedocs.io/en/latest/） 对函数级的C源代码进行分析，来提取属性控制流图。
通过调用python中pycparser模块将C函数解析为抽象语法树。

采用的图框架：DGL

运行所需环境：DGL+PyTorch+python3.7

下载项目后：运行family.py文件即可。

详细介绍

gnnmodel文件夹中不仅有GAT和TextCNN模型，还有诸如Transfomer, TextCNN, TextRCNN,GCN等模型。

gnnmodels文件夹中在gnnmodel文件夹中增加了sumModel.py文件，通过该文件来实现特征向量的有机融合（融合方式有拼接融合和逐点相加两种方式，经过实验发现拼接融合效果更好）。

图神经网络/testcfg文件夹存放的是原始数据集经过joern工具转换的属性控制流程图。

ExtendedRvNN文件夹存放的是原始数据集经过调用python中pycparser模块转换的抽象语法树(AST)。在之后的数据处理中，首先通过对AST的先序遍历获得所有节点符号作为训练语料库;然后使用word2vec进行词嵌入，学习每个节点符号的d维向量。

BatchProgramClassifier.py文件将ExtendedRvNN文件夹嵌入好的节点符号输入Extended RvNN模型中，得到对应的特征向量。

CFGDataset.py文件中使用DGL框架，将testinputs文件夹中的属性控制流程图经过数据处理，作为GCN模型的输入。

ParameterConfig.py文件中是参数的设置。

train_eval.py文件是对模型进行训练和测试，并通过性能指标来评估模型对代码漏洞检测的能力。

sample.txt文件中存放的是少部分的原始数据集，作为一个样本。

clean_gadget.py文件在原始数据集中删除注释，对函数进行标准化。

vectorize_gadget.py文件是将函数转化为token序列。

TextCp.py文件是将token序列进行数据处理，作为TextCNN模型的输入。

ins2vec.py采用的词嵌入方式是Word2Vec。(也试过FasText词嵌入方式，效果比Word2Vec差一点)。

writelabel26.txt是使用ANTLR工具从Software Assurance Reference Dataset (SARD)提取到的C函数级数据集，每个函数都有对应的漏洞，共包含26中漏洞类型，可以用于漏洞类型识别！

下载之后，如遇到问题，欢迎询问！ qq:1135685255
