> https://www.youtube.com/watch?v=tIeHLnjs5U8&list=PLZHQObOWTQDNU6R1_67000Dx_ZCJB-3pi&index=4

# Lesson 1 什么是神经网络

> 什么是神经网络(以手写数字识别为例)。

每张图片的大小为28×28，所以总计784个神经元，神经元中装着的数字代表对应像素的灰度值：0表示纯黑像素，1表示纯白像素。我们把神经元里装着的数字叫做“激活值”，激活值越大，那么这个神经元就越亮。

![image-20210515222215623](E:\md笔记\images\image-20210515222215623.png)



这些784个神经元组成了网络的第一层。最后一层的十个神经元分别代表0-9这10个数字，这些神经元对应的激活值也是在0-1之间，这些值表示系统认为输入的图像对应着哪个数字的可能性。网络中间还有几层隐含层，里面进行着处理识别数字的具体工作：

![image-20210515222903994](E:\md笔记\images\image-20210515222903994.png)



神经网络运作的时候，上一层的激活值将决定下一层的激活值。所以说，神经网络处理信息的核心机制正是：一层的激活值是通过怎样的运算，算出下一层的激活值的。这是对生物神经网络的一种模仿：某些神经元的激发会促使另一些神经元激发。



识别数字的神经网络：输入为一张图片——实质是在网络输入层的784个神经元处输入了784个代表输入图像各像素的灰度值，那么这层激活值的图案会让下一层的激活值产生某些特殊的图案，再让再下一层的产生特殊的图案，最终在输出层得到某种结果，而输出层最亮的那个神经元就表示神经网络的“选择。

![image-20210515230908007](E:\md笔记\images\image-20210515230908007.png)





![image-20210515231414393](E:\md笔记\images\image-20210515231414393.png)

如上图，第二层和第三层的各个神经元都对应组成原数字的更小的”部件“。这样，当图像输入进来的时候，它就能把所有关联短边的八到十个神经元都给点亮：

![image-20210515231707483](E:\md笔记\images\image-20210515231707483.png)



假如神经网络真能识别出给定图片的图案和边缘，就能很好的运用到图片识别任务上来，甚至不光是图像识别，世界上各种人工智能的任务，都可以转化为抽象元素，一层层的抽丝剥茧，如语音识别，就是要从原视频中识别出特殊的声音，组合成特定的音节，组合成单词，再组合成短语以及更加抽象的概念。



如何建立起前一层和下一层的神经元的连接呢？

拿第二层的一个神经元来说，我们需要给这个神经元与第一层的所有神经元都建立一条接线，且都赋上一个权重值；然后我们让第一层的所有激活值与其对应的权重值一起算出它们的加权和：

![image-20210515232925022](E:\md笔记\images\image-20210515232925022.png)



将这些权重值做成一个表格，把正的权重值标记成绿色，负的标记成红色，颜色越暗表示权重越接近于0。



计算出来的加权和可能是任意大小，但在这个网络中，我们需要激活值都处在0-1之间；于是，我们顺其自然地把这个加权和输进某个函数，使其输出都在0-1之间。其中一个叫 `sigmoid` 的函数非常常用，它又叫 logistic 曲线。简而言之，它能把非常大的负值变成接近0，非常大的正值变成接近1，而在取值0附近则是平稳增长的。

![image-20210516110405030](E:\md笔记\images\image-20210516110405030.png)



所以这个神经元中的激活值实际上就是一个加权和到底有多正的打分。但有时，即使加权和大于0时，你也不想把神经元点亮。可能只有当和大于例如10的时候才希望它被激发，此时只需要加上一个偏置值。

![image-20210516111001932](E:\md笔记\images\image-20210516111001932.png)



总而言之，**权重告诉你这个第二层的神经元关注什么样的像素图案，偏置则告诉你加权和得有多大才能让神经元的激发变得有意义**。

以上就解说完了其中一个神经元。但这一层的每一个神经元都会和第一层全部的784个神经元相连接。

![image-20210516111647924](E:\md笔记\images\image-20210516111647924.png)



这里假设的是第二层有16个神经元，那么第一层和第二层之间的连接就需要784×16个权重值、16个偏置值，别的层之间的连接也有它们分别自带的权重和偏置。该网络一共需要13000个权重、2个偏置，相当于这个网络上有13000多个旋钮开关邀你调整，从而带来不一样的结果。

![image-20210516132957508](E:\md笔记\images\image-20210516132957508.png)

所以，当我们讨论机器学习的时候，我们其实是在讲：电脑应该如何设置这一大坨的数字参数才能让它正确的解决问题。



我们把某一层中所有的激活值统一成一列向量，再把它和下一层间所有的权重放到一个矩阵中（矩阵第n行就是这一层的所有神经元和下一层第n个神经元间所有连线的权重），这样权重矩阵和向量乘积的第n项就是这一层所有的激活值和下一层第n个神经元间连线权重的加权和。

![image-20210516133748287](E:\md笔记\images\image-20210516133748287.png)



神经网络各层之间激活值的转化可以清晰的表达为：

![image-20210516133953539](E:\md笔记\images\image-20210516133953539.png)



前面，我们说的是“神经元里装着一个数字”，实际上，神经元中装着的值取决于你的输入图像。所以，我们把神经元看做一个函数才更加准确。函数的输入是上一层所有神经元的输出，输出是一个0-1之间的值。

其实整个神经网络就是一个函数，一个输入为784个像素值、输出为数字0-9中的一个。这个函数极其复杂，因为它用了13000个权重参数、2个偏置参数来识别图案，又要循环不停的用到矩阵乘法和sigmoid映射运算。

![image-20210516134727590](E:\md笔记\images\image-20210516134727590.png)



神经网络如何通过数据来获得合适的权重和偏置？

# Lesson 2 梯度下降

>  神经网络如何学习？梯度下降的概念。

定义一个成本/损失函数(cost function)，**损失函数**衡量的是获得的结果与目标值的不相似程度，这是我们在训练过程中**要最小化**的损失函数。

# Lesson 3 反向传播

> 反向传播（神经网络学习的核心算法）。

**反向传播算法**算的是单个训练样本想怎样修改权重与偏置，不仅是说每个参数应该变大还是变小，还包括了这些变化的比例是多大才能最快的降低代价。真正的梯度下降得对好几万个训练范例都这么操作，然后对这些变化值取平均，但算起来太慢了，所以我们通常先把所有的样本分到各个minibatch中去，计算一个minibatch来作为梯度下降的一步；计算每个minibatch的梯度，调整参数，不断循环；最终会收敛到代价函数的一个局部最小值上。

# Lesson 4 链式法则

> 怎样理解深度学习中的链式法则（微积分）。

![image-20210516154225307](E:\md笔记\images\image-20210516154225307.png)

