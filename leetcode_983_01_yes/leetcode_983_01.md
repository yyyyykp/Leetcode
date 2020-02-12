### 思路

遍历一年的每一天，只在旅行日购买通行证，进而更新未来第 1 天，未来第 7 天，未来第 30 天的消费值。

动态转移方程：

dp[i]表示在第 i 天所需的最低消费，分为两种情况：

1. 当第 i 天是旅行日时，

   dp[i]=min(dp[i],dp[i-1]+costs[0]);

   dp[i+6]=min(dp[i+6],dp[i-1]+costs[1]);

   dp[i+29]=min(dp[i+29],dp[i-1]+costs[2]);

2. 当第 i 天不是旅行日时，

   dp[i]=min(dp[i],dp[i-1]);

需要注意的两个地方是：

1. 对于costs[]的处理，如果为期30的通行证比为期7天的更便宜，就直接买为期30的通行证，即令costs[1]=costs[2]，同理进行处理；

2. 最后处理到365+30，为的是避免一种情况，假如最后一个旅行日是365，但是最小花费在(365,365+30)的区间内（在前边一个旅行日买了为期30天的通行证），这样再往后计算30天就可以更新到最小值了。

   举一个栗子：days=[363,365]，costs=[3,4,4]。

纪念一下100%的耗时😊

![](..\resource\img\983.png)



### 代码

```c++
class Solution {
public:
    int mincostTickets(vector<int>& days, vector<int>& costs) {
        if(costs[2]<costs[1])
            costs[1]=costs[2];
        if(costs[2]<costs[0])
            costs[0]=costs[2];
        if(costs[1]<costs[0])
            costs[0]=costs[1];
        int mark[450];
        int dp[450];
        memset(mark,0,sizeof(mark));
        memset(dp,0x3f3f3f3f,sizeof(dp));
        dp[0]=0;
        int s=days.size();
        for(int i=0;i<s;i++){
            mark[days[i]]=1;
        }
        for(int i=1;i<=365+30;i++){
            if(mark[i]){
                dp[i]=min(dp[i],dp[i-1]+costs[0]);
                if(i+6<=365+30)
                    dp[i+6]=min(dp[i+6],dp[i-1]+costs[1]);
                if(i+29<=365+30)
                    dp[i+29]=min(dp[i+29],dp[i-1]+costs[2]);
            }
            else{
                dp[i]=min(dp[i],dp[i-1]);
            }
        }
        return dp[365+30];
    }
};
```