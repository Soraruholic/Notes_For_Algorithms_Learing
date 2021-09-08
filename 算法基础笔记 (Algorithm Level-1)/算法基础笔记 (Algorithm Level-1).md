# 算法基础笔记 (Algorithm Level-1) 

## Chapter 1. 动态规划

### 1.1 数字三角形模型

#### 1.1.1 [ 摘花生 ](https://www.acwing.com/problem/content/1017/)

##### 1.1.1.a Analysis

+ By applying Y's Method, we can have that :

$DP \begin{cases}\text{State Presentation: }f(i,j)\begin{cases}\text{Sample Space (S): }\text{从(1,1)走到(i,j)的所有路线的集合}\\\text{Attribution: }\text{Max\{每一条路线拿到花生的数量\}}\end{cases}\\\text{State Calculation} \begin{cases}\text{Partition: }\text{\{[到达(i,j)的]最后一步是从左侧; 最后一步是从上侧\}}\\\text{Transition: }f(i,j)=max(f(i-1,j), f(i,j - 1)) + w(i,j)\end{cases}\end{cases}$.
>> 由于Original markdown是不支持LaTex的，而且很多plugins也不支持inline LaTex，所以建议在本地浏览，或者直接看下方的图片格式。
>> ![image](https://user-images.githubusercontent.com/73734860/132600830-66f64009-fe44-49be-b77d-1446a0aaa8ab.png)


+ 注意，此题题面说明了不存在"走回头路"的情况。

##### 1.1.1.b Codes

```c++
#include <iostream>
#include <cstring>
#include <cstdio>
#include <algorithm>
using namespace std;
const int N = 107;
int T, n, m;
int f[N][N], w[N][N];
int main (void) {
    scanf ("%d", &T);
    while (T --) {
        scanf ("%d%d", &n, &m);
        for (int i = 1; i <= n; i ++)
            for (int j = 1; j <= m; j ++) {
                scanf ("%d", &w[i][j]);
                f[i][j] = max (f[i - 1][j], f[i][j - 1]) + w[i][j];
            }
        printf ("%d\n", f[n][m]);
    }
    return 0;
}
```



#### 1.1.2 [最低通行费](https://www.acwing.com/activity/content/problem/content/1257/)

##### 1.1.2.a Analysis 

+ 假设不走回头路，则恰好需要$2n-1$步才可以达到终点，而根据题意，商人最少走$2n-1$步，因此得出结论：商人只能不走回头路。从而，这道题和[1.1.1 摘花生]非常相似，区别就在于此题求的是最小值，而且需要对边界进行特殊处理。
+ $DP \begin{cases}\text{State Presentation: }f(i,j)\begin{cases}\text{Sample Space (S): }\text{从(1,1)走到(i,j)的所有路线的集合}\\\text{Attribution: }\text{Min\{每一条路线缴纳的通行费\}}\end{cases}\\\text{State Calculation} \begin{cases}\text{Partition: }\text{\{[到达(i,j)的]最后一步是从左侧; 最后一步是从上侧\}}\\\text{Transition: }f(i,j)=max(f(i-1,j), f(i,j - 1)) + w(i,j)\end{cases}\end{cases}$.
>> 由于Original markdown是不支持LaTex的，而且很多plugins也不支持inline LaTex，所以建议在本地浏览，或者直接看下方的图片格式。
>> ![image](https://user-images.githubusercontent.com/73734860/132600864-59a35d0f-4b3b-426a-923c-d26a641dfd4e.png)

##### 1.1.2.b Codes

```c++
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;
const int N = 107;
int n;
int f[N][N], w[N][N];
int main (void) {
    scanf ("%d", &n);
    memset (f, 0x3f, sizeof f);   //处理边界问题，此处简化代码，可优化。
    for (int i = 1; i <= n; i ++)
        for (int j = 1; j <= n; j ++) {
            scanf ("%d", &w[i][j]);
            if (i == 1 && j == 1) 
                f[i][j] = w[i][j];    //此处简化代码，可优化
            f[i][j] = min (f[i - 1][j], f[i][j - 1]) + w[i][j];
        }
    printf ("%d\n", f[n][n]);
    return 0;
}
```



#### 1.1.3 [方格取数](https://www.acwing.com/problem/content/1029/)

##### 1.1.3.a Analysis

+ $DP \begin{cases}\text{State Presentation: }f(k,i_1,i_2)\begin{cases}\text{Sample Space (S): }\text{从(1,1)走到(i1,k-i1)或者(i2,k-i2)的所有路线的集合}\\\text{Attribution: }\text{Max\{每一组路线(两条)的数字和\}}\end{cases}\\\text{State Calculation} \begin{cases}\text{Partition: }\text{到达(i1,k-i1)和(i2,k-i2)的最后一步:\{右+下;下+右;下+下;右+右\}}\\\text{Transition: }f(k,i_1,i_2)=max(f(k-1,i_1-1,i_2),f(k-1,i_1,i_2-1),f(k-1,i_1-1,i_2-1),\\f(k-1,i_1,i_2)) + (i_1==i_2)?w(i_1,k-i_1):[w(i_1,k-i_1)+w(i_2,k-i_2)]\end{cases}\end{cases}$
>> 由于Original markdown是不支持LaTex的，而且很多plugins也不支持inline LaTex，所以建议在本地浏览，或者直接看下方的图片格式。
>> ![image](https://user-images.githubusercontent.com/73734860/132600892-c873afa9-dd8e-47f8-a03c-b9b5734bfa20.png)

+ 注意此题必须设立三个独立的状态$k,i_1,i_2$，不可以简单的贪心+两 遍DP。另外，还需要判断两个路线有重复部分的情况，由于此题不走回头路，故而两条路线重复的充分必要条件是$(i_1+j_1=i_2+j_2)\and(i_1=i_2)$.

##### 1.1.3.b Codes

```c++
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
using namespace std;
const int N = 15;
int n, x, y, v;
int f[N*2][N][N], w[N][N];
int main (void) {
    scanf ("%d", &n);
    while (scanf ("%d%d%d", &x, &y, &v), x || y || v)
        w[x][y] = v;
    for (int k = 2; k <= n * 2; k ++)
        for (int i1 = max (1, k - n); i1 <= min (k, n); i1 ++)     //防止越界+去除多余循环
            for (int i2 = max (1, k - n); i2 <= min(k, n); i2 ++) {
                int t = w[i1][k - i1];
                if (i1 != i2) t += w[i2][k - i2];
                int &x = f[k][i1][i2];
                x = max (x, f[k - 1][i1 - 1][i2] + t);
                x = max (x, f[k - 1][i1][i2 - 1] + t);
                x = max (x, f[k - 1][i1][i2] + t);
                x = max (x, f[k - 1][i1 - 1][i2 - 1] + t);
            }
    printf ("%d\n", f[n*2][n][n]);
    return 0;
}
```



#### 1.1.4 [传纸条](https://www.acwing.com/problem/content/277/)

##### 1.1.4.a Analysis

+ 同[1.1.3 方格取数]，注意这里n和m的不同几何意义。
+ $DP \begin{cases}\text{State Presentation: }f(k,i_1,i_2)\begin{cases}\text{Sample Space (S): }\text{从(1,1)走到(i1,k-i1)或者(i2,k-i2)的所有路线的集合}\\\text{Attribution: }\text{Max\{每一组路线(两条)的数字和\}}\end{cases}\\\text{State Calculation} \begin{cases}\text{Partition: }\text{到达(i1,k-i1)和(i2,k-i2)的最后一步:\{右+下;下+右;下+下;右+右\}}\\\text{Transition: }f(k,i_1,i_2)=max(f(k-1,i_1-1,i_2),f(k-1,i_1,i_2-1),f(k-1,i_1-1,i_2-1),\\f(k-1,i_1,i_2)) + (i_1==i_2)?w(i_1,k-i_1):[w(i_1,k-i_1)+w(i_2,k-i_2)]\end{cases}\end{cases}$
>> 由于Original markdown是不支持LaTex的，而且很多plugins也不支持inline LaTex，所以建议在本地浏览，或者直接看下方的图片格式。
>> ![image](https://user-images.githubusercontent.com/73734860/132600892-c873afa9-dd8e-47f8-a03c-b9b5734bfa20.png)
##### 1.1.4.b Codes

``` c++
#include <iostream>
#include <cstdio>
#include <cstring>
#incldue <algorithm>
using namespace std;
const int N = 57;
int m, n;
int f[N * 2][N][N], w[N][N];
int main (void) {
    scanf ("%d%d", &m, &n);
    for (int i = 1; i <= m; i ++)
        for (int j = 1; j <= n; j ++)
            scanf ("%d", &w[i][j]);
    for (int k = 2; k <= m + n; k ++)
        for (int i1 = max (1, k - n); i1 <= min (k, m); i1 ++)
            for (int i2 = max (1, k - n); i2 <= min (k, m); i2 ++) {
                int t = w[i1][k - i1];
                if (i1 != i2) t += w[i2][k - i2];
                int &x = f[k][i1][i2];
                x = max (x, f[k - 1][i1][i2] + t);
                x = max (x, f[k - 1][i1 - 1][i2 - 1] + t);
                x = max (x, f[k - 1][i1][i2 - 1] + t);
                x = max (x, f[k - 1][i1 - 1][i2] + t);
            }
    printf ("%d\n", f[m + n][m][m]);
    return 0;
}
```
