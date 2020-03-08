### 思路

dp\[i][j]表示字符串从 i 到 j 的最长回文子序列，从[i,j]向[i-1,j+1]扩展，也就是dp\[i][j]的状态由dp\[i+1][j-1]转化而来；

状态转移方程为：

- s[i]==s[j], dp\[i][j]=dp\[i+1][j-1]+2
- s[i]!=s[j], dp\[i][j]=max(dp\[i][j-1],dp\[i+1][j])

### 代码

```c++
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        int len=s.length();
        int dp[len][len];
        memset(dp,0,sizeof(dp));
        for(int i=0;i<len;i++){
            dp[i][i]=1;
        }
        for(int i=len-1;i>=0;i--){
            for(int j=i+1;j<len;j++){
                if(s[i]==s[j]){
                    dp[i][j]=dp[i+1][j-1]+2;
                }
                else{
                    dp[i][j]=max(dp[i][j-1],dp[i+1][j]);
                }
            }
        }
        return dp[0][len-1];

    }
};
```