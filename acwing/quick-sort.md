# 快速排序
## 中心思想
- 分治
<br></br>

## 条件
- 已知有一个长度为n的数组q，一个整数l代表q的最左端位置q[0]， 另一个整数r代表q的最右端位置q[n-1]
<br></br>

## 步骤

1. 确定分界点x (四选一)
    - 左侧：q[l]
    - 中间：q[(l+r)/2]
    - 右侧：q[r]
    - 随机位置
2. 调整区间
    - 把数组q里<=x的数字放在x左侧
    - 把数组q里>=x的数字放在x右侧
3. 递归处理左右两段
<br></br>

## 解决方案
### - 暴力解题法:
1. 建空数组a和b
2. 遍历数组q
    - 如果元素<=x，将它插入到a数组里
    - 如果元素>x，将它插入到b数组里
3. 将a中的元素，x，和b中的元素分别放回到q里
4. 递归处理x左右两段
<br></br>

### - 双指针法
1. 设一个指针i指向q[0]，j指向q[n-1]
2. 从左侧遍历元素，只要i指向的元素<x，i就右移，直到遇到一个>=x的元素为止
3. 从右侧遍历元素，只要j指向的元素>x，j就左移，直到遇到一个<=x的元素为止
4. 如果i在的位置<j在的位置，交换i和j指向的元素
5. 如果i<j，循环2-4
5. 递归处理j及j左侧，和j右侧





<br></br><br></br>

# Quick Sort

## Key Idea
- Divide and Conquer


## Condition

Now we have:
- an array q with size n

- an integer l representing the leftmost position of q, which is q[0]

- another integer r representing the rightmost position of q, which is q[n-1]


## Steps

1. Determine a split point x from one of the following ways
    - leftmost: q[l]
    - midpoint: q[(l+r)/2]
    - rightmost: q[r]
    - a random position on q
<br></br>
2. Adjust the left and right sides
    - put elements that <= x to the left side of x
    - put elements that >= x to the right side of x
<br></br>
3. Recursively repeat the above steps for the left and right sides
<br></br>

## Solutions
### - Brute force solution
1. Initialize new arrays a and b
2. Iterate elements in q:
    - if element <= x, put it in a
    - if element > x, put it in b
3. Put elements in a, x, and elements in b back to q respectively
4. Recursively repeat the above steps for the left and right sides
<br></br>

### - Two pointers, in-place solution
1. Set a pointer i pointing at q[0], a pointer j pointing at q[n-1]
2. Iterate from the left, as long as the element i points at < x, move i one step to the right
3. Iterate from the left, as long as the element j points at > x, move j
one step to the left
4. If index of i < index of j, swap q[i] with q[j]
5. Iterate step 2-4 as long as index of i < index of j
6. Recursively repeat the steps for indices <= j and indices > j
<br></br>


# Code for two pointers, in-place solution 双指针解法代码
    # C++
    # include <iostream>

    const int N = 1e6 + 10;

    int n;
    int q[N];

    void quick_sort(int q[], int l, int r)
    {
        if (l >= r) return;
        
        int i = l - 1, j = r + 1, x = q[l + r >> 1];
        while (i < j)
        {
            do (i++); while (q[i] < x);
            do (j--); while (q[j] > x);
            if (i < j) std::swap(q[i], q[j]);
        }
        quick_sort(q, l, j), quick_sort(q, j + 1, r);
    }

    int main()
    {
        scanf("%d", &n);
        
        for (int i = 0; i < n; i++)
        {
            scanf("%d", &q[i]);
        }
        
        quick_sort(q, 0, n - 1);
        
        for (int j = 0; j < n; j++)
        {
            printf("%d ", q[j]);
        }
        
        return 0;
    }