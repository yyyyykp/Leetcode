### 思路

二维前缀和的思想，dp[i]\[j]表示以（0，0）为左上角，（i，j）为右下角的区域和，那么ans[i]\[j]则可以表示为：

ans[i]\[j]=dp[i+K+1]\[j+K+1]-dp[i-K]\[j+K+1]-dp[i+K+1]\[j-K]+dp[i-K]\[j-K]，同时需要注意数组越界问题。

### 代码

```c++
class Solution {
public:
    vector<vector<int>> matrixBlockSum(vector<vector<int>>& mat, int K) {
        int m=mat.size();
        int n=mat[0].size();
        int dp[m+1][n+1];
        memset(dp,0,sizeof(dp));
        vector<vector<int> >ans(m,vector<int>(n));
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                dp[i+1][j+1]=dp[i][j+1]+dp[i+1][j]-dp[i][j]+mat[i][j];
            }
        }
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                int x1 = min(i+K+1,m);
                int x2 = max(i-K,0);
                int y1 = min(j+K+1,n);
                int y2 = max(j-K,0);
                ans[i][j]=dp[x1][y1]-dp[x2][y1]-dp[x1][y2]+dp[x2][y2];
            }
        }
        return ans;
    }
};
```

谁能想到我还写了一个100多行的暴力，比起纯暴力时间上也是有一些优化吧。记录一下看官勿扰

```c++
class Solution
{
public:
    vector<vector<int>> matrixBlockSum(vector<vector<int>> &mat, int K)
    {
        vector<vector<int>> ans = mat;
        int m = mat.size();
        int n = mat[0].size();
        int dp[m][n][4];
        int mark[m][n][4];
        memset(mark, 0, sizeof(mark));
        //下侧和
        for (int j = 0; j < n; j++)
        {
            int t = min(K, m - 1);
            for (int k = 0; k <= t; k++)
            {
                mark[0][j][1] += mat[k][j];
            }
            for (int i = 1; i < m; i++)
            {
                if (i + K < m)
                    mark[i][j][1] = mark[i - 1][j][1] + mat[i + K][j];
                else
                    mark[i][j][1] = mark[i - 1][j][1];
                mark[i][j][1] -= mat[i - 1][j];
            }
        }
        //左侧和
        for (int i = m - 1; i >= 0; i--)
        {
            int t = min(K, n - 1);
            for (int k = 0; k <= t; k++)
            {
                mark[i][n - 1][2] += mat[i][n - 1 - k];
            }
            for (int j = n - 2; j >= 0; j--)
            {
                if (j - K >= 0)
                    mark[i][j][2] = mark[i][j + 1][2] + mat[i][j - K];
                else
                    mark[i][j][2] = mark[i][j + 1][2];
                mark[i][j][2] -= mat[i][j + 1];
            }
        }
        //上侧和
        for (int j = n - 1; j >= 0; j--)
        {
            int t = min(K, m - 1);
            for (int k = 0; k <= t; k++)
            {
                mark[m - 1][j][3] += mat[m - 1 - k][j];
            }
            for (int i = m - 2; i >= 0; i--)
            {
                if (i - K >= 0)
                    mark[i][j][3] = mark[i + 1][j][3] + mat[i - K][j];
                else
                    mark[i][j][3] = mark[i + 1][j][3];
                mark[i][j][3] -= mat[i + 1][j];
            }
        }
        //右侧和
        for (int i = 0; i < m; i++)
        {
            int t = min(K, n - 1);
            for (int k = 0; k <= t; k++)
            {
                mark[i][0][0] += mat[i][k];
            }
            for (int j = 1; j < n; j++)
            {
                if (j + K < n)
                    mark[i][j][0] = mark[i][j - 1][0] + mat[i][j + K];
                else
                    mark[i][j][0] = mark[i][j - 1][0];
                mark[i][j][0] -= mat[i][j - 1];
            }
        }
        //计算右下K个区域内和
        memset(dp, 0, sizeof(dp));
        for (int i = 0; i < m; i++)
        {
            int t = min(K, n - 1);
            for (int k = 0; k <= t; k++)
            {
                dp[i][0][0] += mark[i][k][1];
            }
            for (int j = 1; j < n; j++)
            {
                if (j + K < n)
                    dp[i][j][0] = dp[i][j - 1][0] + mark[i][j + K][1];
                else
                    dp[i][j][0] = dp[i][j - 1][0];
                dp[i][j][0] -= mark[i][j - 1][1];
            }
        }
        //计算右上k个区域内和
        for (int i = 0; i < m; i++)
        {
            int t = min(K, n - 1);
            for (int k = 0; k <= t; k++)
            {
                dp[i][0][2] += mark[i][k][3];
            }
            for (int j = 1; j < n; j++)
            {
                if (j + K < n)
                    dp[i][j][2] = dp[i][j - 1][2] + mark[i][j + K][3];
                else
                    dp[i][j][2] = dp[i][j - 1][2];
                dp[i][j][2] -= mark[i][j - 1][3];
            }
        }
        //计算左上k个区域内和
        for (int i = m - 1; i >= 0; i--)
        {
            int t = min(K, n - 1);
            for (int k = 0; k <= t; k++)
            {
                dp[i][n - 1][3] += mark[i][n - 1 - k][3];
            }
            for (int j = n - 2; j >= 0; j--)
            {
                if (j - K >= 0)
                    dp[i][j][3] = dp[i][j + 1][3] + mark[i][j - K][3];
                else
                    dp[i][j][3] = dp[i][j + 1][3];
                dp[i][j][3] -= mark[i][j + 1][3];
            }
        }
        //计算左下k个区域内和
        for (int i = m - 1; i >= 0; i--)
        {
            int t = min(K, n - 1);
            for (int k = 0; k <= t; k++)
            {
                dp[i][n - 1][1] += mark[i][n - 1 - k][1];
            }
            for (int j = n - 2; j >= 0; j--)
            {
                if (j - K >= 0)
                    dp[i][j][1] = dp[i][j + 1][1] + mark[i][j - K][1];
                else
                    dp[i][j][1] = dp[i][j + 1][1];
                dp[i][j][1] -= mark[i][j + 1][1];
            }
        }
        //求结果
        for (int i = 0; i < m; i++)
        {
            for (int j = 0; j < n; j++)
            {
                //加右下边
                ans[i][j] = dp[i][j][0];
                //加左下边
                ans[i][j] += (dp[i][j][1] - mark[i][j][1]);
                //加右上边
                ans[i][j] += (dp[i][j][2] - mark[i][j][0]);
                //加左上边
                ans[i][j] += (dp[i][j][3] - mark[i][j][2] - mark[i][j][3] + mat[i][j]);
            }
        }
        return ans;
    }
};
```

Ps:数组初始化的区别

```c++
//① runtime error
int dp[m+1][n+1];
在全局状态下才会初始化，其它时候不会
//② fatal error: variable-sized object may not be initialized
int dp[m+1][n+1]={0};
使用变量来定义数组长度时，这个数组可以定义，但是不能同时进行初始化赋值，需要在之后赋值
//③
int dp[m+1][n+1];
memset(dp,0,sizeof(dp));
```

