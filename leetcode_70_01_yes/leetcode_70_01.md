### 思路

简单题，第 i 个台阶可以由第 i-1 个台阶和第 i-2 个台阶的状态转移过来

dp[i]代表爬到第 i 个台阶的方法数

状态转移：dp[i]=dp[i-1]+dp[i-2]



### 代码

```c++
class Solution {
public:
    int climbStairs(int n) {
        int dp[n+1]={0};
        dp[0]=1;
        dp[1]=1;
        for(int i=2;i<=n;i++){
            dp[i]=dp[i-1]+dp[i-2];
        }
        return dp[n];
    }
};
```