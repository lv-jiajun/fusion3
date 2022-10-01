在基于深度学习的代码漏洞检测领域，提出一种基于多表征融合的代码漏洞检测方法S2FVD。 为保证漏洞检测的粒度，选取函数而非整个程序作为基本分析单元。 首先，通过对函数的词法和语法解析，从中构建token序列和属性控制流图（ACFG），作为函数的两种不同的原始表征结构； 然后，分别利用针对序列的神经网络TextCNN和图卷积神经网络，从中抽取深层次的代码语义特征； 最后，考虑到token序列和ACFG是从不同角度对同一函数的语义进行的互补性描述，因此通过直观的向量拼接操作，将从二者中抽取的特征向量进行有机融合，并送入分类层，实现函数级漏洞的检测。

词嵌入方式：Word2Vec

训练模型：TextCNN,GCN

开源的源码分析工具Joern（https://joern.readthedocs.io/en/latest/） 对函数级的C/C++源代码进行分析，来提取属性控制流图。

采用的图框架：DGL

运行所需环境：DGL+PyTorch+python3.7

下载项目后：运行family.py文件即可。

详细介绍
gnnmodel文件夹中不仅有GCN和TextCNN模型，还有诸如Transfomer, TextCNN, TextRCNN等模型。

gnnmodels文件夹中在gnnmodel文件夹中增加了sumModel.py文件，通过该文件来实现特征向量的有机融合。

testinputs文件夹存放的是原始数据集经过joern工具转换的属性控制流程图。

CFGDataset.py文件中使用DGL框架，将testinputs文件夹中的属性控制流程图经过数据处理，作为GCN模型的输入。

ParameterConfig.py文件中是参数的设置。

train_eval.py文件是对模型进行训练和测试，并通过性能指标来评估模型对代码漏洞检测的能力。

sample.txt文件中存放的是少部分的原始数据集，作为一个样本。

clean_gadget.py文件在原始数据集中删除注释，对函数进行标准化。

vectorize_gadget.py文件是将函数转化为token序列。

TextCp.py文件是将token序列进行数据处理，作为TextCNN模型的输入。

ins2vec.py采用的词嵌入方式是Word2Vec。(也试过FasText词嵌入方式，效果比Word2Vec差一点)。

下载之后，如遇到问题，欢迎询问！ qq:1135685255
