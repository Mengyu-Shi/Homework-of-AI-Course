# 人工智能概论Pacman作业

## 实验0：Python学习

### 0.1  实验目的

学习Python语言，加深对于Python环境编程的应用的理解……

### 0.2  实验设计

（1）简短的Unix/Linux基础学习；
（2）Python环境安装；
（3）简短的Python基础学习。

## 实验1：Search

[实验1参考资料1](https://blog.csdn.net/weixin_42062229/article/details/94637972)
实验1的总体介绍

### 1.1  实验目的

本实验旨在通过编写吃豆人程序，实现宽度优先、深度优先、一致代价搜索、A*搜索算法，并将这些算法用于求解Pacman世界中的路径规划和吃豆子问题掌握其基本原理和实现方式。同时，通过分析吃豆人程序的实现过程，提升对人工智能的理论和实践水平。
通过阅读并修改实现所有搜索算法的代码文件，阅读运行游戏的主文件、底层逻辑文件及算法执行所需的数据结构定义文件等资料，了解Pacman游戏的程序框架与运行机制。
通过构建上述通用的搜索算法以实现问题求解，对比不同搜索算法下的求解结果，从而加深对搜索算法及其对应数据结构（先进先出队列、后进先出队列及优先级队列）的理解。

### 1.2  实验平台

Pycharm/VSCode，Anaconda
主要是search\search.py

### 1.3  实验设计

在这个项目中，设计的吃豆人Agent将在迷宫世界寻找路径，到达一个特定的地点并有效地收集食物，学生将构建通用的搜索算法并将其应用于Pacman场景。
首先，创建集合Visited_States以储存已访问节点，并生成堆栈（或队列）states用于搜索。读取Pacman的初始状态后将其放入states中。之后，进入循环直到当达到目标节点或states为空（表示没有未访问节点）。最后，输出每一步的动作，形成完整路线图。
不同的搜索策略使用不同的数据结构。深度优先搜索使用后进先出（LIFO）的堆栈，新生节点被添加到队列前部，确保最先扩展最近产生的节点。而宽度优先搜索则使用先进先出（FIFO）的队列，新生节点被添加到队列尾部。对于一致代价搜索和A*搜索，使用优先级队列，其中新生节点根据评估函数进行排序，并按照优先级从高到低加入队列。在搜索过程中，始终从队列末尾取出节点，即选择优先级最高的节点进行扩展。

### 1.4  实验内容

#### 1.4.1  深度优先搜索（DFS）策略

深度优先搜索策略采取优先扩展最新产生的节点，即先扩展最深的节点，纵向全部扩展后才考虑其他，而横向同等深度的节点则可以任意排列，frontier是LIFO队列(栈)，新生节点放置在前部，即后进先出队列，对应的结构体为util.py中的Stack。
设计思路：定义一个空数组closed，用以存放已经走过的节点，定义堆栈fringe存放未探索节点。首先将起始状态节点压入堆栈中，开始进行第一个循环。每一次循环都将堆栈中的第一个元素弹出。如果此点是目标节点，则结束搜索；如果这个点不是目标节点且没有访问过，则将其标记为已访问并存入closed中，同时获取其后继子节点并压入栈中；如果该点已访问过则弹出下一个元素继续进行循环。直到堆栈为空或者弹出的点是目标点时搜索结束。

#### 1.4.2  宽度优先搜索（BFS）策略

宽度优先搜索在搜索过程中按照接近起始节点的程度依次扩展节点，frontier是FIFO队列，即先进先出队列，优先扩展最浅的未扩展节点，对应的结构体是util.py中的Queue。
设计思路：基本流程同深度优先算法一致，区别在于此时用Quene存放未探索节点，每次弹出的元素都是最先存入的元素，也就是先进先出。

#### 1.4.3  一致代价搜索策略

一致代价搜索沿着代价等高线进行探索。对应的数据结构是优先级队列，对应的结构体是util.py中的PriorityQueue。
设计思路：定义一个变量cost用于记录当前代价，其初始值为0。同时，需要定义一个用于记录路径的空间以及一个用于记录父节点的空间。接下来，计算从起始点到其余所有点的最小代价路径，当找到根节点和目标节点之间的最短路径后，停止搜索。

#### 1.4.4  A*搜索策略

A*搜索算法的评价函数是从初始节点经n到目标节点的实际代价和估计代价之和来确定下一个要扩展的节点。对应的数据结构是优先级队列，对应的结构体是util.py中的PriorityQueue。
设计思路：与一致性代价搜索策略基本相同，已知起点和终点的位置来寻找最佳路径的启发式搜索算法。

一个状态的值定义为一个agent从该状态出发能得到的最佳输出（效益）。
$$ any \quad non-terminal \quad states, \quad v(s)=max(v(s')), s' \in successors(s) \\
any \quad terminal \quad states, \quad v(s)=known $$

## 实验2：Multi-Agent

### 2.1  实验目的

深入理解博弈搜索，掌握Minimax算法和α-β剪枝算法，在多Agent博弈搜索实验中，为经典版本的Pacman游戏设计多个Agents，也包括ghosts。同时，在此过程中实现Minimax算法的博弈以及基于α-β剪枝算法的博弈，并尝试基于实验框架进行评价函数设计，评价当前局面，支持博弈过程实施并分析结果。
博弈搜索是多智能体参与的一种搜索方法。首先需定义搜索的状态空间图，即构建博弈树。搜索的规则是双方交替进行。为评价每次搜索的效果，引入一个评价函数。一个智能体的搜索目标是找到一个节点，使得评价函数值极大化；而另一个智能体的搜索目标则是使评价函数值极小化。

### 2.2  实验平台

Pycharm/VSCode，Anaconda
主要是multiAgent\multiAgents.py

### 2.3  实验设计

在Pacman游戏中，多Agent博弈问题的特点是：有确定的参与方（Pacman和ghosts），按照一定的规则从各自允许的行为和策略中选择并加以实施，并且最后能分出胜负。博弈双方的对立性使得不再通过最小代价搜索，而且双方采用的搜索策略不同，而一方的行动都对游戏状态和另一方的决策起到重要影响。传统的搜索方法需要将节点一直拓展到分出胜负为止，搜索树的规模特别庞大，不适用于博弈问题求解。
在该实验中，要为含有ghosts环境下的Pacman设计程序，首先尝试重新设计evaluationFunction（评估函数）来估计每一步的评价，以此完善简单反射agent搜索，并完成minimax、α-β剪枝搜索算法，之后建立效果更好的评估函数。

[实验2参考资料1](https://blog.csdn.net/weixin_45942927/article/details/120315999)
一个代码组。

### 2.4  实验内容

#### 2.4.1  RefleXAgent

重新设计evaluationFunction（评估函数）。该评估函数需要接收Pacman位置、食物位置、ghost状态以及ghost受惊吓的时间等参数，并从游戏状态中获取其他参数。我们需要基于这些信息自行设计新的评估函数，以在测试布局下能够快速完美清除所有豆子。

#### 2.4.2 MiniMax

$$ any \quad pacman-controlled \quad states, \quad v(s)=max(v(s')), s' \in successors(s) \\
any \quad ghost-controlled \quad states, \quad v(s)=min(v(s')), s' \in successors(s) \\
any \quad terminal \quad states, \quad v(s)=known $$
也就是说，mini指的是对ghost控制的节点，ghost想要让这个节点取得最小值以最小化pacman的收益，max指的是对pacman控制的节点，agent想要让这个结点取得最大值以最大化pacman的收益。

[实验2参考资料2](https://blog.csdn.net/weixin_42165981/article/details/103263211)
一个minmax算法的图解，非常简单易懂。
为何pacman会选择在原地不动？因为原地不动value更高。为何原地不动value更高？因为pacman在原地不动时，ghost必须移动，ghost的value会变小，从而使得pacman的value变大。

#### 2.4.3  AlphaBetaAgent

Minimax算法的递归调用生成了给定深度内的所有节点才进行评价值的比较，往往会产生巨大的分支，尤其是遇到复杂问题的时候，节点数随搜索深度的增加呈指数级增长。这时候可以用α-β剪枝算法进行优化：只考虑必要的分支，减少节点数，但不会对结果产生任何影响。
判定哪些分支是必要的，哪些分支是不必要的，需要定义MAX层节点的下界为α，MIN层节点的上界为β，若**任意MAX层的下界大于等于后继MIN层某一个节点的上界，即α(父)≥β(子)，则该MIN节点肯定不会被MAX层选择**，即使该MIN节点包含其他较大值，也不会传递至上层，因此可以剪除该分支，称为α剪枝。同理，若**任意MIN层的上界小于等于后继MAX层某一个节点的下界，即α(子)≥β(父)，则该MAX节点肯定不会被MIN层选择**，称为β剪枝。

#### 2.4.4 ExpectimaxAgent

Minimax和alpha-beta很好，但是都是假设对手也是做出最优选择，在这种假设下，Pacman在一些情况下会认为自己是必死的，从而导致其策略选择出现问题。然而通常ghost并非一直做出最优选择，在该问题下就需要实现ExpectimaxAgent，用于对做出次优选择的Agent进行行为建模。
Expectimax算法是Minimax算法的改进，它假设对手不是每次都做出最优选择，而是有概率做出次优/一般选择。因此，Expectimax算法在计算每个节点的值时，需要考虑所有可能的子节点的值，并根据子节点的概率进行加权平均。具体来说，对于每个MIN节点（ghost控制的节点），Expectimax算法计算所有子节点的最小值，并取**平均值**作为该节点的值；对于每个MAX节点（Pacman控制的节点），Expectimax算法计算所有子节点的最大值，并取**最大值**作为该节点的值。这种办法意味着，ghost在做出选择时，会考虑所有可能的子节点的值，并根据子节点的概率进行加权平均，从而使得Pacman在做出选择时，能够更好地应对ghost的次优/一般选择。

#### 2.4.5 betterEvaluationFunction

[cnblog](https://www.cnblogs.com/lqblala/p/15302959.html)
一个代码组。
[cs188 project2](https://www.misaka-9982.com/2022/12/16/CS188-Proj-2/)
一个效果非常好的评估函数。一个有用的启发是，**在启发式函数中，应将最近的豆作为启发参数，而不是最远的豆，即优先吃掉最近的豆**。
**为了避免pacman在距离ghost很远时仍然倾向于远离ghost，可以仅在距离ghost很近时才考虑ghost（曼哈顿距离五六格之内）**。
[zhihu](https://zhuanlan.zhihu.com/p/106197121)
吃掉这个大圆点的话，吃豆人就会变成另外一种状态，地图上的鬼会变成白色的，然后可以被吃豆人吃掉。

在提供的函数 betterEvaluationFunction 中为 pacman 编写更好的评估函数。评估函数应该评估状态，而不是像 Reflex 代理评估函数那样执行操作。您可以使用任何可用的工具进行评估，包括上一个项目中的搜索代码。使用深度 2 搜索时，您的评估函数应该在一半以上的时间内使用一个随机幽灵清除 smallClassic 布局，并且仍然以合理的速率运行（要获得全额积分，Pacman 获胜时的平均分应约为 1000 分）。