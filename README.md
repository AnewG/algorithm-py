基础：
=================

```
why n(n-1)/2 = (n-1)+(n-2)+...+1 ~ N*N/2
http://www.maths.surrey.ac.uk/hosted-sites/R.Knott/runsums/triNbProof.html

图-广度优先运行时间为 边数+顶点数 能够有效找出边数最少的，但找出的不一定是总值最小的
Dijkstra's Algorithm 适用于 "非负权边的有向图"
有负权边的可以使用 贝尓曼-福德(bellman-ford) 算法

贪婪算法：每个局部最优解逐步完成到全局，就可以认为是全局最优解

数学上，给定集合 S，其幂集是以 S的全部子集为元素的集合
https://zh.wikipedia.org/wiki/%E5%86%AA%E9%9B%86  2的n次方

NP完全问题的简单定义是，以难解著称的问题，如旅行商问题和集合覆盖问题
怎样判断NP问题：
1.元素较少时算法的运行速度非常快，但随着元素数量的增加，速度会变得非常慢。
2.涉及 "所有组合" 的问题通常是NP完全问题。
不能分解为小问题的，必须考虑各种情况的。可分的用动态规划
3.如果问题涉及序列（如旅行商问题中的城市序列）且难以解决，它 "可能" 就是NP完全问题。
4.如果问题涉及集合（如广播台集合）且难以解决，它 "可能" 就是NP完全问题。
5.如果问题可转换为集合覆盖问题或旅行商问题，那它 "肯定" 是NP完全问题。
旅行商问题与最短路径问题的区别：https://www.zhihu.com/question/270419595

动态规划：
1.从网格开始（行为各商品[每一行表示之前所有商品]，列为从小到大各种容量的背包，值为价值）
2.cell[i][j] = max(cell[i-1][j], 当前商品价值+cell[i-1][j-当前商品重量])
贪婪算法不同：贪婪是每次都拿最高的，直到不能拿
动态规划用于解决离散的，不会互相依赖的子问题

推荐系统：k最近邻算法
更好的方案为 余弦相似度 KNN
```

排序方法：
=================

```

 * selection sort 选择排序 N*N/2 次比较，N次交换
 
   开始时整个数列为扫描列表。
   每次扫描找出当前扫描列表中最小的那个，交换移到扫描列表最前，然后排除出扫描列表
   循环至扫描列表长度为1结束
   
   特点1：运行时间与输入无关，无论何种输入排序时间相差无几。上一次扫描无法为下一次扫描提供可用的信息
   特点2：数据移动少，交换次数与数组大小是线性关系
   
 * insert sort 插入排序平均 N*N/4 次比较与交换，最差情况为 N*N/2 次比较与交换，最好为 N-1 比较 0 交换
 
   对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。未排序数组逐渐减少。
   因而在从后向前扫描过程中，需要反复把已排序元素逐步向后挪位，为最新元素提供插入空间。
 
   特点1：与输入有关，越接机排序顺序的输入序列所用时间越短
   特点2：如果要优化着重于减少移动的操作次数（比如用向右平移代替元素两两交换）
 
 * shell sort 希尔排序 https://zh.wikipedia.org/wiki/%E5%B8%8C%E5%B0%94%E6%8E%92%E5%BA%8F 使用列的模式更容易理解
 
   希尔排序是基于插入排序的以下两点性质而提出改进方法的：
     1.插入排序在对几乎已经排好序的数据操作时，效率高，即可以达到线性排序的效率（越后期越有序）
     2.但插入排序一般来说是低效的，因为插入排序每次只能将数据移动一位（按间隔组成子数组进行插入排序，移动位置较远，能更快达到最终位置）
 
   中心思想是使数组中间隔为 n(n逐步减少) 的元素组成的子数组都是有序的
   
   最好步长算式： 
   9*(Math.pow(4,i)) - 9*(Math.pow(2,i)) + 1 => 1，19，109，505...
   Math.pow(2,i+2) * (Math.pow(2,i+2)-3) + 1 => 5，41，209，929...
   result => 1, 5, 19, 41, 109...
 
   特点1：比较在希尔排序中是最主要的操作，而不是交换。
   特点2：用这样步长序列的希尔排序比插入排序要快，甚至在小数组中比快速排序和堆排序还快，但是在涉及大量数据时希尔排序还是比快速排序慢。
 
 * merge sort (sub) 归并排序
 
   merge 函数：
     接收数组A，全拷贝至另一数组B，B数组分2段[逻辑上,各自要有序]，分别给予变量 i,j 进行索引。
     比较 B[i],B[j], 较小的值写回 A，较小的索引变量自增。
     某一索引先到其逻辑上分段尾部时，另一段剩余的直接全部写回 A
 
   主要缺点是需要额外空间来归并跟 N 成正比 (归并2个已排序数组的操作其实可以使用各种方式)
   由于排序路径跟二叉树相似，所以时间跟 NlgN 成正比
   
   自顶向下的排序算法就是把数组元素不断的二分，直到子数组的元素个数为一个，因为这个时候子数组必定是已有序的。
   然后将两个有序的序列合并成一个新的有序的序列，两个新的有序序列又可以合并成另一个新的有序序列，以此类推，直到合并成一个有序的数组
   最坏 NlgN，最好 1/2N*lgN。一共在logN个维度上进行比较，每个维度最多需要比较N次，最少比较N/2次(一方都比另一方小)
   最多访问 6NlgN 次数组，拷贝数组2N[原数组与新数组], 赋值2N[两数组之间], 比较2N[应该只有1N?]
   
   自底向上的归并排序算法的思想就是数组中先一个一个归并成两两有序的序列（不需要递归分割，直接扫描两两处理）。
   两两有序的序列归并成四个四个有序的序列，然后四个四个有序的序列归并八个八个有序的序列，以此类推直到归并的长度大于整个数组的长度，此时整个数组有序。
 
 * quick sort
 
   结合了额外空间小（可以原地）和 NlgN 复杂度（快速）的特点
   该算法还可以进一步改进
   
   当数组长度小于M的时候（high-low <= M），终止递归不进行快排，而进行插排
   转换参数M的最佳值和系统是相关的，一般来说，5到15间的任意值在多数情况下都能令人满意
   
   三数取中法(取比较基准)：分别取出数组的最左端元素，最右端元素和中间元素，在这三个数中取出中位数，作为基准元素。
   
   对于大量重复元素的数组，不同于普通的两切分，甚至可以考虑三切分 [ 小于x1 (x1) 大于x1小于x2 (x2) 大于x2 ]
 
 * heap sort
 
   构造堆: 如果是二叉堆，堆（二叉堆）可以视为一棵完全的二叉树。可用数组索引表示堆情况，构造完后此时最大元素在顶部 
   如果数组索引0放第一个元素，则
     父节点i的左子节点在位置 2i+1;
     父节点i的右子节点在位置 2i+2;
     子节点i的父节点在位置 floor((i-1)/2);
   如果索引0放空，则 2i，2i+1，floor(i/2)

   最大堆调整（Max-Heapify）：将堆的末端子节点作调整，使得子节点永远小于父节点
   创建最大堆（Build-Max-Heap）：将堆所有数据重新排序，使其成为最大堆
   堆排序（Heap-Sort）：移除位在第一个数据的根节点，最后的节点移到空缺处，并做最大堆调整的递归运算
   
   能平衡有效的利用空间与时间，但由于无法缓存，所以使用较少
 
稳定排序：插入排序，归并排序
原地排序：除了归并排序都可以

算法：        时间复杂度      空间复杂度
选择排序         N平方          1
插入排序        N~N平方         1       依赖输入数据
希尔排序
快速排序         NlogN         lgN
并归排序         NlogN          N
堆排序           NlogN          1(因为用本身的数组来表示堆)

```

查找方法：
=================

```

 * 二叉查找树 BST：

   也称二叉搜索树，有序二叉树，排序二叉树
   左子树上所有节点的值均小于它的根节点的值，右子树上所有节点的值均大于它的根节点的值，没有键值相等的节点
   在进行插入操作时，不必移动其它结点，只需改动某个结点的指针（不需要旋转调整）
   树层级较高时，查找效率较低
   范围查询使用中序遍历
   删除节点：由于二叉查找树的性质，如果将当前节点替换为左子树中最大的或者右子树中最小的一定不会破坏二叉查找树的结构

 * 平衡查找树 [能够保证在最差的情况下也能达到lgN的效率]：

   其中有以下实现：
     2-3搜索树，2-3-4树，红黑树，AVL树等
   
 * 2-3 搜索树
 
   任一节点只能是 2-结点 或 3-结点 [2-结点：一个键(值)二条链接，3-结点：两个键三条链接]
   各叶子节点到根节点距离相同
   插入时：
    2-结点 之间插入变为 3-结点
    3-结点 临时创建为 4-结点 后中心元素上提，并不断递归向上提(消除4-结点)
    由于这种性质，树是由下向上生长。
    
 * 红黑树：

   由二叉查找树(全 2-结点)+额外信息(红黑数据)来表示
   节点是红色或黑色。根和叶子是黑色。每个红色节点必须有两个黑色的子节点。
   从任一节点到其每个叶子的所有简单路径都包含相同数目的黑色节点
   好处是可以用二叉树的方法查找(简洁高效)，并且表示出 2-3树(平衡查找树) 因为是黑色平衡的(每条路径黑链数相同)
   进行添加或删除后，通过左旋或右旋保证上述红黑树的性质

                                  z
      x                          /                  
     / \      --(左旋)-->        x   旋转后仍然保持原二叉树的性质
    y   z                      /
                              y
   
   
                               y
      x                         \                 
     / \      --(右旋)-->         x
    y   z                         \
                                   z

   红黑树结点的插入：
   	1.作为普通二叉树插入结点
   	2.着色为红色，然后调整树满足上述关系（分不同几种情况递归进行旋转与重新着色处理）
    [如果设为黑，根到叶子路径上多个额外黑节点，很难调整。设为红，可能会出现连续红色的冲突，可以通过颜色调换和树旋转来调整]
   红黑树结点的删除：
   	1.作为普通二叉树删除结点
   	2.然后调整树满足上述关系（分不同几种情况递归进行旋转与重新着色处理）
   https://zh.wikipedia.org/wiki/%E7%BA%A2%E9%BB%91%E6%A0%91

```

图：
=================

![Alt text](img/graph.png?raw=true "Title")

```

 * 无向图
 
   连通分支（Connected Component）是指：在一个图中，某个子图的任意两点有边连接，并且该子图与去掉该子图剩下的任何点都没有边相连
   强连通分支是指子图点i可以连接到点j，同时点j也可以连接到点i。
   弱连通（只对有向图）分支是指子图点i可以连接到点j，但是点j可以连接不到点i。
   
   图的表示有以下3种
   
```

![Alt text](img/data_present.png?raw=true "Title")
![Alt text](img/data_present2.png?raw=true "Title")
![Alt text](img/data_present3.png?raw=true "Title")

```

 * 深度优先
   
 * 广度优先（原理用先进先出队列，结果路径可能较短）
   
```
