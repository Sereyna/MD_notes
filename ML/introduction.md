# 绪论
监督学习
1. 回归——预测的是连续值
2. 分类——预测的是离散值

## 1 基本术语
- 数据集
- 样本 / 示例 / 特征向量
- 特征 / 属性
- 特征值 / 属性值
- 特征空间 / 属性空间 / 样本空间 / 输入空间
- 假设：学得模型对应了关于数据的某种潜在的规律
- 标记空间 / 输出空间
- 假设空间
- 版本空间

## 2 评估方法
### 分离训练集与测试集
**_留出法_**
"留出法" (hold-out) 直接将数据集 D 划分为两个互斥的集合?其中一个集合作为训练集 5 ，另一个作为测试集 T， 即$D=S\cup T ， S\cap T= \phi$.在 S 上训练出模型后，用 T 来评估其测试误差，作为对泛化误差的估计.

训练/测试集的划分要尽可能保持数据分布的一致性，避免困数据划分过程引入额外的偏差而对最终结果产生影响，如果从来样 (sampling) 的角度来看待数据集的划分过程，则保留类别比例的采样方式通常称为"分层采样" (stratified sampling).

在使用留出法时，一般要采用若干次随机划分、重复进行实验评估后取平均值作为留出法的评估结果.例如进行 100 次随机划分，每次产生一个训练/测试集用于实验评估， 100 次后就得到 100 个结果?而留出法返回的则是这 100 个结果的平均.

常见做法是将大约$2/3\sim 4/5$的样本用于训练，剩余样本用 于测试.

**_交叉验证法_**
"交叉验证法"(cross validation) 先将数据集 D 划分为 k 个大小相似的互斥子集，每个子集 $D_i$ 都尽可能 保持数据分布的一致性，即从 D 中通过分层采样得到。然后，每次用k-1 个子集 的并集作 为训练集?余 F 的那个子集作为测试集;这样就可获得 k组训练/测试集，从而可进行 k 次训练和测试? 最终返回的是这 k 个测试结果的均值。显然，交叉验证法评估结果的稳定性和保真性在很大程度上取决于 k的取值 ，为强调这一点 ，通常把交叉验证法称为 " k 折交叉验证" (k-fold cross validation)

与留出法相似，将数据集 D 划分为 k 个子集同样存在多种 划 分方式.为减小因样本划分不同 而 引入的差别 ， k 折交叉验证通常 要随机使用不同的划分重复 p 次，最终的评估 结果是这 p 次 k 折交叉验证 结果 的均值

**_自助法_**
自助法在数据集较小、难以有效划分训练/测试集时很有用;此外，自助法
能从初始数据集中产生多个不同的训练集，这对集成学习等方法有很大的好处.然而，自助法产生的数据集改变了初始数据集的分布，这会引入估计偏差。因此，在初始数据量足够时，留出法和交叉验证法更常用一些.

## 3 性能度量
1. 错误率和精度
2. 查准率（precision）、查全率（recall）和F1
3. ROC与AUC
4. 代价敏感错误率与代价曲线

## 4 比较检验
1. 假设检验
2. 交叉验证t检验
3. McNemar 检验
4. Friedman 检验与 Nemenyi 后续检验

## 5 偏差与方差



矩阵，设 $$ n_{samples} \geq n_{features} $$ , 则该方法的复杂度为 $$ O(n_{samples} n_{fearures}^2) $$