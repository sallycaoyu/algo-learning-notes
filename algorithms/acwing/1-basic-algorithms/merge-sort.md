# 归并排序
## 中心思想
- 分治
<br></br>

## 条件
- 已知有一个长度为n的数组q，一个长度为n的数组tmp
<br></br>

## 步骤

1. 确定分界点
    - mid = (l+r)/2
2. 递归排序left，right
3. 归并 - 合二为一（难点）

<br></br>

## 解决方案
1. 如果q里只有一个或没有元素，返回
2. 确定分界点
3. 递归排序left，right
4. 归并
    - 设k为tmp里已经有多少个数，i为左半边的起点，j为右半边的起点
    - 当i，j都不超越各自终点时，比较i，j指向的元素大小，将更小的元素插入tmp的下一位，更小的i/j指针向右移一位
    - 如果左半边没循环完，把左半边数字继续插到tmp里
    - 如果右半边没循环完，把右半边数字继续插到tmp里
    - 把tmp里的数复制回q里

<br></br>

## 模板
    void merge_sort(int q[], int l, int r)
    {
        if (l >= r) return;

        int mid = l + r >> 1;
        merge_sort(q, l, mid);
        merge_sort(q, mid + 1, r);

        int k = 0, i = l, j = mid + 1;
        while (i <= mid && j <= r)
            if (q[i] <= q[j]) tmp[k ++ ] = q[i ++ ]; // i++先赋值再自增
            else tmp[k ++ ] = q[j ++ ];

        while (i <= mid) tmp[k ++ ] = q[i ++ ];
        while (j <= r) tmp[k ++ ] = q[j ++ ];

        for (i = l, j = 0; i <= r; i ++, j ++ ) q[i] = tmp[j];
    }

作者：yxc
链接：https://www.acwing.com/blog/content/277/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。