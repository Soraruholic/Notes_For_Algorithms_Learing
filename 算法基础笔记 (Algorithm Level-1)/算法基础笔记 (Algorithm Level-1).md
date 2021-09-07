# 算法基础笔记 (Algorithm Level-1)

## Chapter 1. 动态规划

### 1.1 数字三角形模型

#### 1.1.1 摘花生 (Acwing 1015)

##### 1.1.1.a Analysis

By applying Y's Method, we can have that :

$$$DP \begin{cases}\text{State Presentation: }f(i,j)\begin{cases}\text{Sample Space (S): }\text{从(1,1)走到(i,j)的所有路线的集合}\\\text{Attribution: }\text{Max\{每一条路线拿到花生的数量\}}\end{cases}\\\text{State Calculation} \begin{cases}\text{Partition: }\text{\{[到达(i,j)的]最后一步是从左侧; 最后一步是从上侧\}}\\\text{Transition: }f(i,j)=max(f(i-1,j), f(i,j - 1)) + w(i,j)\end{cases}\end{cases}$$$.

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
        memset (f, 0, sizeof f);
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



#### 1.1.2 最低通行费 (Acwing 1018)

#### 1.1.3 方格取数 (Acwing 1027)

#### 1.1.4 传纸条 (Acwing 275)

