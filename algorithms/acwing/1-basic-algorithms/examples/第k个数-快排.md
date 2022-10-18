# 第k个数 - 快速选择

## 两种情况：
1. k在左半边：
    - 递归左半边Sl
2. k在右半边
    - 递归右半边Sr；k - Sl

## 时间复杂度：
- 递归第一层：n
- 递归第二层：n / 2
- 递归第三层：n / 4
- ...
```
所以时间复杂度
= n + n / 2 + n / 4 + n / 8 + ...
= n * (1 + 1/2 + 1/4 + 1/8 + ...)
<= 2n
= O(n)
```

## 重点/难点
- 左半边<=x，右半边>=x；左半边右边界是j，右半边左边界是i；左半边q[l] - q[j]，右半边q[j+1] - q[r]
- 求左半边数量sl = 左边右边界 - 左边左边界 + 1 = j - l + 1
- 快排一次递归后i与j的相对位置
    - i = j
    - i = j + 1
- 为什么分界点会把区间分为n/2？
    - 选分界点一共有n种选择，左半边长度分别为1,2,3...,n
    - 所以左半边平均长度 = (1 + 2 + 3 + ... + n) / n = (n+1)*n/(2n) = (n+1)/2，约等于n/2
- 跳出递归的条件为什么也可以写成l==r？
    - 因为这里时刻保证第k个数是在区间里的，可以l==r
    - 快排里则不能l==r，必须l>=r，因为快排的区间里可以为空

## 题解：
```
#include <iostream>
using namespace std;

const int N = 100010;

int quick_sort(int q[], int l, int r, int k)
{
    if (l >= r)
    {
        return q[l];
    }
    
    int x = q[(l + r) / 2], i = l - 1, j = r + 1;
    
    while (i < j)
    {
        while (q[++i] < x);
        while (q[--j] > x);
        
        if (i < j)
        {
            swap(q[i], q[j]);
        }
    }
    
    int sl = j - l + 1; 
    if (k <= sl)
    {
        return quick_sort(q, l, j, k);
    }
    return quick_sort(q, j + 1, r, k - sl);
}

int main()
{
    int n, k, q[N];
    cin >> n >> k;
    for (int i = 0; i < n; i++)
    {
        cin >> q[i];
    }
    
    cout << quick_sort(q, 0, n - 1, k) << endl;
    return 0;
}
```