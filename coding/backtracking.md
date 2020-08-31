回溯算法
-------------

回溯法采用**试错**的思想，它尝试分步的去解决一个问题。在分步解决问题的过程中，当它通过尝试发现现有的分步答案不能得到有效的正确的解答的时候，它将**取消上一步甚至是上几步**的计算，再通过其它的可能的分步解答再次尝试寻找问题的答案。回溯法通常用最简单的**递归**方法来实现，在反复重复上述的步骤后可能出现两种情况：

- 找到一个可能存在的正确的答案
- 在尝试了所有可能的分步方法后宣告该问题没有答案

在实现的过程中，**剪枝操作**是提高回溯效率的一种技巧。利用剪枝，我们并不需要穷举搜索所有的情况，从而提高搜索效率。

### ０-1 背包问题

```java
// 回溯算法实现
private int maxW = Integer.MIN_VALUE; // 结果放到maxW中
private int[] weight = {2，2，4，6，3};  // 物品重量
private int n = 5; // 物品个数
private int w = 9; // 背包承受的最大重量
// cw 当前背包总重量
public void f(int i, int cw) { // 调用f(0, 0)
  if (cw == w || i == n) { // cw==w表示装满了，i==n表示物品都考察完了
    if (cw > maxW) maxW = cw;
    return;
  }
  f(i+1, cw); // 选择不装第i个物品
  if (cw + weight[i] <= w) {
    f(i+1,cw + weight[i]); // 选择装第i个物品
  }
}
```

把物品依次排列，整个问题就分解为了 n 个阶段，每个阶段对应一个物品怎么选择。先对第一个物品进行处理，选择装进去或者不装进去，然后再递归地处理剩下的物品。

![img](images/42ca6cec4ad034fc3e5c0605fbacecea.jpg)

递归树中的每个节点表示一种状态，我们用（i, cw）来表示。

- i 表示将要决策第几个物品是否装入背包
- cw 表示当前背包中物品的总重量

添加备忘录后的代码

```java
private int maxW = Integer.MIN_VALUE;   // 结果放到maxW中
private int[] weight = {2，2，4，6，3};  // 物品重量
private int n = 5;                      // 物品个数
private int w = 9;                      // 背包承受的最大重量
private boolean[][] mem = new boolean[5][10]; // 备忘录，默认值false
public void f(int i, int cw) { // 调用f(0, 0)
  if (cw == w || i == n) {     // cw==w表示装满了，i==n表示物品都考察完了
    if (cw > maxW) maxW = cw;
    return;
  }
  if (mem[i][cw]) return;      // 重复状态
  mem[i][cw] = true;           // 记录(i, cw)这个状态
  f(i+1, cw);                  // 选择不装第i个物品
  if (cw + weight[i] <= w) {
    f(i+1,cw + weight[i]);     // 选择装第i个物品
  }
}
```

