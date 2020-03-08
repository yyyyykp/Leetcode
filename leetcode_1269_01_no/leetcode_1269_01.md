### 思路

令dp\[i][j]表示第 i 步在 j 处的方案数，由于这一状态可通过左边右移一步，右边左移一步，保持不动转移过来。

所以dp\[i][j]=dp\[i-1][j-1]+dp\[i-1][j+1]+dp\[i][j]​.

### 代码

```c++
class Solution {
public:
    int numWays(int steps, int arrLen) {
        int mod = 1000000007;
        int dp[steps+1][steps+1];
        memset(dp,0,sizeof(dp));
        dp[0][0]=1;
        for(int i=1;i<=steps;i++){
            for(int j=0;j<=min(i,arrLen-1);j++){
                dp[i][j] =(dp[i][j]+dp[i-1][j])%mod;
                if(j-1>=0) dp[i][j] =(dp[i][j]+dp[i-1][j-1])%mod;
                if(j+1<=i) dp[i][j] =(dp[i][j]+dp[i-1][j+1])%mod;
            }
        }
        return dp[steps][0];
    }
};
```