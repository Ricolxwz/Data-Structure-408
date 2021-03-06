## B+树

### B+树的定义

一棵m阶的B+树需要满足下列条件:
- 每个分支结点最多有m棵子树(孩子结点)
- 非叶根结点至少有两棵子树, 其他每个分支结点至少有⌈m/2⌉棵子树(追求“绝对平衡”, 即所求子树的高度相同)
- 结点的子树个数和关键字的个数相等(这一点和B-树有很大的不同)
- 所有叶结点包含全部关键字及指向相应记录的指针, 叶结点中将关键字按大小顺序排列, 并且相邻叶结点按大小顺序互相链接起来(支持顺序查找)
- 所有分支结点中仅包含它的各个子结点中关键字的最大值以及指向其子结点的指针

### m阶的B+树和m阶的B树的主要差异

- 在B+树中, 有n个关键字的结点只含有n棵子树, 即每个关键字对应一棵子树; 而在B树中, 具有n个关键字的结点含有n+1棵子树
- 在B+树中, 结点的关键字数的范围是⌈m/2⌉≤n≤m(根结点: 2≤n≤m); 在B树中, 结点的关键字数的范围是⌈m/2⌉-1≤n≤m-1(根结点: 1≤n≤m-1)
- 在B+树中, 叶结点包含信息, 所有的非叶结点仅起到索引的作用, 非叶结点中的每个索引项只含有对应子树的最大关键字和指向该子树的指针, 不含有该关键字对应的记录的存储地址; 在B树中, 每个结点都是包含关键子对应的信息的
- 在B+树中, 叶结点包含了全部关键字, 即在非叶结点中出现的关键字也会出现在叶结点中; 而在B树中, 终端结点包含的关键和其他其他内部结点所包含的关键字是不重复的
- B+树在查找过程中, 非叶结点上的关键字值等于给定值时并不终止, 而是继续向下寻找, 直到找到叶结点上的该关键字为止

### B+树的两种查找运算

通常在B+树中有两个头指针, 一个指向根结点, 另一个指向关键字最小的叶结点
- 从最小关键字开始的顺序查找
- 从根结点开始的多路查找
