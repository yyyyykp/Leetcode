### 思路

简单题，第 i 个台阶的状态直接由前两个台阶的状态转化而来。

dp[i]代表到达第 i 个台阶花费的最小值

转移方程：dp[i]=min(dp[i-1]+cost[i-1],dp[i-2]+cost[i-2])

### 代码

```c++
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int length=cost.size();
        int dp[length+1]={0};
        for(int i=2;i<=length;i++){
            dp[i]=min(dp[i-1]+cost[i-1],dp[i-2]+cost[i-2]);
        }
        return dp[length];
    }
};
```