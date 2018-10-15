```
why n(n-1)/2 = (n-1)+(n-2)+...+1
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
3.如果问题涉及序列（如旅行商问题中的城市序列）且难以解决，它 "可能" 就是NP完全问题。
4.如果问题涉及集合（如广播台集合）且难以解决，它 "可能" 就是NP完全问题。
5.如果问题可转换为集合覆盖问题或旅行商问题，那它 "肯定" 是NP完全问题。
旅行商问题与最短路径问题的区别：https://www.zhihu.com/question/270419595
```

排序方法：
=================

```

 * selection sort 选择排序
   
   开始时整个数列为扫描列表。
   每次扫描找出当前扫描列表中最小的那个，交换移到扫描列表最前，然后排除出扫描列表
   循环至扫描列表长度为1结束
   
   特点1：运行时间与输入无关，无论何种输入排序时间相差无几。上一次扫描无法为下一次扫描提供可用的信息
   特点2：数据移动最少，交换次数与数组大小是线性关系
   
 * insert sort 插入排序
 
   对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。未排序数组逐渐减少。
   因而在从后向前扫描过程中，需要反复把已排序元素逐步向后挪位，为最新元素提供插入空间。
 
   特点1：与输入有关，越接机排序顺序的输入序列所用时间越短
   特点2：如果要优化着重于减少移动的操作次数（比如用向右平移代替元素两两交换）
 
 * shell sort 希尔排序
 
   中心思想是使数组中任意间隔为 [h数列中的元素] 都是有序的
   生成h数列(每个i生成2个,i>=0,值要小于排序数组长度) 9*4的i次方 - 9*2的i次方 + 1 < arr.length || 2的i+2次方 * (2的i+2次方-3) + 1
   过程使用插入排序(步长为h数列中的值循环递减至最小的元素1)
   最终再用插入排序将局部有序的数组排序
 
   特点1：插入排序对于大规模乱序数组较慢，元素需要从一端移动到另一端。希尔排序减少移动多做比较，改进此问题。
 
 * merge sort (sub) 归并排序
 
   主要缺点是需要额外空间来归并跟 N 成正比 (归并2个已排序数组的操作其实可以使用各种方式)
   由于排序路径跟二叉树相似，所以时间跟 NlgN 成正比
   
   自顶向下的排序算法就是把数组元素不断的二分，直到子数组的元素个数为一个，因为这个时候子数组必定是已有序的。
   然后将两个有序的序列合并成一个新的有序的序列，两个新的有序序列又可以合并成另一个新的有序序列，以此类推，直到合并成一个有序的数组
   
   自底向上的归并排序算法的思想就是数组中先一个一个归并成两两有序的序列（不需要递归分割，直接扫描两两处理）。
   两两有序的序列归并成四个四个有序的序列，然后四个四个有序的序列归并八个八个有序的序列，以此类推直到归并的长度大于整个数组的长度，此时整个数组有序。
 
 * quick sort
 
   结合了额外空间小和 NlgN 复杂度的特点
   该算法还可以进一步改进，不同于普通的两切分，甚至可以考虑三切分 [ 小于x1 (x1) 大于x1小于x2 (x2) 大于x2 ]
 
 * heap sort
 
   构造堆: 如果是二叉堆，可用数组索引表示堆情况，构造完后此时最大元素在顶部
   下沉排序: 将最大元素删除放到堆调整后的空出位置，反复
   能平衡有效的利用空间与时间，但由于无法缓存，所以使用较少
 
稳定排序：插入排序，归并排序

```

查找方法：
=================

```

 * 二叉查找树：

   也称二叉搜索树，有序二叉树，排序二叉树
   左子树上所有节点的值均小于它的根节点的值，右子树上所有节点的值均大于它的根节点的值，没有键值相等的节点
   在进行插入操作时，不必移动其它结点，只需改动某个结点的指针（不需要旋转调整）
   树层级较高时，查找效率较低
   范围查询使用中序遍历

 * 平衡查找树：

   2-结点：一个键(值)二条链接，3-结点：两个键三条链接
   各叶子节点到根节点距离相同
   插入时：
    2-结点 之间插入变为 3-结点
    3-结点 临时创建为 4-结点 后中心元素上提，变为 3个2结点 或 1个3结点和2个2结点(上提的元素为了保持树平衡与另一结点合并为3结点)
    可递归向上提(消除4-结点)
    由于这种性质，树是由下向上生长。更能保持平衡的特性
    
 * 红黑树：

   由二叉查找树(全 2-结点 )+额外信息(红黑数据)来表示
   红链接(均为左链接)连接的2个结点合起来表示一个 3-结点，且左结点包含前2个子结点，右结点包含剩余的1个子结点
   黑链接为普通链接
   好处是可以用二叉树的方法查找(简洁高效)，并且表示出 2-3树(平衡查找树) 因为是黑色平衡的(每条路径黑链数相同)
   一个结点的颜色指的是指向其链接的颜色
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
   红黑树结点的删除：
   	1.作为普通二叉树删除结点
   	2.然后调整树满足上述关系（分不同几种情况递归进行旋转与重新着色处理）


```

回收站：
=================

动态规划

解决方案有：

1.带结果缓存的自顶向下法（当需要一个子问题的解时，先检查是否保存过此解，可以存储各种所需信息）
2.自底向上法（要求划分好子问题的规模后，按小至大排序后依次求解。这样当求解某个子问题时，更小的子问题都已求解完毕）
