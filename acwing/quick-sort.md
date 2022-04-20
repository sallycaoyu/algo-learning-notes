# 快速排序
## 条件
- 已知有一个长度为n的数组q，一个指针l指向q的最左端q[0]， 另一个指针r指向q的最右端q[n-1]

## 方法

1. 确定分界点x(四选一)
    - 左侧：q[l]
    - 中间：q[(l+r)/2]
    - 右侧：q[r]
    - 随机位置

2. 调整区间
    - 把数组q里<=x的数字放在x左侧
    - 把数组q里>=x的数字放在x右侧

3. 递归处理左右两段

## 中心思想：分治
<br>
<br>
<br>

# Quick Sort
## Condition
Now we have:
- an array q with size n

- a pointer l pointing to the leftmost position of q, which is q[0]

- another pointer r pointing to the rightmost position of q, which is q[n-1]

## Steps

1. Determine a split point x from one of the following ways
    - leftmost: q[l]
    - midpoint: q[(l+r)/2]
    - rightmost: q[r]
    - a random position

2. Adjust the left and right sides
    - put elements that <= x to the left side of x
    - put elements that >= x to the right side of x

3. Recursively repeat this for the left and right sides

## Key idea

- Divide and Conquer

