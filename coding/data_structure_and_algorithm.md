# 数据结构与算法之美 

## 数组

![image-20200331165016453](images/image-20200331165016453.png)

```py
a[i]_address = base_address + i * data_type_size

a[i][j]_address = base_address + ( i * n + j) * type_size
```



- 下标查找 O(1), 值查找 O(n)
- 添加 O(n)   # 移位，
- 删除 O(n)

优点：

使用连续空间，可以借助 CPU 缓存禁止，预读数据

缺点：

大小固定，一经声明就要占用整块连续内存空间

如果不够用，只能申请一个更大的内存空间

## 链表

![3.21.实现无序列表：链表.figure6](images/3.21.实现无序列表：链表.figure6-1591345877907.png)

![img](images/b93e7ade9bb927baad1348d9a806ddeb.jpg)![img](images/452e943788bdeea462d364389bd08a17.jpg)

- 查找 O(n)   # 遍历
- 插入 O(1)
- 删除给定指针 O(1)，删除给定值 O(n)， 首先查找  

链表本身没有大小的限制，天然地支持动态扩容

![img](images/4f63e92598ec2551069a0eef69db7168.jpg)

### 插入节点

**指针**： 将某个变量赋值给指针，实际上就是将这个变量的地址赋值给指针，或者反过来说，指针中存储了这个变量的内存地址，指向了这个变量，通过指针就能找到这个变量。

![img](images/05a4a3b57502968930d517c934347c6e.jpg)

```sh
p->next = x;       // 将p的next指针指向x结点 (a->next = x)
x->next = p->next; // 将x的结点的next指针指向b结点 (x->next = b)
```

![img](images/4a701dd79b59427be654261805b349f8.jpg)

## 栈

### 先进后出

栈只支持两个基本操作：**入栈 push()和出栈 pop()**

### 动态扩容

当栈满了之后，我们就申请一个更大的数组，将原来的数据搬移到新数组中

![img](images/b193adf5db4356d8ab35a1d32142b3da.jpg)

### 应用

#### 函数调用栈

```python

int main() {
   int a = 1; 
   int ret = 0;
   int res = 0;
   ret = add(3, 5);
   res = a + ret;
   printf("%d", res);
   reuturn 0;
}

int add(int x, int y) {
   int sum = 0;
   sum = x + y;
   return sum;
}
```

![img](images/17b6c6711e8d60b61d65fb0df5559a1c.jpg)

#### 表达式求值

![img](images/bc77c8d33375750f1700eb7778551600.jpg)

#### 十进制转化二进制

![3.8.十进制转换成二进制.figure5](images/3.8.十进制转换成二进制.figure5.png)

## 队列

### 先进先出FIFO

**入队 enqueue()** ，放一个数据到队列尾部

**出队 dequeue()**，从队列头部取一个元素

![img](images/9eca53f9b557b1213c5d94b94e9dce3e.jpg)

![c916fe2212f8f543ddf539296444d393](images/c916fe2212f8f543ddf539296444d393.jpg)

```sh
// 入队
tail->next = new_node;
tail = tail->next;
// 出队
head = head->next;
```

![3d81a44f8c42b3ceee55605f9aeedcec](images/3d81a44f8c42b3ceee55605f9aeedcec.jpg)

```sh
tail = (tail + 1) % n;
head = (head + 1) % n;
```



## 递归

写递归代码的关键就是找到如何将大问题分解为小问题的规律，并且基于此写出递推公式，然后再推敲终止条件，最后将递推公式和终止条件翻译成代码。

### 三定律

1. 递归算法必须具有基本情况
2. 递归算法必须改变其状态并向基本情况靠近
3. 递归算法必须以递归方式调用自身



1. 一个问题的解可以分解为几个**子问题**的解
2. 这个问题与分解之后的子问题，除了数据规模不同，求解**思路完全一样**
3. 存在递归**终止条件**

### 弊端

- 堆栈溢出
- 重复计算
- 调用耗时多
- 空间复杂度高（O(n)）

### 应用

#### 分形(可视化递归)

![4.7.介绍：可视化递归.ac1](images/4.7.介绍：可视化递归.ac1.png)

### 汉诺塔游戏

![4.10.汉诺塔游戏.figure1](images/4.10.汉诺塔游戏.figure1.png)