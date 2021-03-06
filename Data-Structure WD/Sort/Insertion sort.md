## 插入排序

### 算法思想

每次将一个待排序的记录按其关键字大小插入到前面已经排好序的子序列中, 直到全部记录插入完成

### 算法实现(不带哨兵)

```c
// 直接插入排序
void InsertSort(int A[], int n)
{
    int i, j, temp;
    for (i = 1; i < n; ++i) // 将各元素插入已经排好序的序列中
    {
        if (A[i] < A[i - 1]) // 如果A[i]的关键字小于前驱
        {
            temp = A[i];                                // 用temp暂存A[i]
            for (j = i - 1; j >= 0 && A[j] > temp; j--) // 检查所有前面已经排好序的元素
            {
                A[j + 1] = A[j]; // 所有大于temp的元素都向后挪位
            }
            A[j + 1] = temp; // 复制到插入位置
        }
    }
}
```

### 算法实现(带哨兵)

优点: 无需每轮都判断j>=0
```c
void InsertSort(int A[], int n)
{
    int i, j;
    for (i = 2; i <= n; i++) // 依次将A[2] - A[n]插入到前面已经排序的序列
    {
        if (A[i] < A[i - 1]) // 若A[i]关键码小于其前驱, 将A[i]插入有序表
        {
            A[0] = A[i]; // 复制为哨兵, A[0]不存放元素
            for (j = i - 1; A[0] < A[j]; --j) // 从后往前查找带插入的位置
            {
                A[j + 1] = A[j]; // 向后挪位
            }
            A[j + 1] = A[0]; // 复制到插入位置
        }
    }
}
```

### 算法效率分析

- 空间复杂度: O(1)
- 时间复杂度: 主要来自对比关键字、移动元素; 若有n个元素, 则需要n-1趟处理. 考虑最坏情况: 第1趟, 对比关键字2次, 移动元素3次; 第2趟, 对比关键字3次, 移动元素4次; 第i趟, 对比关键字i+1次, 移动元素i+2次...第n-1趟, 对比关键字n次, 移动元素n+1次. 所以最坏的时间复杂度: O(n^2); 最好的时间复杂度: O(n); 平均时间复杂度: O(n^2)
- 算法稳定性: 稳定

## 优化——折半插入排序

- 思路: 先用折半查找找到应该插入的位置, 再移动元素
- 当low>high时折半查找停止, 应将[low, i-1]内的元素全部右移, 并将A[0]复制到low所指的位置
- 当A[mid]==A[0]时, 为了保证算法的“稳定性”, 应继续在mid所指位置的右边寻找插入的位置

```c
// 折半插入排序
void InsertSort(int A[], int n)
{
    int i, j, lowm, high, mid;
    for (i = 2; i <= n; i++) // 依次将A[2]~A[n]插入前面的已排序序列
    {
        A[0] = A[i]; // 将A[i]暂存到A[0]
        low = 1;     // 设置折半查找的范围
        high = i - 1;
        while (low <= high) // 折半查找(默认递增有序)
        {
            mid = (low + high) / 2; // 取中间点
            if (A[mid] > A[0])
            {
                high = mid - 1; // 查找左半子表
            }
            else
            {
                low = mid + 1; // 查找右半子表
            }
            for (j = i - 1; j >= high + 1; --j)
            {
                A[j + 1] = A[j]; // 统一后移元素, 空出插入位置
            }
            A[high + 1] = A[0]; // 插入操作
        }
    }
}
```

## 希尔排序(Shell Sort)

比较好的情况: 基本有序
| 20  | 30  | 10  | 50  | 60  | 40  | 70  | 80  |
| --- | --- | --- | --- | --- | --- | --- | --- |

### 基本思想

- 先追求表中元素部分有序, 再逐渐逼近全局有序
- 先将待排序表分割成若干形如L[i, i+d, i+2d, ... , i+kd]的“特殊”子表, 对各个子表分别进行直接插入排序. 缩小增量d, 重复上述的过程, 直到d=1为止
- 希尔本人建议: 每次增量缩小一半

### 代码实现

```c
// 希尔排序
void ShellSort(int A[], int n)
{
    int d, i, j;
    // A[0]只是暂存单元, 不是哨兵, 当j<=0时, 插入位置已到
    for (d = n / 2; d >= 1; d = d / 2) // 步长变化
    {
        for (i = d + 1; i <= n; i++)
        {
            if (A[i] < A[i - d]) // 需将A[i]插入有序增量子表
            {
                A[0] = A[i]; // 暂存在A[0]
            }
            for (j = i - d; j > 0 && A[0] < A[j]; j -= d)
            {
                A[j + d] = A[j]; // 记录后移, 查找插入的位置
            }
            A[j + d] = A[0]; // 插入
        }
    }
}
```

### 时间复杂度

和增量序列d1, d2, d3...的选择有关系, 目前无法用数学手段证明确切的时间复杂度. 最坏的时间复杂度为O(n^2), 当n在某个范围时, 可达O(n^1.3)

### 算法性能分析

- 不稳定
- 仅仅适用于顺序表, 不适用于链表