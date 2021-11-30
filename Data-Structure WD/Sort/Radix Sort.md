## 基数排序

### 基本思想

假设长度为n的线性表中每个结点aj的关键字由d元组($k_j^{d-1}$, $k_j^{d-2}$, $k_j^{d-3}$, $k_j^{d-4}$,...,$k_j^{1}$, $k_j^{0}$)组成. 其中, 0≤$k_j^{i}$≤r-1(0≤j<n, 0≤i≤d-1), r称为“基数”(j表示的是结点的序号, d-i表示的是个位、十位...), $k_j^{d-1}$被称为最高位关键字(最主位关键字), $k_j^{0}$被称为最低位关键字(最次位关键字)

### 算法过程

基数排序得到递减序列的过程如下:
1. 初始化: 设置r个空队列, $Q_{r-1}, Q_{r-2}...Q_{0}$
2. 按照各个关键字位权重递增的次序(个、十、百), 对d个关键字位分别做“分配”和“收集”
3. 分配: 顺序扫描各个元素, 若当前处理的关键字位=x, 则将元素插入$Q_x$队尾
4. 收集: 把$Q_{r-1}, Q_{r-2}...Q_{0}$各个队列中的结点依次出队并链接

### 算法效率分析

- 基数排序通常基于链式存储实现

#### 空间复杂度

需要r个辅助队列, 空间复杂度=O(r)

#### 时间复杂度

一趟分配O(n), 一趟收集O(r), 总共d趟分配、收集, 总的时间复杂度=O(d(n+r))

#### 稳定性

稳定, 基你太稳

### 基数排序擅长解决的问题

- 数据元素的关键字可以方便地拆分为d组, 且d较小
- 每组关键字的取值范围不大, 即r较小
- 数据元素个数n较大