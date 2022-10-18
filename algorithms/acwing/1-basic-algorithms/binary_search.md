# 整数二分
## 中心思想
- 单调性：有单调性一定可以二分，可以二分不一定有单调性
- 只要能满足这样一个性质p：整个区间一分为二，一半满足p，另一半不满足p。二分就可以用来找两半边各自的边界。
- 当l=r（区间长度为1）时得到结果
<br></br>

## 条件
- 中间值mid
- check(mid)判断是否满足性质
<br></br>

## 解决方案
### - 法1: 找左半边upper bound
1. mid = (l + r + 1) / 2
2. 当check(mid)为
    - true：mid满足左半边性质，在答案的左侧或等于答案。答案在[mid, r]中(包含mid)，l=mid
    - false：mid不满足左半边性质，在右半边，答案的右侧。答案在[l, mid-1]中，r=mid-1
<br></br>

### - 法2: 找右半边lower bound
1. mid = (l + r) / 2
2. 当check(mid)为
    - true：mid满足右半边性质，在答案的右侧或等于答案。答案在[l, mid]中，r=mid
    - false：mid不满足右半边性质，在左半边，答案的左侧。答案在[mid + 1, r]中，l=mid+1

## 难点
### mid的取值
- 看解决方案第2步中取值范围修改为l=mid 还是 r=mid
- l=mid（法1）时，mid= (l + r + 1) / 2，向上取整
    - c++为下取整，当l = r - 1，mid不 + 1向上取整时，
        - mid = (l + r) / 2 = (l + l + 1) / 2 = (2l + 1) / 2 = l
    - 当check(mid) = true时，
        - 此时更新l = mid = l，区间不变，会发生死循环
    - 如果mid向上取整，l = r - 1，
        - mid = (l + r + 1) / 2 = (r - 1 + r + 1) / 2 = 2r / 2 = r
        - 此时更新l = mid = r，区间变为[r, r]，不会发生死循环 
- r=mid（法2）时，mid= (l + r) / 2

## 整数二分模板
    bool check(int x) {/* ... */} // 检查x是否满足某种性质

    // 区间[l, r]被划分成[l, mid]和[mid + 1, r]时使用：
    int bsearch_1(int l, int r)
    {
        while (l < r)
        {
            int mid = l + r >> 1;
            if (check(mid)) r = mid;    // check()判断mid是否满足性质
            else l = mid + 1;
        }
        return l;
    }
    // 区间[l, r]被划分成[l, mid - 1]和[mid, r]时使用：
    int bsearch_2(int l, int r)
    {
        while (l < r)
        {
            int mid = l + r + 1 >> 1;
            if (check(mid)) l = mid;
            else r = mid - 1;
        }
        return l;
    }
<br/> <br/>


# 浮点数二分
## 条件
边界由精度确定，所以不同考虑边界不同情况
- 例：r - l <= 1e - 6
## 浮点数二分模板
    # 方法1
    bool check(double x) {/* ... */} // 检查x是否满足某种性质

    double bsearch_3(double l, double r)
    {
        const double eps = 1e-6;   // eps 表示精度，取决于题目对精度的要求
        while (r - l > eps)
        {
            double mid = (l + r) / 2;
            if (check(mid)) r = mid;
            else l = mid;
        }
        return l;
    }

    # 方法2
    bool check(double x) {/* ... */} // 检查x是否满足某种性质

    double bsearch_3(double l, double r)
    {
        for (int i=0; i<100; i++) // 直接循环100次来保证精度：区间长度/2^100
        {
            double mid = (l + r) / 2;
            if (check(mid)) r = mid;
            else l = mid;
        }
        return l;
    }

作者：yxc<br/>
链接：https://www.acwing.com/blog/content/277/<br/>
来源：AcWing<br/>
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。