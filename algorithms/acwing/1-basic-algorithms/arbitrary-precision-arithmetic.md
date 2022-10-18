# 高精度计算

## 大整数的存储
C++存大整数用数组**倒序**存每一位，这样方便push_back高位

## 两个大整数相加 
- 位数<=10^6

## 两个大整数相减 10^6
- 位数<=10^6

## 一个大整数乘一个小整数
- 大整数位数<=10^6
- 小整数数值<=10^9

## 一个大整数除以一个小整数

## 模板
```
// 高精度加法模板
// C = A + B, A >= 0, B >= 0
vector<int> add(vector<int>& A, vector<int>& B)
{
    vector<int> C;
    
    int t = 0;
    for (int i = 0; i < A.size() || i < B.size(); i++)
    {
        if (i < A.size())
        {
            t += A[i];
        }
        if (i < B.size())
        {
            t += B[i];
        }
        C.push_back(t % 10);
        t /= 10;
    }

    if (t)
    {
        C.push_back(t);
    }
    return C;
}

int main()
{
    string a, b;
    vector<int> A, B;

    cin >> a >> b;
    for (int i = a.size() - 1; i >= 0; i--)
    {
        A.push_back(a[i] - '0');
        B.push_back(b[i] - '0');
    }

    auto C = add(A, B);
    for (int i = C.size() - 1; i >= 0; i--)
    {
        cout << C[i];
    }
    cout << endl;
    return 0;
}

//高精度减法模板
// C = A - B, 满足A >= B, A >= 0, B >= 0
vector<int> sub(vector<int> &A, vector<int> &B)
{
    vector<int> C;
    for (int i = 0, t = 0; i < A.size(); i ++ )
    {
        t = A[i] - t;
        if (i < B.size()) t -= B[i];
        C.push_back((t + 10) % 10);
        if (t < 0) t = 1;
        else t = 0;
    }

    while (C.size() > 1 && C.back() == 0) C.pop_back();
    return C;
}


```