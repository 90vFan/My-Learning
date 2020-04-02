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
- 删除给定指针 O(1)，删除给定值 O(n), 首先查找  

链表本身没有大小的限制，天然地支持动态扩容

![img](images/4f63e92598ec2551069a0eef69db7168.jpg)