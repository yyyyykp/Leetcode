### 思路

根据题目意思，这场游戏局一定有赢家，所以先手一定可以走出最佳状态，所以答案全是true。

但是换种问法，先手最多比后手多出多少个石子，还是得dp啦

dp\[i][j]表示在区间[i,j]范围内最多可多出的石子数目值，状态转移的思路可以是：

先计算出相邻1堆的值（即plies[i]本身），根据求出的值再计算相邻2堆的值，再根据求出的值计算相邻3堆的值，以此类推，求出相邻length堆的值。因为玩家只能从两端取，所以可得状态转移方程：

dp\[i][j]=max(piles[i]-dp\[i+1][j],piles[j]-dp\[i][j-1]).

减法是因为从上一个状态转移到下一个状态时，上一个状态代表着当前一位玩家和对手的差值，当状态转移到对手之后，就该用取的堆的石子数减去上一个状态的值。

### 代码

```c++
class Solution {
public:
    bool stoneGame(vector<int>& piles) {
        int length=piles.size();
        int dp[length][length];
        memset(dp,0,sizeof(dp));
        for(int i=0;i<length;i++){
            dp[i][i]=piles[i];
        }
        for(int l=1;l<length;l++){  //遍历区间长度
            for(int j=0;j<length-l;j++){  //变量区间左端点
                dp[j][j+l]=max(piles[j]-dp[j+1][j+l],piles[j+l]-dp[j][j+l-1]);
            }
        }
        return dp[0][length-1]>0;
    }
};
```