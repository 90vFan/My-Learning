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