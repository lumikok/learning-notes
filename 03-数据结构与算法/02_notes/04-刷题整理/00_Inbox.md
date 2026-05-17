# P 1605. 迷宫

**平台**：[洛谷 P 1605 迷宫](https://www.luogu.com.cn/problem/P1605)；
**题号**：1605  
**难度**：普及-  
**标签**：`#DFS` `#回溯` `#网格`  
**状态**：✅ 已掌握，🔴待二次重做 
 #review
## 📌 一句话题干

给定 N×M 网格（含障碍），从起点到终点，每格只能走一次，统计所有路径总数。[](https://www.luogu.com.cn/problem/P1605?contestId=60878)

## 💡 核心思路

采用 DFS + 回溯算法。递归向四个方向探索，到达终点时方案数 +1，递归返回后撤销标记。

- **深度优先搜索（DFS）**：尽可能深地探索分支，直到无法继续再回溯。
    
- **回溯**：递归返回后撤销访问标记，避免阻塞其他合法路径。
    
- **剪枝**：通过合法性判断（边界、障碍、访问标记）提前排除无效方向，减少搜索量。
    

## 📝 代码（C++）

```cpp
#include <bits/stdc++.h>
using namespace std;
int N, M, T;
int SX, SY, FX, FY;
int ma[7][7];   // 障碍物标记：1 为障碍
int vis[7][7];  // 访问标记：true 为已走过
int ans = 0;
int dx[] = {0, 0, 1, -1};
int dy[] = {1, -1, 0, 0};
void dfs(int x, int y) {
    if (x == FX && y == FY) {
        ans++;
        return;
    }
    for (int i = 0; i < 4; i++) {
        int nx = x + dx[i];
        int ny = y + dy[i];
        if (nx >= 1 && nx <= N && ny >= 1 && ny <= M && !ma[nx][ny] && !vis[nx][ny]) {
            vis[nx][ny] = true;
            dfs(nx, ny);
            vis[nx][ny] = false;  // ⭐ 关键：回溯
        }
    }
}
int main() {
    cin >> N >> M >> T;
    cin >> SX >> SY >> FX >> FY;
    while (T--) {
        int a, b;
        cin >> a >> b;
        ma[a][b] = 1;
    }
    vis[SX][SY] = 1;
    dfs(SX, SY);
    cout << ans << endl;
    return 0;
}
```

## 💥 易错点 & 注意

1. **起点标记**：搜索前必须将 `vis[SX][SY]` 设为 `true`，否则会在起点处无限兜圈。
    
2. **回溯**：`vis[nx][ny] = false` 必不可少——递归返回后若不撤销，该格子会被永久占用，导致其他路径无法经过，最终漏算方案。
    
3. **边界检查**：网格索引从 `1` 开始，注意 `<=` 还是 `<`。
    
4. **小范围暴力**：数据范围 `1 ≤ N, M ≤ 5`，虽是指数级复杂度但完全可行。[](https://www.luogu.com.cn/problem/P1605?contestId=60878)
    

## 📊 复杂度分析

- **时间复杂度**：O(4^(N×M))，指数级，但 N×M ≤ 25，实际可接受。
    
- **空间复杂度**：O(N×M)，即迷宫数组和访问数组的规模。
    

## 🔗 关联知识点

- **知识库链接**：[[../Knowledge/DFS_BFS/深度优先搜索.md | DFS & 回溯]]、[[../Knowledge/DFS_BFS/网格DFS.md | 网格DFS模板]]
    
- **其他经典题**：[[LeetCode/03_DFS_BFS/0200_number_of_islands.md | 200. 岛屿数量]]、[[LeetCode/03_DFS_BFS/0079_word_search.md | 79. 单词搜索]]


# P 1036 [NOIP 2002 普及组] 选数

**平台**：洛谷
**题号**：1036
**难度**：普及-
**标签**：`#DFS` `#组合枚举` `#回溯` `#数学`
**状态**：✅ 已掌握

## 📌 一句话题干
从 n 个整数中任选 k 个相加，求有多少种选法的和为素数。

## 💡 核心思路
通过 DFS/回溯，不重复地枚举出所有从 n 个数中选 k 个的组合，每得到一个组合就计算和并判断是否为素数，最后输出素数的总数。
- **递归/回溯**：这是核心，负责生成所有组合，注意需要参数来保证组合的“不降原则”，防止重复。
- **素数判定**：编写一个辅助函数，用试除法（从 2 遍历到√n）来判断一个数是否为素数。
- **数据规模**：n ≤ 20，允许复杂度较高的算法，这是使用 DFS 的重要信号。

## 📝 代码 (C++)

```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 25;
int n,k;
int arr[N];
int ans = 0;

bool is_prime(int x) {
    for(int i = 2;i * i <= x;i++) {
        if(x % i == 0) {
            return false;
        }
    }
    return true;
}

void dfs(int idx,int selected,long long sum) {
    if(selected == k) {
        if(is_prime(sum)) {
            ans++;
        }
        return;
    }

    if(n - idx + 1 < k - selected) { // 此处出错了
        return;
    }

    dfs(idx+1,selected+1,sum + arr[idx]);
    dfs(idx+1,selected,sum);
}

int main() {
    cin >> n >> k;
    for(int i = 1;i <= n;i++) {
        cin >> arr[i];
    }
    
    dfs(1,0,0);
    cout << ans << endl;
    return 0;
}
```

## 💥 易错点 & 注意

1. **组合去重**：这是重中之重。必须通过 `startIdx` 参数来控制循环，确保每次只从当前位置之后选数，否则会导致重复的组合。
    
2. **素数判定的边界**：`1` 不是素数，`2` 是素数。循环只需到 `sqrt(n)` 即可，记得包含 `<=`。
    
3. **DFS 参数设计**：明确 `startIdx`（可选范围）、`sum`（当前和）、`count`（已选数量）三个参数的作用。
    
4. **输入输出**：使用 `cin` 和 `cout` 即可满足要求。
    

## 📊 复杂度分析

- **时间复杂度**：O(Cnk⋅sum)O(Cnk​⋅sum​)。组合数 CnkCnk​ 在最坏情况下约 C 2010=184756 C 2010​=184756，乘以素数判断的开销，完全可行。
    
- **空间复杂度**：O(n)O(n)，主要是存储数组和递归栈的开销。
    

## 🔗 关联知识点

- **知识库链接**：[[../Knowledge/DFS_BFS/深度优先搜索.md | DFS & 回溯]]、[[../Knowledge/Math/Prime.md | 素数判定]]
    
- **经典题型索引**：组合枚举 / DFS
    

这道题的价值在于，它让你理解了如何用**DFS 去生成“组合”**，而不只是在二维网格中移动。这是算法思维的一个重要跳跃，非常值得记录下来。

出错的点 ： 剪枝边界条件出错了